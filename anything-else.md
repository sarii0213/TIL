## ルート（相対）パス

db/seeds.rb
```ruby
require './db/seeds/posts'
```
- ルート階層が基準の相対パス
- サーバーでしか機能しない

## Rails + Vue.js
- [初心者用チュートリアル](https://www.techpit.jp/courses/195)
- HTML、CSS、JavaScriptを1ファイルで記述できる
- JS in HTML
- Reactは１ファイルに記述できない ＆ "HTML in JS" & jsxという記法が特徴

## Markdownで画像表示
1. GitHubに画像アップロード
2. `![alt name](url..?raw=true)`と画像URLに`?raw=true`をつければOK

## MySQL環境構築
- `mysql --version`で存在確認
- なければ`brew install mysql`
- 動作確認は`sudo mysql.server start`->`mysql -u root -p`でパスワード入力
- 終了するには、`> exit`->`sudo mysql.server stop`

