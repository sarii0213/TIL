## has_many関連付け名
- 複数形にしないとエラーになる、というわけではない
- 慣習的にわかりやすい名前をつけてOK（「フォローしている人たち」なら"following" as seen in Twitter）

## with_attached_<model_name>.includes(:user)

- `with_attached_images`メソッド(images=attachment_name)を使うことで、内部的にincludes結合扱いになり、**N+1クエリ問題**を回避できる
- [with_attached_<attachment_name> 中身](https://github.com/rails/rails/blob/2895c6b9a22b856f2ba22e0866524162701886c1/activestorage/lib/active_storage/attached/model.rb#L73)
- ↑ `scope :"with_attached_#{name}", -> { includes("#{name}_attachment": :blob) }` ←スコープの条件式で、関連付けられたblobのincludesを行なっている
- [最新版rails](https://edgeapi.rubyonrails.org/classes/ActiveStorage/Attachment.html)では、`with_attached_`
- has_one_attachedで結ばれているモデル同士以外のコントローラでは、このスコープはそのまま使えないが、スコープの条件式を応用すればOK
- includesメソッドの引数：「配列」「ハッシュ」または「配列やハッシュをネストしたハッシュ」を指定。並列関係なら「配列」で横並び、A->Bに繋がるような関係なら「ハッシュ」または「配列やハッシュをネストしたハッシュ」
- 例１）投稿たちに紐づく画像と投稿ユーザーと、ユーザーに紐づくアバター情報をeager loadingしたい  posts#index
  - →　`Post.all.with_attached_images.includes(:user {avatar_attachment: :blob})` ← ネストしたハッシュ
  -  indexでは、投稿ごとに**１つ**のusers→attachments→blobs→variants テーブルがロードされる
- 例２） 投稿コメントたちに紐づくユーザーと、ユーザーに紐づくアバター情報と、それに紐づくvariant_recordsをeager loadingしたい　　posts#show  
  - →　`@comments = @post.comments.includes(user: { avatar_attachment: {blob: :variant_records } })` 
  - showでは、投稿ごとに**複数**のcomments→users→attachments→blobs→variantsテーブルがロードされる	

### eager loading(一括読み込み)：  
Model.findによって返されるオブジェクトに関連付けられたレコードを、クエリの利用回数をできるかぎり減らして読み込むためのメカニズム。includesメソッドなどでeager loadingを実現。  
includes: Active Recordは指定された全ての関連付けを最小限のクエリ回数で読み込む

https://railsguides.jp/active_record_querying.html#%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91%E3%82%92eager-loading%E3%81%99%E3%82%8B　


## has_many関連付けで追加されるメソッド
- `collection<<(object, ...)`：1つ以上のオブジェクトをコレクションに追加　
- `collection.destroy(object)`：コレクションからオブジェクトを削除（:dependentオプションは無視される）
- `collection_singular_ids`: コレクションに含まれるオブジェクトのidを配列にしたものを返す


## 自己結合
自己結合（self-joining）関連付け：
例）１つのデータベースモデルに全従業員（employees）を格納しておきたいが、マネージャー（manager）と部下（subordinates）の関係も追えるようにしておきたい場合
  
 - マイグレーション
```
create Employee 
 t.references :manager, foreign_key: { to_table: :employees }
```
- モデル
```
class Employee <  ApplicationRecord
	has_many  :subordinates, class_name: “Employee”, foreign_key: “manager_id”
	belongs_to :manager, class_name: “Employee”, optional: true
```
https://railsguides.jp/association_basics.html#%E8%87%AA%E5%B7%B1%E7%B5%90%E5%90%88


## マイグレーションにおいて参照先テーブル名を自動で推定できないカラムを外部キーとして指定

`foreign_key {to_table: :users }`　　  

　↑　 in relationshipテーブルを作成するマイグレーションファイル

FK値(follower_id, followed_id) = usersテーブルのid　　

https://qiita.com/kymmt90/items/03cb9366ff87db69f539

## modelファイルで、関連名と参照先のクラス名が異なる場合
`belongs_to :followed, class_name: 'User'`のように、`class_name`で参照先クラス指定

## inverse_of

双方向の関連付けをRailsで自動認識できない場合に、belongs_toやhas_manyにその関連付けの逆関連付けとなる関連名を`:inverse_of`オプションに指定して、双方向関連付けを明示する必要がある

（:inverse_of を指定しないと、中間レコードの作成ができないケースがあるそう）  
https://sil.hatenablog.com/entry/rubocop-rails-inverse-of
https://railsguides.jp/association_basics.html#%E5%8F%8C%E6%96%B9%E5%90%91%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91　

## validates + inclusion
- `validates :read, inclusion: { in: [true, false] }` 
- inclusionヘルパー：指定の集合に属性の値が含まれているかどうかを検証。集合には任意のenumerableオブジェクトが使える。

## validates + format
- `validates :url, presence: true, format: /\A#{URI::DEFAULT_PARSER.make_regexp(%w[http https])}\z/`
- `URI::DEFAULT_PARSER.make_regexp(%w[http https])`: http, httpsが含まれたURIを検出するための正規表現を生成
- validates formatを使う場合は\A〜\zでくくる（`\A`: 入力の先頭, `\z`：入力の末尾, `//`:正規表現部分を示す）
- `format: { with: /\A#{URI::DEFAULT_PARSER.make_regexp(%w[http https])}\z/ }`のショートカット版が↑

## delegate
- `delegate :title, to: :notification, prefix: true`
- メソッドを異なるクラス間で簡単に使えるようにするマクロ的な使い方ができる
- `prefix: true`で、delegate先のオブジェクトをプリフィックスに設定

