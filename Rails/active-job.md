# Active Job
 - Railsにおけるバックグラウンドジョブを動かすための共通インターフェース
    - Railsで提供されるのはジョブをメモリに保持するインプロセスのキューイングシステムだけなので、Railsを再起動するとジョブは全て失われる&Railsを起動してもジョブは実行されない  
      - キューイング：ジョブを列に並べる
    → production環境では永続的なバックエンドが必要！
    - 永続的なバックエンドとして、Sidekiq, Resque, Delayed Jobなどさまざまなキューイングバックエンドに接続できるアダプタが用意されている（sidekiqというのは、”sidekiqというバックグラウンドジョブを動作させるためのフレームワーク”を使えるようにするアダプタ！）

- 参考
  - [Railsガイド Active Job](https://railsguides.jp/active_job_basics.html#active-job%E3%81%AE%E7%9B%AE%E7%9A%84)

## gem

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

- Sidekiqを使うためには、Client, Redis, Serverの３つが必要
  - Client: Railsアプリケーション自身。ジョブを実行する主体
  - Server: Railsと別プロセスとして起動。`bundle exec sidekiq`で実行
  - Redis: Jobをキューイングするために使う。Clientからアクセスできるようにする必要あり

- 参考
  - [Railsガイド Active Job](https://railsguides.jp/active_job_basics.html#active-job%E3%81%AE%E7%9B%AE%E7%9A%84)
  - [sidekiq qiita](https://qiita.com/tatsurou313/items/d3664f8dda05dcd12d56)
  - [sidekiq doc](https://github.com/mperham/sidekiq/wiki/Active-Job)
  - [sidekiqの使い方](https://qiita.com/nysalor/items/94ecd53c2141d1c27d1f)

### redis + sidekiq
- 非同期処理：時間のかかるタスクを「後で処理するリスト」に入れておき、他のタスクを実行しながら「後で処理するリスト」のタスクも並列で行っていく
- 「後で処理するリスト」にはRedisのキューがよく使われる。これから処理していく予定のジョブのデータをRedisに永続化することで、Railsプロセスが落ちたとしてもエンキューされたジョブのデータは失われない
- RailsサーバとRedisとSidekiqの関係：Railsサーバの処理の中で「後で処理するリスト」に任せたいジョブがあれば、Redisで永続化しているキューにジョブを追加（エンキュー）し、Sidekiqがジョブを取り出して（デキュー）実行していく
- ↑これらのキュー操作をさまざまな方法で実行するためにインターフェースなどを定めているフレームワークがActive Job

- config/initializers/sidekiq.rbでは、Redisのロケーションの設定をするために、serverとclientの両方を定義する必要がある。

- routes.rbでは、sidekiqの提供する管理画面（Webアプリケーション）にアクセスできるようにルーティング設定

- config/sidekiq.yml
  - `concurrency`
    ＝１つのSidekiqプロセスで使用するスレッド数。ジョブの同時実行数。（config/database.ymlのpoolをconcurrency+1にする）
  - `queues`: 実行したい順にキューを定義（ここで定義するキューたちは、app/workersのsidekiq_optionsのqueueに指定したID。コントローラでジョブが生成された時にそのジョブが並ぶ列の名前を指定しているイメージ。指定なしだと、キューIDはdefaultに。） 
    - `active_storage_analysis`: Active Storageでファイルがアタッチされると、`ActiveStorage::AnalysisJob`がキューに入る。そして実行されると、ファイルからメタデータを抽出し、blobレコードのmetadataカラムに保存される。現在のバージョン（`config.load_defaults 7.0`）では、ファイルアタッチが行われると専用の`active_storage_analysis`キューに送信される。これによって、ジョブのカスタム優先順位レベルをアプリで設定できるように。
    - `active_storage_purge`: 最初のファイルがアタッチされると、ストレージにアップロードされ、Railsはそのストレージの場所を指すレコードを１件作成。２番目のファイルがアタッチされると同様にストレージにアップロードされるが、Railsはこのレコードを更新して、新しいストレージの場所を指すように変更。最初にアップロードしたファイル（置き換えられ不要になったファイル）については、`ActiveStorage::PurgeJob`が`active_storage_purge`キューに入り、最終的にファイルをストレージから削除する。

    - [Active Storage + Sidekiq](https://techracho.bpsinc.jp/hachi8833/2019_11_14/83047)


- [わかりやすい解説](https://dev.icare.jpn.com/dev_cat/sidekiq/)
- [sidekiq with redis doc](https://github.com/mperham/sidekiq/wiki/Using-Redis#using-an-initializer)

## Active Job + Rspec
- ジョブの実行をテストしたい＆キューに入ったことをテストしたい
  - → `config.active_job.queue_adapter = :test`
- ジョブの実行結果までをテストしたい（＝同期実行したい）
  - → `config.active_job.queue_adapter = :inline`
  - ただし、キューに入ったことが確認できなくなる＆他の非同期のままにしたい処理まで影響
  - → `ActiveJob::TestHelper`を使う方法も
    - ActiveJob::TestHelperをincludeして使えるようになる#perform_enqueued_jobsは、ブロック内のActiveJobを同期実行してくれる。

- [参考](https://qiita.com/upinetree/items/41a2a8fe9e1dd7c291ab)

## ActionMailer + sidekiq
- キューIDは`mailers`（sidekiq.yml）ただし、`mailers`が書かれていない場合は`default`がキューIDに？
- [参考](https://gist.github.com/maxivak/690e6c353f65a86a4af9)