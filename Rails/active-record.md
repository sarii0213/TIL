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

## ポリモーフィック関連
[![Image from Gyazo](https://i.gyazo.com/7ad4545cb4f1b8bb8827877f58e453e1.png)](https://gyazo.com/7ad4545cb4f1b8bb8827877f58e453e1)
- SQLアンチパターン
  - 外部キーの設定（外部キー制約）ができないため、テーブルが紐づく対象についてDBレベルでデータの整合性を保証することができない
  - 整合性の保持が完全にアプリケーション側に依存する
    - アプリケーション側での整合性保持：`dependent:`オプション、`uniqueness: true`などのDBバリデーション機能
    - → SQLを直で触ったり、アプリ側でのパリデーションに不備があると、不正な状態でのデータ入力ができてしまう
- ただ、RailsのActiveRecordなどの一部のORマッパーはこのポリモーフィック関連をサポートしているため、Railsを使用する際にはこの方法を使用することを推奨

- ポリモーフィックとは、ダックタイピングの一種
  - ダックタイピング：複数のクラスのインターフェースを統一し、同じように扱えるようにすること
    - → コードを簡潔に、拡張性を保って書くことができる
- Railsのポリモーフィックが便利なのは「一つのモデルを同じインターフェースを持ったものが扱う（ダックタイピングする）」場合
  - モデルを追加する時も、同様のインターフェイス（＝振る舞い、入出力の定義…属性・メソッドのこと？）を持ったモデルを追加するだけでOK.既存コードをいじる必要なし

- 関連付けの方法：インターフェースを設定（↓インターフェース：`imageable`）
  
  ```rb
  class Picture < ApplicationRecord
    belongs_to :imageable, polymorphic: true
  end

  class Employee < ApplicationRecord
    has_many :pictures, as: :imageable
  end

  class Product < ApplicationRecord
    has_many :pictures, as: :imageable
  end
  ```

  外部キーのカラムと型のカラムを両方宣言
  ```rb
  class CreatePictures < ActiveRecord::Migration[7.0]
    def change
      create_table :pictures do |t|
        t.string  :name
        t.bigint  :imageable_id
        t.string  :imageable_type
        t.timestamps
      end

      add_index :pictures, [:imageable_type, :imageable_id]
    end
  end
  ```
  - ↑ `t.references :imageable, polymorphic: true`をcreate_tableブロックに書く方法だと`add_index`不要
    - `:imageable_type`: 紐づけるモデル名(ex. `employee`, `product`)
    - `:imageable_id`: FK値。紐づくインスタンスのID（ex. = `employees.id`）
  - `@employee.pictures` → 写真のコレクションをEmployeeモデルのインスタンスから取得
  - `@picture.imageable` → Pictureモデルのインスタンスから親のインスタンスを取得

- [複数のテーブルに対して多対一で紐づくテーブルの設計アプローチ](https://spice-factory.co.jp/development/has-and-belongs-to-many-table/)
- [Railsのポリモーフィック関連とはなんなのか](https://qiita.com/itkrt2y/items/32ad1512fce1bf90c20b#%E9%96%93%E9%81%95%E3%81%A3%E3%81%9F%E3%83%9D%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF)

## enum
- 列挙型
- モデルの数値カラムに対して文字列による名前定義をマップすることができる
- データ操作用の便利なメソッドも提供
- モデルでのenum定義方法
  - 名前定義（文字列）のみ指定  
    - `enum rate: [ :good, :normal, :bad ]`
    - 配列左から順に0, 1, 2と数値が割り当てられる
  - 名前定義と対応する数値を指定
    - `enum rate: { good: 0, normal: 1, bad: 2 }`
- 一番最初に宣言した値をデフォルト値としてモデルに定義することを推奨
    - migration file `t.integer :rate, default: 0`
- integerでなくboolean型のカラムにenumを定義するとDB更新時エラーになる
  - falseに更新しようとすると、NULLが入り、更新処理中断しロールバックする
- 定義済みのenumの参照方法
  - カラム名に`s`をつければOK
    - `Rating.rates` -> ハッシュ形式でenumの定義取得
      - 定義項目の抽出可能。定義していない項目を取得しようとすると**nilが返る**
- 便利メソッド
  - `.<enum_string>?`： 指定の名前定義か確認メソッド
  - `.<enum_string>!`：　指定の名前定義に更新メソッド
  - `.<enum_string>`：　データベースを検索メソッド（名前定義と同名のscopeが割り当てられている）
    - `.not_<enum_string>`メソッドも使える
- 名前定義重複時の対応
  - `_prefix:_ true`, `_suffix: :sample`など、接頭辞・接尾辞を付与可能
- [Enumってどんな子？使えるの？](https://qiita.com/ozackiee/items/17b91e26fad58e147f2e)