## URLパラメータの付与
- `root_path(after_sign_in: true)`のようにすると、`root_path`のURLに`?after_sign_in=true`というパラメータを付与できる

## featureブランチでのmigrationのミスの対処
- featureブランチでのミス（カラム名のタイプミスなど）は、マイグレーションファイルに残さず、正しい処理のみ残すようにする
  - （`rails db:migrate:reset`すれば正しいテーブル設計が反映される）

## 使い捨てスクリプト
- [使い捨てのスクリプトの置き場の名前のはoneshotがよさそう](https://shinkufencer.hateblo.jp/entry/2019/08/19/000000)


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

## rails c で直前の実行結果を代入したい時
- `<variable> = _`で直前の実行結果を変数に代入できる

## flashについて
- flashは１度redirectされても残るもの
- redirectと一緒に使うときは`flash`のままでOK
- renderと一緒に使うときは`flash.now`にすることで、redirectされるとflash messageは消えてくれる

## turbo
- `link_to`でdeleteメソッドにしたい場合は、`data: { "turbo-method": :delete }`をつける
  

## session
- sessionはユーザーごとに別々に用意されたハッシュ