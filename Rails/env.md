## 環境構築
### Ruby各バージョンのインストール方法 (homebrewが既にある前提)
- homebrew: Macのパッケージマネージャ（homebrewインストールにはXCodeが必要。XCodeをターミナルから操作するのにCommand Line Toolsが必要）
- rbenv: Ruby環境のインストール・アンインストール・バージョン管理ツール（homebrewでインストール）
1. `brew update`
2. `brew install rbenv`
3. `rbenv install -l`: Rubyの最新バージョン確認
4. `rbenv install <インストールしたいバージョン名>`
5. `rbenv versions`: インストールされているRuby環境の確認
6. `rbenv global <適用したいバージョン名>`
7. `rbenv versions`で適用したいバージョンに`*`ついてれば完了！
8. `ruby -v`で最終確認

### nodeのインストール
- nodeとは：RailsアセットパイプラインのコードをコンパイルするのにJavascriptランタイムが必要。
  - Javascriptランタイム：JavaScriptの実行が開始されて終わるまで必要なことがすべてが詰め込まれている大きな箱。実行環境。そのひとつがNode.js。
-  `nodenv install <version>`
-  `nodenv global/local <version>`で設定

### gem, rbenv使用の注意
- rbenvなどでRubyのバージョンを切り替えている場合、Rubyのバージョンごとにgemをインストールしなおす必要がある
- gemの中にはC言語向けの外部ライブラリを利用するものもあるため、gem installのみでインストール失敗した場合はエラーメッセージをよく読み、gemのREADMEファイルやGitHubのissueなどを参考にして、必要なライブラリをインストールしたり、ドキュメントやissueで提示されているオプションを指定したり対処する

### yarn
- JavaScriptのパッケージマネージャ
- npmより互換性があり優秀
- "yarnコマンドないよ”と言われたら、`npm install --global yarn`
- `yarn`で、package.jsonに記載されたモジュールをインストール

### 環境構築手順
1. プロジェクトのクローン
2. rubyとnodeのバージョンを合わせる
3. `bundle install --path vendor/bundle`
4. `yarn`（package.jsonに記述されたJSパッケージ/モジュールのインストール）
5. `cp config/database.yml.default config/database.yml` → username, passwordをMySQLの自分の設定に書き換え
6. MySQLの起動（`sudo mysql.server start`）
7. Redisの起動（`redis-server`。Ctrl+cで終了。）
 セッションなど有効期限のあるデータを扱う場合
 ランキングデータなど重たいSQLを走らせないといけない処理を扱う場合
8. `bin/rails db:create`
9. `bin/rails db:migrate`
10. `bin/rails db:seed_fu` （初期データを入れるのに◎。すでに存在しているが変更したいレコードだけ更新したり、ファイル単位で実行できたり。`db:seed`だと実行毎に同じデータが登録されてしまう）
11. `bin/webpack-dev-server`（webpackerのサーバーを立ち上げてcssやjsをリアルタイムで反映）
12. `bin/rails s`


#### 環境構築ではまったこと
- `bundle`実行時に、libv8, mini_racerのインストールでエラーが発生
- 結局、`bundle update` → `bundle install`を行った。（※ Gemfile.lockはGitHubにコミットしないよう注意）
- [参考？](https://blog.dnpp.org/libv8_build_still_difficult_in_2021)