# 🐳 Docker

## よく使うコマンド
- `docker compose up` ：　コンテナ起動（Ctrl+cで終了）
- `docker compose up -d`　: バックグラウンドで起動
- `docker ps`　: 起動しているコンテナ一覧
- `docker attach <CONTAINER_NAME>`　: Ctrl+ p -> q　でdettach。exitで停止
- `docker compose exec <SERVICE> <COMMAND>`　: 指定したservice （docker-compose.ymlに記載。webなど) の中で、指定したcommand （bashなど） を実行
- `docker stop <CONTAINER_NAME>`：コンテナ停止
- `docker compose down`: コンテナ停止 & `docker-compose up`で作成したコンテナ、ネットワーク、ボリューム、イメージを削除
<br>


## Dockerについて理解
### 参考
[実践 Docker - ソフトウェアエンジニアの「Docker よくわからない」を終わりにする本](https://zenn.dev/suzuki_hoge/books/2022-03-docker-practice-8ae36c33424b59/viewer/1-1-readme)
### サーバ仮想化とは
- 1台のマシンに、ネットワークやストレージを仮想的に用意して、複数の異なるサーバが動いているように見せかける技術
- 物理的なサーバとソフトウェアの間に仮想的なソフトウェアを挟むことで実現
- 仮想化３種類
  - ホスト型仮想化：ホストOS（Mac OS）にインストールされ、ゲストOS(Ubuntu)を管理する
  - ハイパーバイザー型仮想化：ハードウェア(MacBook)にインストールされ、ゲストOS(CentOS)を管理する
  - コンテナ型仮想化： ホストOSにインストールされ、アプリケーション（php, mail system, mysql）を管理する（１コンテナ１アプリケーション）
- コンテナ型仮想化の特徴
  - 起動が早い（ゲストOSがないため）
  - デプロイがしやすい（デプロイ先がコンテナ型なら手間いらず。そうでなくても、１コンテナ１アプリケーションなので移行の手間少ない）
  - ホストOSの違いがコンテナに影響与えることも（コンテナのカーネル＝ホストマシンのカーネル。MacBook IntelとM1で衝突したり、UbuntuコンテナとWindowsコンテナが共存不可だったり）

### Dockerとは
- コンテナ型仮想化を用いてアプリケーションの開発や配置を行うためのプラットフォーム
- Docker Engine: コンテナ型仮想化ソフトウェア（コンテナを載せる部分）。Linuxで動く。
- Docker CLI: コマンド。Docker Engineに提供され、`docker run`のような`docker`ではじまるコマンドで Docker に命令をする。
- Docker Desktop: Windows/MacでDockerを使うときにインストールするDocker一式が入ったGUIアプリケーション。Docker EngineやLinuxのカーネルが含まれているため、Linux以外のOSでもDocker Engineを動かすことができる。（Docker ComposeやKubernetesも含む）
- Docker Compose: Docker CLI（`docker`コマンド群）をまとめて実行してくれる便利なツール。複雑なコマンドを、Yamlファイルを書くことで実現できる。
- Docker Hub: DockerのイメージレジストリであるSaasサービス。イメージのGitHubのようなもの。インストールしたり、アップロードしたり。
- デプロイ時
  - ECS/GKE:ECS(Amazon Elastic Container Service)とGKE(Google Kubernetes Engine)は、コンテナ管理サービス。Docker Engineの入ったLinuxのことで、ローカル環境で使ったコンテナをそのままデプロイできる場所。
  - ECR/GCR: ECR(Amazon Elastic Container Registory)とGCR(Google Container Registory)は、非公開のイメージのレジストリ。プライベートなDocker Hub。商用サービスを公開したくないけど、レジストリに登録しないとデプロイできない場合などに利用。
- コンテナ運用（インフラ構築）
  - Kubernetes: 多数のコンテナを管理するオーケストレーションソフトウェア。  

### 疑問
- コンテナはホストOSのカーネルを利用する？でもMac OSのカーネルはXNUでLinuxではないよね？Docker Desktopに入っているLinuxカーネルと、ホストOSのカーネルはどう作用しあうの？

### Dockerの基本要素３つ
- コンテナ
- イメージ
- Dockerfile

#### コンテナ
- 特定のコマンドを実行するために作られる ホストマシン上の隔離された領域（LinuxのNamespaceにより、PIDはホストマシンのものと衝突しない）
- ホストマシンの１プロセスにすぎない。仮想サーバではない。
- イメージをもとに作られる
- DockerのCLIやAPIを使って、生成・起動・停止を行える
- 複数のコンテナは互いに独立し、影響を与えない
- Docker Engineの上ならローカルマシンでも仮想マシンでもクラウド環境でも動かせる

### イメージ
- コンテナの実行に必要なパッケージで、ファイルやメタ情報を集めたもの
- 複数のレイヤーからなる
- イメージに含まれる情報
  -　ベースはなにか　（ex. Ubuntu）
  -　なにをインストールしてあるか (ex. PHP)
  -　環境変数はどうなっているか
  -　どういう設定ファイルを設置しているか (ex. php.ini)
  -　デフォルト命令はなにか  
-　Docker Hubで公開されているものから、検索して選ぶ
  -　（＝`image pull`はせず、`container run`で`<image>`に`REPOSITORY:TAG`指定。`TAG`省略＝`latest`）
  -　Tagページのバージョンを選んだ画面で見れるのはDockerfileの中身ではなく、Dockerfileによって積み上げられたレイヤーの情報

### Dockerfile
- 既存のイメージにレイヤーをさらに積み重ねるためのテキストファイル
- 公開されているイメージ(ex. Ubuntu&PHP&.ini)にDockerfileでレイヤー(ex. Git)を乗せる感じ
- GitHubで共有するなどして、チームメンバーで同一のコンテナを構築できる
- Dockerfileの有用性
  - コンテナ内で行った操作は、コンテナ終了とともに全てなかったことになる
  - Docker Hubにあるイメージは、レイヤーが最低限しか積み上がっていない
  - 公式イメージでは十分なセットアップを得られない場合に、あらかじめ必要なセットアップを済ませたイメージを自分で作成するのが◎ 

#### Dockerfile の命令
- `FROM`: ベースイメージを指定
- `RUN`: 任意のコマンドを実行しレイヤーを確定
  - 例） `apt-get install mariadb-client tzdata libvips`:MariaDB, タイムゾーンDB, libvipsをインストール
- `COPY`: ホストマシンのファイルをイメージに追加
- `CMD`: デフォルト命令を指定


### Docker 基本コマンド　（最初の`docker`省略）
#### container 操作
- `container run [option] <image> [command]`: イメージからコンテナ起動　（=`image pull`&`container create`&`container start`）
  - `--rm`オプション： コンテナが停止したとき自動削除
  - `--detach`:バックグラウンドで実行
  - `--name`: コンテナ名指定
  - `--interactive`: コンテナの標準入力に接続（対話操作）
  - `--tty`: 疑似ターミナルを割り当てる（対話操作）　（tty: 接続端末の名札のようなもの）
  - `--platform`: Docker Hubでのサポートプラットフォームと異なるプラットフォームを指定したい時（M1Macなら必須）
  - `--env`: コンテナに環境変数を設定
  - `--mount type=<bind/volume>, src=<dir_of_host_machine/volume_name>, dst=<dir_of_container>`:マウントする（丁寧に指定＆Docker Composeに転用しやすい）
  - `--volume`:マウントする（短く指定）
  - `--publish <host machine>:<container>`: コンテナのポートをホストマシンに公開する
  - `--network`: コンテナをネットワークに接続
  - `--network-alias`: コンテナにネットワーク内でのエイリアスを設定
  - `[command]`: デフォルトの命令ではなく、任意の命令を実行させる

- `container exec [option] <container> <command>`: 起動中のコンテナに命令を送る（コンテナ内で実行するLinuxコマンド）
  - `container run`でコマンド実行するのとは違う動きになる
    - `container run ubuntu:20.04 cat hoge.txt`と２度実行すると、同じイメージから異なるコンテナが起動する
    - 対話操作(`bash`コマンド実行)には、`container run`時と同様のオプションが必要(`-it`)
  - コンテナの中にあるログを調べたり、Dockerfileを書く前にbashでインストールコマンドを試し打ちしたり、mysqlを直接操作したりできる

- `container ls`: コンテナ一覧
- `container stop <container>`: コンテナ停止
- `container rm <container>`: コンテナ削除（`-f`で停止＆削除）

#### image 操作
- `image build [option] <path>`: Dockerfileからイメージ作成（その後、`container run`）
  - `--file`オプション：複数のDockerfileを使い分けるとき（`./Dockerfile`以外のDockerfile指定時）に、Dockerfileを指定
  - `--tag`: 人間が把握しやすいように、ビルド結果にタグをつける 　（`REPOSITORY:TAG`の形）
  - `<path>`: `COPY`で使うファイル指定時の相対パス
- `image ls`: イメージ一覧(`image build` or `container run`の過程で取得されたイメージたち)
- `image history [option] <image>`: イメージのレイヤーを確認

#### compose 操作
- `compose up`: コマンドの手順書(Yamlファイル)に書かれたコマンド(`container run <image>`..)をまとめて実行して起動

### コンテナについて
- コンテナは、メインプロセス（`PID`=1）を実行するために起動する　　　（メインプロセス＝デフォルト命令？）
- コンテナが停止するのは、
  - コンテナを停止させたとき(`container stop`) or 
  - メインプロセスが終了した時（`container run nginx:1.21 ls /etc/nginx`だと即時終了）

### Dockerfileをゼロから書く場合
- 「ベースイメージをただ起動して`bash`で試す」（Dockerfileは書かず、`container run --rm ruby:3.1.1 bash`と打つ）
- 「そこで動いたコマンドを Dockerfile にペーストする」というサイクルに

### ボリューム
- コンテナのデータをコンテナ削除とともに消失させないためのもの
- コンテナ内のファイルをホストマシン上でDockerが管理してくれる仕組み
- ホストマシン側のどこに保存されているかは関心がなく、とにかくデータを永続化したい時に有用（DBコンテナなど）
- コンテナでの変更がホストマシンに影響しない
- `docker volume create [option]`
  - `--name`:ボリューム名を指定
- `docker volume inspect <volume name>`: ボリュームの詳細確認
  - `Mountpoint`:データのある場所の実体。Docker DesktopのLinux上のパスのため、直接読み書きはできない。 

### バインドマウント
- ホストマシンの任意のディレクトリをコンテナにマウントする仕組み
- ホストマシンとコンテナ双方がファイルの変更に関心がある場合に有効（ソースコードの共有などに活用）
- コンテナでの操作（削除など）がホストマシンに波及するため、なるべくボリュームを使って、どうしても必要ならバインドマウントを使うのが◎
- 処理が遅くなりやすい
  - 処理時間短縮のために、マウントオプションを利用するとよい
  - それなりの性能を求めつつ、ホスト側の変更をコンテナに反映したい場合は`cached`オプションを使う◎ 
  - [detail](https://gist.github.com/yallop/d4af9dd1bb33ae61d48adf86692cdf9e)

#### COPYとバインドマウントの使い分け
- COPYとバインドマウントはどちらもホストマシンのファイルをコンテナで扱えるようにする機能
- COPY（to イメージ）
  - `image build`をするタイミングでファイルを含めるため、コンテナが起動すればファイルが存在する
  - 元ファイルの変更を行なってもコンテナには反映されないため、`image build`の再実行が必要に
  - 用途：設定ファイルや本番デプロイ時のソースコードなど
- バインドマウント(to コンテナ)
  - ホストマシンでファイルを変更するとコンテナに即時影響
  - 用途：開発時のソースコード（ホストマシンで変更したいがコンテナに即時反映させたい）や初期化クエリ（イメージ配布時点では用意できない）

  - 用途：開発時のソースコード（ホストマシンで変更したいがコンテナに即時反映させたい）や初期化クエリ（イメージ配布時点では用意できない）

### 疑問
- インスタクローンはバインドマウントしている？ソースコードはバインドマウントしているんだよね？どこで確認できる？
- ボリュームを利用している：docker-compose.ymlの記述では、mysql_data（DBのデータ）とbundle（gemのデータ）というボリューム名でボリュームを作成している。

### ポートの公開
- コンテナにホストマシンからアクセスするため　（ホストマシンーコンテナの通信）
- コンテナはホストマシンから隔離されており、コンテナ内でWebサーバやDBサーバを起動してもそのままではホストマシンからアクセスすることはできない
- `--publish host-machine:container`の、`host-machine`側のポートは起動する人が自由に決められる（1024未満は特権ポートなのでNG）


### ネットワーク
- コンテナ同士の通信を可能にする
- ネットワークドライバ：
  - ブリッジネットワーク：同一のDocker Engine上のコンテナが互いに通信をする場合に利用
  - オーバーレイネットワーク：異なるDocker Engine上のコンテナが互いに通信をする場合に利用（ローカル開発では起こり得ない）
- デフォルトブリッジネットワーク：
  - すべてのコンテナ間をリンクする操作が必要
  - コンテナ間の通信はIPアドレスで行う
  - Docker Engine上のすべてのコンテナに接続できてしまう
- ユーザ定義ブリッジネットワーク
  - 相互通信をできるようにするには同じネットワークを割り当てるだけでよい
  - コンテナ間で自動的にDNS解決を行える
  - 通信できるコンテナが同一ネットワーク上のコンテナに限られ、隔離度が上がる
- `docker network create [option] <name>`：ユーザ定義ネットワークを作成

### Docker Compose
- `image build`の手順と`container run`の手順を置き換える
- コンテナの起動を楽にするツールであり、イメージを作るツールではないので、Dockerfileは必須
- `docker compose up [option]`
  - `--detach`: バックグラウンドで実行（ターミナルが固まるのを避ける）
  - `--build`: コンテナを起動する前にイメージをビルド（Dockerfileの変更を反映）
- `docker compose down [option]`：サービスを停止

#### docker-compose.yml
- `services:`の直下に、管理するサービス（＝コンテナ）を任意の名前で記述
- `container_name:`には`container run --name`でしたコンテナ名を置き換える
- `build:`は、`image build --file dockerfile <path>`で指定していた`dockerfile`を`build.dockerfile:`に、`<path>`を`build.context:`で置き換える
- `image:`は、`container run <image>`で指定していたイメージを置き換え
- イメージのビルドは、`compose up`をしたがイメージが存在しなかった場合 or `compose up`に`--build`オプションをつけて実行した場合に行われる
- イメージの取得は、`compose up`をしたがイメージが存在しなかった場合に行われる
- `environment:`は、`container run --env`で指定していた環境変数を置き換える
- `volumes`（１階層目）: `volume create`で行っていたボリュームの作成を置き換える
- `volumes`（２階層目）：`container run --mount`で指定していたボリュームのマウントを置き換える
  - （「`--mount`オプションの指定方法と似ている」とあるが、`--volume`オプションでの書き方もできるっぽい？） 
  - `<volume name>:<mount point>`：ボリューム名とマウントポイント？コンテナ上のパス？を指定するのが、ボリュームマウント
  - `<path>:<mount point>`：ホストマシンのパスとマウントポイント？コンテナ上のパス？を指定するのが、バインドマウント
- `ports:`に、`container run --publish`で指定していたポートの公開を置き換える
- 自動でブリッジネットワークが作成され、サービス名が自動でコンテナのネットワークにおけるエイリアスとして設定される
- `command:`には、`container run <command>`で指定していたデフォルト命令の指定を置き換える
- `depends_on:`には、依存関係を定義。`docker-compose up`をした時に、`depends_on:`に記されたサービスたちから先に起動する。`docker-compose stop`をした時には、`depends_on:`に記されたサービスたちが後になるような順で停止する。
