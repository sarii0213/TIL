## with_attached_images.includes(:user)

`with_attached_images`メソッドを使うことで、内部的にincludes結合扱いになり、N+1クエリ問題を回避できる

eager loading(一括読み込み)：  
Model.findによって返されるオブジェクトに関連付けられたレコードを、クエリの利用回数をできるかぎり減らして読み込むためのメカニズム。includesメソッドなどでeager loadingを実現。  
includes: Active Recordは指定された全ての関連付けを最小限のクエリ回数で読み込む

https://railsguides.jp/active_record_querying.html#%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91%E3%82%92eager-loading%E3%81%99%E3%82%8B　

## has_many関連付けで追加されるメソッド
- `collection<<(object, ...)`：1つ以上のオブジェクトをコレクションに追加　
- `collection.destroy(object)`：コレクションからオブジェクトを削除（:dependentオプションは無視される）

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
　↑　relationshipテーブルを作成するマイグレーションファイル

FK値(follower_id, followed_id) = usersテーブルのid　　

https://qiita.com/kymmt90/items/03cb9366ff87db69f539

## modelファイルで、関連名と参照先のクラス名が異なる場合
`belongs_to :followed, class_name: 'User'`のように、`class_name`で参照先クラス指定

## inverse_of

双方向の関連付けをRailsで自動認識できない場合に、belongs_toやhas_manyに:inverse_ofオプションをつけて双方向関連付けを明示する必要がある  

（:inverse_of を指定しないと、中間レコードの作成ができないケースがあるそう）  
https://sil.hatenablog.com/entry/rubocop-rails-inverse-of
https://railsguides.jp/association_basics.html#%E5%8F%8C%E6%96%B9%E5%90%91%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91　

