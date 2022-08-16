## GitHub Actions

GitHubと連携しやすいCIツール

これから学んでいきたい

https://qiita.com/ham0215/items/41a66a17190dd7c319a7


https://github.com/actions/runner-images

## rspec + ubuntu + mysql の相性
`jobs.rspec.runs-on: ubuntu-latest`だと、ローカルのDockerコンテナ上では問題なくrspecが通るのに、GitHub Actionsでは「mysqlがどうこう」などのエラーが出る不具合が。
`ubuntu-18.04`にすると、エラーが出なくなる！
