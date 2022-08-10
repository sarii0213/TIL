## よく使うgem
- sorcery: 認証機能
- erb-lint: erbファイルを整える
- annotate: テーブル情報をモデルに出力
- pagy: ページネーション（kaminariより良い？）
- pre-commit: コミット直前に走らせたいスクリプトを定義（選択肢に限りがあるため、pre-commitファイルを自作するのも◎）
- ransack: 検索機能
- lipvips: 画像処理（other choices: ImageMagick, GraphicsMagick, OpenCV） [詳細](https://tech.medpeer.co.jp/entry/2020/07/30/100000)
- mysql2: MariaDBを使えるようにするgem。(railsに標準搭載gem)
  - MariaDB: MySQLから派生したオープンソースのシステムで、MySQLと互換性がある＆より高速であることが多い。  [詳細](https://www.integrate.io/jp/blog/mariadb-vs-mysql-everything-you-need-to-know-ja/#what)
- draper:  ヘルパーをオブジェクト指向的に書けるようにするgem。"デコレータ"。


## 各gemについて詳細

### ransack
- `distinct: true`  
  例えば、画像にコメントができるようになっていたとします。（画像対コメントが１対多になっていたとする）  
  この時、コメントを検索する時に画像が一覧で表示されるようになっていたとします。  
  同じ画像に同じワードを使ったコメントがあった場合、distinct: true がない場合同じ画像が２つ表示されてしまいます。  
  distinct: true を使っていれば、重複を削除しているので画像は１つのみの表示になります！  
  https://toomeeto.hatenablog.com/entry/2019/07/05/110624

### RuboCop
- `# rubocop:disable Metrics/MethodLength`: RuboCopにスルーしてもらいたい箇所を囲む
