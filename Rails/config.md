## generator設定
### configにて、ヘルパーを生成しない記述を書くワケ 
ヘルパー：view内で行いたい処理を定義するモジュール。ヘルパーを利用することで、viewをシンプルに保てる。  

今回ヘルパーを生成しない理由：
- 不要
- ヘルパーを新規作成すると、コードの汚染に繋がりやすいから非推奨
  - （スコープがコード全体になっており、気づかずにメソッドのオーバーライドをしてしまうことがあり危険）

### rails g controller時に、request specを生成させない方法
config/application.rb
```ruby
config.generators do |g|
      g.test_framework :rspec, request_specs: false
    end
```
これでいけるはず

### いろいろ自動生成させたくない時
```ruby
config.generators do |g|
      g.helper false
      g.skip_routes true
      g.assets false
      g.test_framework false
      g.decorator false
    end
```
- これで、ヘルパー、ルーティング、css/jsファイル、rspecファイル 、decoratorファイルが自動生成されなくなる、はず。
- [rails generateで余計なファイルを作らない方法](https://study-diary.hatenadiary.jp/entry/2020/07/31/165629)
- [decoratorの自動生成off](https://stackoverflow.com/questions/15071460/how-can-i-disable-the-draper-decorator-generator)
- [configで設定＆rails g 時に設定](https://qiita.com/iamtsujikenta/items/44fcbafe335a40cc59b8)

### ジェネレータ実行時に出る警告について
```
Deprecation warning: Expected string default value for '--test-framework'; got false (boolean).
This will be rejected in the future unless you explicitly pass the options `check_default_type: false` or call `allow_incompatible_default_type!` in your code
You can silence deprecations warning by setting the environment variable THOR_SILENCE_DEPRECATION.
```
　↑ thor gemによるバグらしい。　　
 
[この記事](https://qiita.com/kyokucho1989/items/578c1bb88a1e7e2ec0b8)によるとthorのバージョンを0.19.1にすれば警告出なくなるとのことだったが、Rails7は1.0以上しか使えないため、この解決法は適用できず。  
警告は無視してバグが修正されるのを待つか、警告文にあるように`THOR_SILENCE_DEPRECATION`変数をいじるかの２択かな。

#### 環境変数の設定
- Gemfileのdevelopment, testグループに`gem 'dotenv-rails'`追加 → `bundle`
- .envファイルに設定した環境変数を書いていく
- Dockerコンテナにrailsの環境変数を適用できる！　[参考](https://qiita.com/curry__30/items/f85ab4eaf4ca8164b3b0)
- [thor由来の警告を黙らせる with dotenv gem](https://qiita.com/d0ne1s/items/1ecd114b33e80058215f)


https://qiita.com/iamtsujikenta/items/44fcbafe335a40cc59b8

## バリデーションエラー表示でレイアウト崩れる時の対処
```ruby
config.action_view.field_error_proc = Proc.new { |html_tag, instance| html_tag }
```
デフォルト状態だと、バリデーションエラー表示時に、入力欄がfield_with_errorsクラスを持つdivタグで自動的に囲まれレイアウトが崩れてしまうことがある。それを防ぐために、divタグを生成させない設定が上記。
