## validates /w sorcery
```ruby
validates :password, confirmation: true, if: -> { new_record? || changes[:crypted_password] }
```
- password_confirmationという仮想的な属性を追加
- バリデーションが適用されるのは、パスワードが新規登録（未保存）された時orパスワードに変更がある時のみ（＝パスワード以外の情報更新時にはパスワード入力省略可）
