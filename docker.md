## よく使うコマンド
- `docker compose up` ：　Ctrl+cで終了
- `docker compose up -d`　: バックグラウンドで起動
- `docker ps`　: 起動しているコンテナ一覧
- `docker attach <CONTAINER_NAME>`　: Ctrl+ p -> q　でdettach。exitで終了
- `docker (compose) exec <SERVICE> <COMMAND>`　: 指定したservice （docker-compose.ymlに記載。webなど) の中で、指定したcommand （bashなど） を実行
- `docker stop <CONTAINER_NAME>`
