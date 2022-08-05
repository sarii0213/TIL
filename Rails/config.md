## configにて、ヘルパーを生成しない記述を書くワケ 
ヘルパー：view内で行いたい処理を定義するモジュール。ヘルパーを利用することで、viewをシンプルに保てる。  

今回ヘルパーを生成しない理由：
- 不要
- ヘルパーを新規作成すると、コードの汚染に繋がりやすいから非推奨
  - （スコープがコード全体になっており、気づかずにメソッドのオーバーライドをしてしまうことがあり危険）

## バリデーションエラー表示でレイアウト崩れる時の対処
```ruby
config.action_view.field_error_proc = Proc.new { |html_tag, instance| html_tag }
```
デフォルト状態だと、バリデーションエラー表示時に、入力欄がfield_with_errorsクラスを持つdivタグで自動的に囲まれレイアウトが崩れてしまうことがある。それを防ぐために、divタグを生成させない設定が上記。

## rails g controller時に、request specを生成させない方法
config/application.rb
```ruby
config.generators do |g|
      g.test_framework :rspec, request_specs: false
    end
```
これでいけるはず
