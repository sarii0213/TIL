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