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
- Docker Hub: DockerのイメージレジストリであるSaasサービス。イメージのGitHubのようなもの。
- デプロイ時
  - ECS/GKE:ECS(Amazon Elastic Container Service)とGKE(Google Kubernetes Engine)は、コンテナ管理サービス。Docker Engineの入ったLinuxのことで、ローカル環境で使ったコンテナをそのままデプロイできる場所。
  - ECR/GCR: ECR(Amazon Elastic Container Registory)とGCR(Google Container Registory)は、非公開のイメージのレジストリ。プライベートなDocker Hub。商用サービスを公開したくないけど、レジストリに登録しないとデプロイできない場合などに利用。
