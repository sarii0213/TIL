# タイムゾーンの設定
## Rails内
config/application.rb

```ruby
config.time_zone = 'Tokyo'
config.active_record.default_timezone = :local
```

- [参考](https://zenn.dev/ryouzi/articles/dda18594f2dbd3)

## MySQL内
- /etc/my.cnfで設定を変更　[参考](https://salumarine.com/checking-timezone-on-mysql/)
- ただ、Docker使用時はdocker-compose.ymlを書き換える必要があり、ややこしそう。。
- rails内で、時間を設定してしまえばOK [参考](https://nishinatoshiharu.com/prepare-many-seeds/)


## なんでタイムゾーンについて気になったか
- `insert_all`メソッドを使うと、`created_at`の値が９時間ずれることにきづいた　


## 【番外編】 RailsからMySQLへの入り方
1. `$ bin/rails dbconsole`  
2. パスワード：docker-compose.yml `services.db.environment.MY_SQL_PASSWORD`
3. データベース一覧表示したり、テーブル一覧表示したり、SELECT文使ったり

- [参考](https://qiita.com/k_s/items/6fbd2ca409d876858b29)
