# Hotwire
## 全体像
- Turbo: Hotwireの中核となるJSライブラリ。Turboを使うとJSを書かずにSPAのようなインタラクティブなアプリケーションが作成可能
  - Turbo Drive
  - Turbo Frames
  - Turbo Streams
  - （Turbo Native）
- Stimulus: Turboと相性が良いライブラリ。JSにレールを敷く役割。整理するってこと？
- （Strada: Hotwireを使ってモバイルアプリケーションを開発する際に利用。未発表の技術のため、Hotwire = Turbo + Stimulusの理解でOK。）

（Turbo Native & Strada はモバイル開発で使う技術）

## Hotwireの特徴
- フォーム・リンクからのリクエストは全てFetch APIを利用した非同期リクエストになる
- このfetchに対して、サーバーはHTMLをレスポンスする
- ReactやVueを利用したSPAの場合：   
  - fetchに対してサーバーはJSONをレスポンスする
  - そして、JSONを受け取ったクライアントがDOMを構築する
  - このやり方だと、バックエンドとフロントエンドで２つのアプリケーションを作ることになり、同じようなロジックを両者に書く必要が😰
- Hotwireでは、fetchに対してJSONではなくHTMLをレスポンス✨
  - レンダリングはサーバーサイドのみで行うことが可能に
  - 状態管理するのはサーバーサイドだけでいい
  - モデルやバリデーションはサーバーサイドにのみ用意すればいい
  - kaminariやransackのようなビューを構築するgemもそのまま使える
  - Railsアプリケーションの開発の延長線上で、モダンなWebアプリケーションをつくれる！

## Turboとは
- JSを使わずにSPA風のアプリケーションを実現できる

### Turbo Drive　 📄
- 画面遷移を高速化
- 旧名： Turbolinks
- 仕組み
  - リンク・フォームのリクエストをTurbo Driveがインターセプトして、fetchによる非同期リクエストに差し替える
  - そして、レスポンスされたHTMLの<body>要素だけ抜き出して、現在のページの<body>要素を置換する（＋<head>の一部がマージ）
  - 画面遷移しても今のページのCSS・JSをそのまま利用できるため、CSS・JSを初期化してページに適用する処理をスキップできる→画面遷移高速化✨
- 導入方法
  - コードをいじる必要なし！ (`turbo_*`を使う ex) method, confirm,  )
  - Turbo Driveを使いたくない場合： app/javascript/application.jsに`Turbo.session.drive = false`

### Turbo Frames 🔲
- Turbo Driveの部分置換版...画面の一部だけしか更新しない場合に◎
- Turbo Frames: `<turbo-frame>..</turbo-frame>`を置換
  - Turbo Drive: `<body>..</body>` 

- 仕組み
  - クライアント側: ページネーションのリンクをクリックした時に、Turboが通常のリクエストをインターセプトして、fetchを行う（Turbo Driveと同じ）。ただし`<turbo-frame>`内からのリンクなので、リクエストヘッダーに`Turbo-Frame: ID_NAME`を付与する（これでTurbo Frameリクエストになる）
  - サーバー側: サーバー側はリクエストヘッダーに`Turbo-Frame`が存在するとTurbo Frameリクエストだと判断する。Turbo Frameリクエストの場合、高速化のためにレイアウトテンプレート（`application.html.erb`）のレンダリングはせずに、メインテンプレートだけをレンダリングしてレスポンスする。
  - クライアント側: クライアント側はレスポンス（メインテンプレートのレンダリング結果）を受け取り、`<turbo-frame>`のidが一致する`<turbo-frame id="ID_NAME">`部分（一覧部分）だけを置換する。検索フォーム等の一覧以外の部分に関しては、レスポンスはされるが利用されずに捨てられる。

- 導入方法
  - 置換したい箇所を`<%= turbo_frame_tag "ID_NAME" do %>`で囲む
  - 通常は`<trubo-frame>`内からのリンクやフォームのリクエストがTurbo Frameリクエストになる
  - `<trubo-frame>`外のリンク・フォームの場合：`{ data: { turbo_frame: "ID_NAME" } }`と指定すればOK
  
### Turbo Stream  ◽
- 複数箇所のHTMLを要素を同時に更新・追加・削除が可能✨
  - Turbo Framesは一箇所だけ＆更新のみ
- チャットのようなリアルタイムなアプリケーションを作ることも可能
```erb
<%= turbo_stream.append "cats", @cat %>
```
### turbo-rails
- RailsからTurboを便利に使うためのgem
- TurboはJSライブラリであり、Railsには依存しない

### Stimulus ✳️
- Turboを使うと、JSを書かずにサーバーサイドレンダリング＋fetchでHTML要素を更新できるようになる
- RailsのScaffoldで用意される７つのアクション（index, show, new, create, edit, update, destroy）は、JSなしでインタラクティブにできるように
- 結果、ReactやVueを使うのに比べてJSを書く量は劇的に減るが、それでもJSが必要な場合に、Stimulusが用意するレールの上にJSを書くことになる
  
### Hotwireのまとめ
1. 通常の画面遷移： HTMLを丸ごと変える
2. Turbo Drive: `<body>`だけ更新する
3. Turbo Frame: `<turbo-frame>`だけ更新する
4. Turbo Stream: 複数のHTMLを更新する
5. Stimulus: JSを使ってTurboでできない処理をする
  
↓自由度 🆙 ＆ 開発・メンテナンスコスト 🆙  

必要に応じて、段階的に作り込んでいく（1,2,3,,,）のがおすすめ
  
### importmap-rails, jsbundling-rails ❓
- importmap-rails
  - デフォルト 
  - 自前のJavaScriptはES6で書いてバンドルせずにHTTP/2で配信して、サードパーティーのJavaScriptライブラリはCDNから取得する方法 ❓
  - instaクローンではimportmap-railsを使っているっぽい
- jsbundling-rails　（＆cssbundling-rails）
  - `rails new ... --css bootstrap`とするとjsbundling-railsが自動で選ばれる
  - `rails s`を起動するだけではJS・CSSのビルドが自動で行われない
  - `$ bin/dev`(`--css bootstrap`で自動生成されるファイル)を叩けばOK
  - （↑ foremanというプロセス管理のツールを使って、サーバーのプロセスと、JS・CSSの自動ビルドのプロセスを同時に立ち上げる）
  
## 参考
[猫でもわかるHotwire入門 Turbo編](https://zenn.dev/shita1112/books/cat-hotwire-turbo/viewer/abstract)
[importmapとは](https://zenn.dev/takeyuweb/articles/996adfac0d58fb)
[importmap-rails github](https://github.com/rails/importmap-rails)
[Webpackerとjsbundling-railsの比較](https://techracho.bpsinc.jp/hachi8833/2022_03_17/115294)
[jsbundling-rails github](https://github.com/rails/jsbundling-rails)
