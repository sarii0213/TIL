## validates /w sorcery
```ruby
validates :password, confirmation: true, if: -> { new_record? || changes[:crypted_password] }
```
- password_confirmationという仮想的な属性を追加
- バリデーションが適用されるのは、パスワードが新規登録（未保存）された時orパスワードに変更がある時のみ（＝パスワード以外の情報更新時にはパスワード入力省略可）

## validates + inclusion
```ruby
validates :read, inclusion: { in: [true, false] }
``` 
指定の集合に属性の値が含まれているかどうかを検証。

## validates + format
```ruby
validates :url, presence: true, format: /\A#{URI::DEFAULT_PARSER.make_regexp(%w[http https])}\z/
```
http, httpsが含まれたURIを検出するための正規表現を生成し、urlカラムのフォーマットをバリデーション。

## delegate
```ruby
delegate :title, to: :notification, prefix: true
```
メソッドを異なるクラス間で簡単に使えるようにするマクロ的な使い方ができる。`prefix: true`で、delegate先のオブジェクトをプリフィックスに設定。( -> `notification_title`メソッドが使える)
