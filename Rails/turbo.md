# Hotwire
## 全体像
- Turbo: Hotwireの中核となるJSライブラリ。Turboを使うとJSを書かずにSPAのようなインタラクティブなアプリケーションが作成可能
  - Turbo Drive
  - Turbo Frames
  - Turbo Streams
  - Turbo Native 
- Stimulus: Turboと相性が良いライブラリ。JSにレールを敷く役割。整理するってこと？
- Strada: Hotwireを使ってモバイルアプリケーションを開発する際に利用。未発表の技術のため、Hotwire = Turbo + Stimulusの理解でOK。

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

### Turbo Drive
- 画面遷移を高速化
- 旧名： Turbolinks
- 仕組み
  - リンク・フォームのリクエストをTurbo Driveがインターセプトして、fetchによる非同期リクエストに差し替える
  - そして、レスポンスされたHTMLの<body>要素だけ抜き出して、現在のページの<body>要素を置換する（＋<head>の一部がマージ）
  - 画面遷移しても今のページのCSS・JSをそのまま利用できるため、CSS・JSを初期化してページに適用する処理をスキップできる→画面遷移高速化✨
- 導入方法
  - コードをいじる必要なし！ `turbo_method: `などと書けばよい

### Turbo Frames
- Turbo Driveの部分置換版...画面の一部だけしか更新しない場合に◎
- Turbo Frames: `<turbo-frame>..</turbo-frame>`を置換
  - Turbo Drive: `<body>..</body>` 
  
### Turbo Stream
- 複数箇所のHTMLを要素を同時に更新・追加・削除が可能✨
  - Turbo Framesは一箇所だけ＆更新のみ
- チャットのようなリアルタイムなアプリケーションを作ることも可能
  
### Turbo Rails
  
## 参考
[猫でもわかるHotwire入門 Turbo編](https://zenn.dev/shita1112/books/cat-hotwire-turbo/viewer/abstract)
