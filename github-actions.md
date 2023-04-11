## GitHub Actionsとは

GitHubと連携しやすいCIツール

これから学んでいきたい

https://qiita.com/ham0215/items/41a66a17190dd7c319a7


## GitHub Actionsのまとめ
rubocop, erblint, rspecをGitHub Actionsで回したい

- `name:` GitHub上に表示される名前
- `job`: 同一の`ruuner`上で実行される。`step`の集まり。
- `step`: シェルスクリプトor`action`。同一job内の`step`ではデータを共有できる。
- `action`：`step`内の個々のタスク。GitHub公式のアクションやサードパーティのアクション、独自作成したアクションを使用可能。
    - `uses:`で`action`指定 or `run:`でコマンドを実行
    - GitHub公式の`action`の例：`actions/checkout@v2`
      - ランナー上でリポジトリをチェックアウトし、自身のコードに対してスクリプトやアクションを実行できるようにする。
      - __自身のコードに対してスクリプト・アクションを実行する際はcheckoutアクションは常に必要。__
    - サードパーティの`action`の例：`ruby/setup-ruby@v1`
    - `actions/cache@v2`：実行間で gem のキャッシュを自動的に処理。gem を vendor/cache にインストールするように Bundler が構成されます。 ワークフローの実行が成功するたびに、このフォルダーは GitHub Actions によってキャッシュされ、それ以降のワークフローの実行の際に再ダウンロードされる。
      -　https://docs.github.com/ja/actions/using-workflows/caching-dependencies-to-speed-up-workflows 

- `runner`：ワークフローがトリガーされると実行されるサーバー。各`runner`あたりひとつの`job`を実行
- `bats`：テスティングフレームワーク。Bash Automated Testing System。
- `on:`トリガーを指定。例）`[push]`
- `runs-on:`ランナーを指定。例）`ubuntu-latest`
- `run:`ランナー上で実行したいコマンド
- `ruby-version:` リポジトリのルートに`.ruby-version`があれば、定義不要
- ジョブがDockerコンテナ内で実行されるなら、ポートをホストあるいはサービスコンテナにマップする必要はない
- MySQLコンテナのヘルスチェック：コンテナのヘルスチェックを定義しておかないと、コンテナの準備が整う前にワークフローが進んでしまい、接続エラーなどでジョブが落ちる
- `restore-keys:`cacheのキーと完全一致するものがなかった場合に割り当てられる古めのcacheのキーのリスト（前方一致）。ここに一致するものがあっても、`key`と完全一致していなければ`steps.cache-gems.outputs.cache-hit`はfalse
- 最新版のubuntu(2020.03~)では、MySQLのサービスが自動的にスタートしなくなった [参考](https://github.blog/changelog/2020-02-21-github-actions-breaking-change-ubuntu-virtual-environments-will-no-longer-start-the-mysql-service-automatically/)
- 一旦ubuntu-18.04にしてみる
```
      - name: create test db
        run: |
          printf '#!/bin/sh\nexit 0' > /usr/sbin/policy-rc.dq
          apt update && apt upgrade -y
          apt install sudo
          sudo service mysql start 
```
→だめだった
- ubuntu-latestにはMySQLが入ってるからわざわざ入れなくていいらしい 以下省略してみた
```
services:
      db:
        image: mysql:8.0.31
        ports:
          - 3306:3306
        env:
          TZ: "Asia/Tokyo"
          MYSQL_ROOT_PASSWORD: root
        options: --health-cmd="mysqladmin ping" --health-interval=5s --health-timeout=2s --health-retries=3

        - name: set database.yml
        run: cp -v config/database.ci.yml config/database.yml
```
→だめだった

- [こちら](https://qiita.com/ham0215/items/77876875d960e6dd019b)の環境設定など参考にしたらついに通った！

```yml
name: Test # workflowの名前
on: push # workflowのトリガー
jobs: 
  rspec: # job1
    runs-on: ubuntu-latest # runnerの指定。 workflowがトリガーされると実行されるサーバー。
    services: # service container: ジョブごとに定義可能なDockerコンテナ。同一ジョブ内の処理と通信可能。
      db: # DBサーバー
        image: mysql:8.0.31
        ports:
          - 3306:3306 # ポートを公開することでrunnerと接続　サービスコンテナでポートを設定する必要はないらしい？
        env: 
          TZ: "Asia/Tokyo"
          MYSQL_ROOT_PASSWORD: mysql
        options: >-  # コンテナのヘルスチェック
          --health-cmd="mysqladmin ping -pmysql" 
          --health-interval=5s 
          --health-timeout=2s 
          --health-retries=3   
    container: # ジョブの実行場所
      image: ruby:3.1.2
    env: # job1での環境変数
      RAILS_ENV: test
    steps:
      - uses: actions/checkout@v2
      - name: install chrome
        run: |
          wget ...  # ?
      - name: bundler config
        run: bundle config set path 'vendor/bundle'
      - name: cache gems # gemのキャッシュを自動処理→bundle installが高速化
        id: cache-gems # stepを特定・指定できるようにid付与
        uses: actions/cache@v2
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }} # cacheのキー。runner.os = Linux, Windows, or macOS

      - name: setup bundle
        if: steps.cache-gems.outputs.cache-hit != 'true' # cacheのキーと完全一致するものがあるか否か
        run: |
           bundle install --jobs 4 --retry 3 # 並行して行うダウンロードジョブは４つ。ネットワーク失敗時のリトライは３回。
      - name: setup db schema
        run: |
          bundle exec rails db:create db:schema:load --trace # 実行記録を表示
      - name: run spec
        run: bundle exec rspec
      - name: archive rspec result screenshots
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: rspec results screenshots
          path: |
            tmp/screenshots/
            tmp/capybara/

  rubocop:
    runs-on: ubuntu-latest
    container:
      image: ruby:3.1.2
    steps:
      - uses: actions/checkout@v2
      - name: bundler config
        run: bundle config set path 'vendor/bundle'
      - name: cache gems
        id: cache-gems
        uses: actions/cache@v2
        with:
          path: vendor/bundle
          key: 
          
      - name: setup bundle
        if: steps.cache-gems.outputs.cache-hit != 'true'
        run: bundle install --jobs 4 --retry 3
      - name: run rubocop
        run: bundle exec rubocop

  erblint:
    runs-on: ubuntu-latest
    container:
      image: ruby:3.1.2
    steps:
      - uses: actions/checkout@v2
      - name: bundler config
        run: bundle config set path 'vendor/bundle'
      - name: cache gems
        id: cache-gems
        uses: actions/cache@v2
        with:
          path: vendor/bundle
          key:
          restore-keys:
      - name: setup bundle
        if: steps.cache-gems.outputs.cache-hit != 'true'
        run: bundle install --jobs 4 --retry 3
      - name: run erblint
        run: bundle exec erblint .

```

## 参考
- [公式サイト](https://docs.github.com/ja)
- https://qiita.com/M-Yamashii/items/44d32586a498ffced3aa
- [GitHub ActionsでRuby on Railsのテストを実行する（CIの構築） 2020-12版](https://zenn.dev/masaki_murano/articles/9877e45465ec6d)
- [RailsにGitHub Actionsの導入（Rspec, Rubocop）](https://zenn.dev/ryouzi/articles/cd6857c08e60e7)
- https://docs.github.com/ja/actions/using-workflows/workflow-syntax-for-github-actions
- https://docs.github.com/ja/actions/learn-github-actions/contexts
- https://docs.github.com/ja/actions/using-containerized-services/about-service-containers
- [GitHub Actionsのワークフローで使い捨てのMySQLを利用する（サービスコンテナ）](https://blog.norio.io/it/ci-cd/use-ephemeral-mysql-in-github-actions-workflow/)
- [cacheについて](https://github.com/actions/cache  )
- [chromeのインストール](https://www.google.com/linuxrepositories/)
- [chromeのインストール２](https://askubuntu.com/questions/1032415/what-is-deb-deb-src-stable-xenial-main-in-etc-apt-sources-list)
- [apt-keyは2022年半ばに廃止](https://gihyo.jp/admin/serial/01/ubuntu-recipe/0675)

```bash
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add -

echo 'deb http://dl.google.com/linux/chrome/deb/ stable main' | tee /etc/apt/sources.list.d/google-chrome.list
apt update -y
apt install -y google-chrome-stable libvips
```
https://github.com/actions/runner-images

## rspec + ubuntu + mysql のエラー１
`jobs.rspec.runs-on: ubuntu-latest`だと、ローカルのDockerコンテナ上では問題なくrspecが通るのに、GitHub Actionsでは「mysqlがどうこう」などのエラーが出る不具合が。
`ubuntu-18.04`にすると、エラーが出なくなる！

## rspec + mysql のエラー２
- config/environments/test.rbに`config.active_job.queue_adapter = :inline`を追記
- [エラー解決法](https://github.com/brianmario/mysql2/issues/983)
- [Active Jobについて](https://qiita.com/petertakahashi/items/cb9ae73e5ba3020f4a89)


---
---

## 用語について
- `on`: トリガーを指定。例）`push`
- `job`: 同一の`ruuner`上で実行される。`steps`の集まり。
- `runs-on`: ランナーのOSを指定。例）`ubuntu-latest`
    - ランナー： ワークフローがトリガーされると実行されるサーバー。各ランナーあたりひとつの`job`を実行
- `step`: シェルスクリプトor`action`。同一job内の`step`ではデータを共有できる。
- `action`：`step`内の個々のタスク。GitHub公式のアクションやサードパーティのアクション、独自作成したアクションを使用可能。
    - GitHub公式の`action`の例：`actions/checkout@v2`
    - サードパーティの`action`の例：`ruby/setup-ruby@v1`
    - `uses:`で`action`指定
- `services`: サービスコンテナ。`runner`からサービスコンテナへのアクセスはポート番号を指定。コンテナからサービスコンテナへのアクセスはラベル(今回なら`mysql`)を指定。
- `container`: ジョブの実行場所となるコンテナ
- `uses`: アクションを指定。（＝用意されたものを利用）
例）`actions/checkout@v2`: ランナー上にソースコードをチェックアウトし、自身のコードに対してスクリプトやアクションを実行できるようにする。__自身のコードに対してスクリプト・アクションを実行する際はcheckoutアクションは常に必要。__
- `run`: ランナー上で実行したいコマンド・スクリプト
- `bats`： テスティングフレームワーク。Bash Automated Testing System。

## 実際に作成したGitHub Actionsの設定ファイル

```yml
name: Test

on: push

jobs:
  rspec:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    env:
      RAILS_ENV: test
      DB_HOST: 127.0.0.1 # mysqlの仕様でlocalhostだとソケット通信しようとしてしまう
      DB_PORT: 33060
    services:
      mysql:
        image: mysql:8.0.31
        ports:
          - 33060:3306
        env:
          TZ: "Asia/Tokyo"
          MYSQL_ALLOW_EMPTY_PASSWORD: yes
          BIND-ADDRESS: 0.0.0.0 # 設定しないとホストサーバーから接続できない
        options: --health-cmd="mysqladmin ping" --health-interval=5s --health-timeout=2s --health-retries=3
    container:
      image: ruby:3.1.2

    steps:
      - uses: actions/checkout@v2
      - name: Install chrome
        run: |
          wget -qO - https://dl.google.com/linux/linux_signing_key.pub | apt-key add -
          echo 'deb http://dl.google.com/linux/chrome/deb/ stable main' | tee /etc/apt/sources.list.d/google-chrome.list
          apt update -y
          apt install -y google-chrome-stable libvips
      - name: bundler config
        run: bundle config set path 'vendor/bundle'
      - name: cache gems
        id: cache-gems
        uses: actions/cache@v2
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
      - name: setup bundle
        if: steps.cache-gems.outputs.cache-hit != 'true'
        run: |
          bundle install --jobs 4 --retry 3
      - name: set database.yml
        run: cp -v config/database.ci.yml config/database.yml
      - name: setup db schema
        run: |
          bundle exec rails db:create db:schema:load --trace
      - name: run spec
        run: bundle exec rspec
      - name: archive rspec result screenshots
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: rspec result screenshots
          path: |
            tmp/screenshots/
            tmp/capybara/

  rubocop:
    runs-on: ubuntu-latest
    container:
      image: ruby:3.1.2
    steps:
      - uses: actions/checkout@v2
      - name: bundler config
        run: bundle config set path 'vendor/bundle'
      - name: cache gems
        id: cache-gems
        uses: actions/cache@v2
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
      - name: setup bundle
        if: steps.cache-gems.outputs.cache-hit != 'true'
        run: bundle install --jobs 4 --retry 3
      - name: run rubocop
        run: bundle exec rubocop

  erblint:
    runs-on: ubuntu-latest
    container:
      image: ruby:3.1.2
    steps:
      - uses: actions/checkout@v2
      - name: bundler config
        run: bundle config set path 'vendor/bundle'
      - name: Cache gems
        id: cache-gems
        uses: actions/cache@v2
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
      - name: setup bundle
        if: steps.cache-gems.outputs.cache-hit != 'true'
        run: bundle install --jobs 4 --retry 3
      - name: run erblint
        run: bundle exec erblint .
```

## GitHub Actionsで何を実行しているか
- rspec
    1. ソースコードのチェックアウト
自分のコードに対してスクリプトorアクションを実行するための準備

    2. Chromeのインストール
テストにブラウザを使うため。
`run`の中身：chromeのlinux用認証鍵を取得→鍵のリストファイルに取得した認証鍵を追加→aptのアップデート→chromeのインストール

    2. gemのインストール先のパスを設定（`vendor/bundle`に）

    2. gemのキャッシュの設定
        `bundle install`の高速化が目的。
        - `key`が既存のキャッシュと完全一致した場合：`cache-hit`が`true`になり、`path`に指定された場所`vendor/bundle`にキャッシュファイルが復元される。 
        - 完全一致しなかった場合：`cache-hit`が`false`になり、ジョブが正常に完了後に新しい`key`の値をキーとして持つ`path`内のファイルのキャッシュが作成される。

    2. gemのインストール（キャッシュがなかった場合）

    2. CI用のデータベースの設定ファイルをコンテナにコピー
    2. データベースのセットアップ（DBを新規作成→`schema.rb`からテーブル作成）
    2. rspecを実行
    2. rspec失敗時にスクリーンショットを保存
- rubocop
  1. ソースコードのチェックアウト
  1. gemのインストール先パスの設定
  1. gemのキャッシュの設定
  1. キャッシュがない場合にgemをインストール
  1. rubocopを実行


- erblint
  1. ソースコードのチェックアウト
  1. gemのインストール先パスの設定
  1. gemのキャッシュの設定
  1. キャッシュがない場合にgemをインストール
  1. erblintを実行

## 課題
ソースコードのチェックアウト〜gemのインストールのstepは３つのjobで共通しているため、まとめたい。
参考になりそうな記事：[GitHub Actionsを使い始めて、Stepsの共通化で詰まった話](https://qiita.com/ookawara0817/items/a93b8adf5d38a5cad4de)

## 余談
- "チェックアウト"の意味：リポジトリからドキュメントを取り出すこと。図書館やレンタル店で本などを借りるようなイメージ。（[参考](http://itdoc.hitachi.co.jp/manuals/3020/30203N8100/EN810030.HTM)）

## 参考
- [公式サイト](https://docs.github.com/ja)
- [Workflow syntax for GitHub Actions](https://docs.github.com/ja/actions/using-workflows/workflow-syntax-for-github-actions)
- [はじめてのGitHub Actions](https://qiita.com/M-Yamashii/items/44d32586a498ffced3aa)　←図がわかりやすい
- [GitHub ActionsでRuby on Railsのテストを実行する（CIの構築） 2020-12版](https://zenn.dev/masaki_murano/articles/9877e45465ec6d)
- [RailsにGitHub Actionsの導入（Rspec, Rubocop）](https://zenn.dev/ryouzi/articles/cd6857c08e60e7)
- [Github Actionsの使い方メモ](https://qiita.com/HeRo/items/935d5e268208d411ab5a)
