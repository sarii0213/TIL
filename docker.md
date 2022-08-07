# 🐳 Docker

## よく使うコマンド
- `docker compose up` ：　コンテナ起動（Ctrl+cで終了）
- `docker compose up -d`　: バックグラウンドで起動
- `docker ps`　: 起動しているコンテナ一覧
- `docker attach <CONTAINER_NAME>`　: Ctrl+ p -> q　でdettach。exitで停止
- `docker (compose) exec <SERVICE> <COMMAND>`　: 指定したservice （docker-compose.ymlに記載。webなど) の中で、指定したcommand （bashなど） を実行
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
-　Docker Hubで公開されている

### Dockerfile
- 既存のイメージにレイヤーをさらに積み重ねるためのテキストファイル
- 公開されているイメージ(ex. Ubuntu&PHP&.ini)にDockerfileでレイヤー(ex. Git)を乗せる感じ
- GitHubで共有するなどして、チームメンバーで同一のコンテナを構築できる

### Docker 基本コマンド　（最初の`docker`省略）
- `container run [option] <image> [command]`: イメージからコンテナ起動　（=`image pull`&`container create`&`container start`）
  - `--rm`オプション： コンテナが停止したとき自動削除
  - `--detach`:バックグラウンドで実行
  - `--name`: コンテナ名指定
  - `--interactive`: コンテナの標準入力に接続（対話操作）
  - `--tty`: 疑似ターミナルを割り当てる（対話操作）　（tty: 接続端末の名札のようなもの）
  - `[command]`: デフォルトの命令ではなく、任意の命令を実行させる
- `image build`: Dockerfileからイメージ作成（その後、`container run`）
- `container exec`: コンテナに命令を送る
- `compose up`: コマンドの手順書(Yamlファイル)に書かれたコマンド(`container run <image>`..)をまとめて実行して起動
- `container ls`: コンテナ一覧
- `container stop <container>`: コンテナ停止
- `container rm <container>`: コンテナ削除　（`-f`で停止＆削除）

### コンテナについて
- コンテナは、メインプロセス（`PID`=1）を実行するために起動する
- コンテナが停止するのは、コンテナを停止させたとき(stop) or メインプロセスが停止した時


  - `--rm`オプション： コンテナが停止したとき自動削除
