## lメソッド（localizeのエアリアスメソッド）
- 日時の表示に利用
- 設定方法
  - タイムゾーンの設定（config/application.rb config.time_zone） ←必須
  - ロケールの設定（config/application.rb config.i18n.default_locale）　←必須
  - 日時フォーマット（config/locales/ja.yml ja: time: format: default: ***）
  - gem i18n_generatorsインストール＆実行でja.ymlを自動生成できる

https://qiita.com/jnchito/items/831654253fb8a958e　

## dom_idヘルパー
DOMのid規約に沿った名前を取得

## ハッシュ構文で値が省略可に
※キーと値の文字が一致している場合  　
https://blog.saeloun.com/2021/09/28/ruby-allow-value-omission-in-hash-literals.html 

## コレクションをレンダリング
基本形
```ruby
<%= render partial: "product", collection: @products %>
```
複数形のコレクションを渡してパーシャルを呼び出すと、パーシャルの個別のインスタンスは、パーシャルと同じ名前の変数（アンダースコアなし）を経由してコレクションの個別のメンバーにアクセスできる

<br>
ショートハンド記法（さまざまな条件が重なったときに適用可能）

```ruby
<%= render @products %>
```

## collection_check_boxes モデルからチェックボックスを自動生成
```rb
collection_check_boxes(object, method, collection, value_method, text_method, options = {}, html_options = {}, &block)
```
- collection.each + value_method = inputタグのvalue属性の値
- collection.each + text_method = inputタグのlabelのテキスト要素
- object + method = inputタグのname属性
- object + method + value_method = inputタグのid属性


(`object`のクラスの、`method`の値として存在するもののコレクションのチェックボックスタグを返す。  
インスタンス`object`の`method`の戻り値になるものが、選択された状態として表示される。  
`method`がnilを返す場合、なんも選択されていない状態で表示される。  
`:value_method`と`:text_method`パラメータは、`collection`の各メンバーで呼び出される。  
その戻り値は、それぞれチェックボックスタグの`value`属性とボックス横の表示テキスト。)

例）
```rb
<%= form_with model: @user, url: mypage_notification_setting_path do |f| %>
  <%= f.collection_check_boxes :notification_timing_ids, NotificationTiming.all, :id, :timing_type do |b| %>
  ...
```
- object：`@user` 
- method：`:notification_timing_ids`  
  （`{ on_commented: 1, on_liked: 2, on_followed: 3 }`を、ハッシュ形式でenumの定義取得）Use
- collection：`NotificationTiming.all` （checkboxのvalue値とテキスト要素のコレクション）
- value_method: `:id` (`<object>_<>method>_<id num>`の形でidが振られる)
- text_method：`:timing_type` (enumの文字列)

生成されるinputタグ
```html
<input id="user_notification_timing_ids_1" name="user[notification_timing_ids][]" type="checkbox" value="1" />
<label for="user_notification_timing_ids_1">on_commented</label>
<input id="user_notification_timing_ids_2" name="user[notification_timing_ids][]" type="checkbox" value="2" />
...
```

### ストロングパラメータで配列を扱う
配列を許可する時は空配列を指定する
HTML
```html
<input id="user_notification_timing_ids_1" name="user[notification_timing_ids][]" type="checkbox" value="1" />
```
params （一番目と二番目のチェックボックスが選択された場合）
```rb
user => { notification_timing_ids: [1, 2] }
```
ストロングパラメータ
```rb
params.require(:user).permit(notification_timing_ids: [])
```