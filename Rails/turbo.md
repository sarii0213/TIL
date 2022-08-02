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
  - 



## 参考
[猫でもわかるHotwire入門 Turbo編](https://zenn.dev/shita1112/books/cat-hotwire-turbo/viewer/abstract)
