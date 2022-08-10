## Decoratorとは
- ソフトウェアのデザインパターンの１つ
- 既存のオブジェクトを新しいDecoratorオブジェクトでラップすることで既存の関数やクラスの中身を直接触ることなく、その外側から機能を追加したり書き換えたりする
- また、既存のクラスを拡張する際にクラスの継承の代替手段として用いられる
- Decoratorというデザインパターンを導入することで、**ビューファイルにロジックを記述しない**といったことができる
- **モデルの肥大化も防げる！**モデルにはDBにアクセスするような処理のみを記載すべし ☝️   
- helperとの違い
  - helper: モデルから独立し直接関係していない描画ロジックを実装するのに用いる  
   　（※グローバル空間で定義されるためアプリケーション内でのメソッド名の重複に注意＆どこに書かれているか見つけずらい）
  - Decorator: 特定のモデルにがっつり関連した描画ロジックを実装するのに用いる   


## Draperとは
- Railsのプレゼンテーション層の役割を担うgem
  -  プレゼンテーション層：viewとモデルの中間に位置し、モデルのメソッドなどを駆使してビューに実装されやすい表示ロジック/フォーマットを記述する役割


## Draperの詳細
- セットアップ：`rails g draper:install`  (`bundle install`後)
- デコレータ作成：`rails g decorator <Model name>`
- `delegate`: 該当Modelのメソッドが使えるようになる
- `h`or`helpers`メソッド: ヘルパーにアクセス・オーバーライドできる
  - 例）` h.rails_storage_proxy_url(image, only_path: true)`:
    - `Rails.application.routes.url_helpers.rails_storage_proxy_url(img, only_path: true)`: プロキシモードでvariant URLを取得
    - [参考](https://www.appsloveworld.com/ruby/100/183/rails-activestorage-how-to-get-variant-url-when-in-proxy-mode)
    - コントローラー内でリダイレクトする場合は *_url 形式のヘルパーを使う。(エンジン毎に別ドメインで運用していた場合などに、ドメインやプロトコルを含めたフルの URL で正しくリダイレクトする。)
    -  `rails c`で`Rails.application.routes.url_helpers.method(:rails_storage_proxy_url).source_location`で定義場所見つけられた
    -  url_for（コントローラやアクションなどからURLを生成）ヘルパーによって生成されたものらしい
    -  コントローラ：[ActiveStorage::Blobs::ProxyController ](https://edgeapi.rubyonrails.org/classes/ActiveStorage/Blobs/ProxyController.html)
    -  :only_pathがtrueなら、URL全体ではなくパス部分を返す	
    -  https://github.com/rails/rails/blob/9b138decf5c9409ded5519ef2c1494d53d5a020a/actionpack/lib/action_dispatch/routing/route_set.rb#L632
    -  String#partition: [最初のセパレータより前の部分, セパレータ, それ以降の部分] の 3 要素の配列を返す  
    -  以下の場合のセパレータ： (?<!/)/ ＝ 否定後読み → //はスルーして、１個目の/のみマッチ　　→　相対パスが取得できる 　？
    -  [否定後読み・否定先読みの説明](https://www.javadrive.jp/regex-basic/writing/index2.html#section5)
      ```
      if only_path
            "/" + url.partition(%r{(?<!/)/(?!/)}).last
      ```


## 参考
- [Decoratorの役割とDraperについて](https://qiita.com/jonson29/items/00077b54bb91ed74fdb8)
- [Draperの使い方 まとめ](https://nekorails.hatenablog.com/entry/2018/10/13/070437)
- [Rails Viewの表示のためにDecoratorを用意してHelperとModelを助ける](https://qiita.com/shizuma/items/ce9e863e9c94e88e0a57)
