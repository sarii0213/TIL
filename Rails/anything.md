## scopeとクラスメソッドの使い分け ❔
- scope 使い回すもの
- クラスメソッド 使い回さないもの


## new と build のちがい
　「モデルを関連付けしたときに使うのがbuild」という慣習

## 永続化するDB @ production env
- AWSのElastiCacheというサービスがあって、それを使うことが多い
- ElastiCache: Redis（など）のフルマネージドなサービス
- フルマネージド：細かいメンテナンスが不要のRedisみたいなイメージ。AWSがよろしく面倒を見てくれるようなイメージ。サーバーレスよりは触れる範囲広い。

## cheat sheets
- [Ruby on Rails Cheatsheet](https://gist.github.com/mdang/95b4f54cadf12e7e0415)
- [Ruby-Cheatsheet](https://github.com/lifeparticle/Ruby-Cheatsheet)