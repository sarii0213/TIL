## GitHub Actions

GitHubと連携しやすいCIツール

これから学んでいきたい

https://qiita.com/ham0215/items/41a66a17190dd7c319a7


https://github.com/actions/runner-images

## rspec + ubuntu + mysql のエラー１
`jobs.rspec.runs-on: ubuntu-latest`だと、ローカルのDockerコンテナ上では問題なくrspecが通るのに、GitHub Actionsでは「mysqlがどうこう」などのエラーが出る不具合が。
`ubuntu-18.04`にすると、エラーが出なくなる！

## rspec + mysql のエラー２
- config/environments/test.rbに`config.active_job.queue_adapter = :inline`を追記
- [エラー解決法](https://github.com/brianmario/mysql2/issues/983)
- [Active Jobについて](https://qiita.com/petertakahashi/items/cb9ae73e5ba3020f4a89)
