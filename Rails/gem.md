## よく使うgem
- capybara: テスティングフレームワーク。Webサイト上のユーザー操作をシミュレート。
- sorcery: 認証機能
- erb-lint: erbファイルを整える
- annotate: テーブル情報をモデルに出力
- pagy: ページネーション（kaminariより良い？）
- pre-commit: コミット直前に走らせたいスクリプトを定義（選択肢に限りがあるため、pre-commitファイルを自作するのも◎）
- ransack: 検索機能
- lipvips: 画像処理（other choices: ImageMagick, GraphicsMagick, OpenCV） [詳細](https://tech.medpeer.co.jp/entry/2020/07/30/100000)
- mysql2: MariaDBを使えるようにするgem。(railsに標準搭載gem)
  - MariaDB: MySQLから派生したオープンソースのシステムで、MySQLと互換性がある＆より高速であることが多い。  [詳細](https://www.integrate.io/jp/blog/mariadb-vs-mysql-everything-you-need-to-know-ja/#what)
- draper:  ヘルパーをオブジェクト指向的に書けるようにするgem。"デコレータ"。
- letter_opener_web: 開発環境でメール送信機能の挙動をブラウザでチェックできる
- config: 定数管理
- redis: データ永続化・セッション＆キャッシュ処理の高速化
- sidekiq: ジョブの非同期処理（バックエンドでのジョブキュー管理にredisが必要）

## 各gemについて詳細

### ransack
- `distinct: true`  
  例えば、画像にコメントができるようになっていたとします。（画像対コメントが１対多になっていたとする）  
  この時、コメントを検索する時に画像が一覧で表示されるようになっていたとします。  
  同じ画像に同じワードを使ったコメントがあった場合、distinct: true がない場合同じ画像が２つ表示されてしまいます。  
  distinct: true を使っていれば、重複を削除しているので画像は１つのみの表示になります！  
  https://toomeeto.hatenablog.com/entry/2019/07/05/110624

### RuboCop
- `# rubocop:disable Metrics/MethodLength`: RuboCopにスルーしてもらいたい箇所を適用ルール指定して囲む（`# rubocop:enable <same rule>`までがdisable範囲）

### letter_opener_web
- [使い方](https://qiita.com/tanutanu/items/c6193c4c2c352ac152ec)
- [Mounting the engine](https://guides.rubyonrails.org/engines.html#mounting-the-engine)

### config
- `bundle` -> `rails g config:install`
- [使い方](https://qiita.com/tanutanu/items/8d3b06d0d42af114a383)
- 定数管理の意義  
  - 環境ごとに違う定数を用いたい
  - いろいろなところに散らばる定数をまとめたい
  - 大規模アプリケーションの構築時
- configの特徴：yml形式で定数管理


### redis
- 高速に値をRead/WriteできるNoSQL

- インメモリ型データ構造ストア
  - メモリ上で動作するキーバリューストア型のDB
  - 全てのデータをメモリ上に格納し、各アプリケーションからの高速アクセスが可能

- データ永続化機能
  - メモリ上のデータは揮発性。何もせずに再起動すると全て消える
  - Redisはメモリを利用するが、メモリ上のデータを任意のタイミングでディスクに格納して保持する仕組み
    - 仕組み①：SAVE/BGSAVEは、DBのフルダンプ（RDB = Redis DataBase）をスナップショットとして保存する方法
    - 仕組み②：AOF（Append Only File）という実行時の全コマンドを記録しておく方法

- レプリケーション機能
  - マスター・スレーブ型レプリケーションの仕組みを持つ
  　（１つのマスターに対し複数のスレーブを作成し、スレーブはマスターとデータの同期を行いながら読み込みに応答する。負荷分散に。）

- Redisのメリット
  - Webアプリケーションの高速化
    - インメモリのNoSQLデータベースのため、RDBよりも処理速度が高速
    - Webアプリケーションのセッションやキャッシュの一時的な保存先に指定することで、アクセス速度が向上
  - アトミック（原子性）
    - アトミックな操作：複数の操作が全て実行されるかあるいは全く実行されないと保証される性質のこと（ずっとも）
    - コマンド実行・トランザクションもアトミックなので、不整合が生じない
  - データ型のサポート
    - 一般的なキーバリューストアにはデータ型がない（json, csv）
    - Redisには、Strings, Lists, Sets, Hashes, Sorted setsと５つの型がある

- デメリット
  - メモリ上で動作するため、常にメモリを消費
  - 大容量のデータを一度に扱う処理はNG
  - メモリの断片化（大量のデータの書き込み・削除を繰り返すとパフォーマンスが低下）
  - データの揮発性 
    - RDB・AOFの２種類の永続化機能はあるが、インメモリデータベースという性質上、揮発性がある
  
- 用途
  - セッション管理・キャッシュ
  - リアルタイム更新
    - 商品・ユーザー・コンテンツなどのデータをリスト化した状態で保持し、変化があれば更新やソートを行う。あるユーザーのアクションをWebサイトの他のデータと連携・反映するような仕組みを作る時に有用
  
- 活用事例
  - Pinterestのフォロー管理
  - Slackのジョブ管理
  - Airbnbの問題監視

- [参考](https://agency-star.co.jp/column/redis)


### sidekiq
- = Active Jobのアダプタ ? 
  - Active Job: 
    - Railsにおけるバックグラウンドジョブを動かすための共通インターフェース
    - Railsで提供されるのはジョブをメモリに保持するインプロセスのキューイングシステムだけなので、Railsを再起動するとジョブは全て失われる&Railsを起動してもジョブは実行されない  
      - キューイング：ジョブを列に並べる
    → production環境では永続的なバックエンドが必要！
    - 永続的なバックエンドとして、Sidekiq, Resque, Delayed Jobなどさまざまなキューイングバックエンドに接続できるアダプタが用意されている（sidekiqというのは、”sidekiqというバックグラウンドジョブを動作させるためのフレームワーク”を使えるようにするアダプタ！）


- 参考
  - [Railsガイド Active Job](https://railsguides.jp/active_job_basics.html#active-job%E3%81%AE%E7%9B%AE%E7%9A%84)
  - [sidekiq qiita](https://qiita.com/tatsurou313/items/d3664f8dda05dcd12d56)
  - [sidekiq doc](https://github.com/mperham/sidekiq/wiki/Active-Job)

### redis + sidekiq
- 非同期処理：時間のかかるタスクを「後で処理するリスト」に入れておき、他のタスクを実行しながら「後で処理するリスト」のタスクも並列で行っていく
- 「後で処理するリスト」にはRedisのキューがよく使われる。これから処理していく予定のジョブのデータをRedisに永続化することで、Railsプロセスが落ちたとしてもエンキューされたジョブのデータは失われない
- RailsサーバとRedisとSidekiqの関係：Railsサーバの処理の中で「後で処理するリスト」に任せたいジョブがあれば、Redisで永続化しているキューにジョブを追加（エンキュー）し、Sidekiqがジョブを取り出して（デキュー）実行していく
- ↑これらのキュー操作をさまざまな方法で実行するためにインターフェースなどを定めているフレームワークがActive Job
- [わかりやすい解説](https://dev.icare.jpn.com/dev_cat/sidekiq/)