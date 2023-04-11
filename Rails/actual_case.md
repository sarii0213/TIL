## 実案件にトライ

### 工数管理
- どれくらいの時間がかかったか、なぜ時間がかかったかをメモしておく

### 開発する前はdevelopブランチを最新に
```bash
git checkout develop
git pull origin develop
```

### ブランチ命名ルール
- issue-番号/ざっくりissue名
- 例）`git checkout -b issue-2316/add_video_link_to_cs_page`

### 作業の手順
- アサインされたら、assigneeを自身に変更してIn progressに移す
- git pull
- git checkout -b issue-...
- git stash apply stash@{n} (Gemfile.lock適用)
  - 参考：`stash@{0}: WIP on issue-2175/modify_content_of_confirmation_mail: f0955e0a [modify] 予約完了メール・リマインドメールの内容を変更`
- 挙動確認してOKだったら
-  git stash push -- Gemfile.lock
-  git add .
-  git commit
-  git push origin issue-...

###  注意
- addしない方がいいファイルは stash or reset
  - package.json
  - Gemfile.lock (libv8 `bundle update`したため)
  - 例） `git stash save -u ".node-version"`
  - 例） `git stash save "after building env"`
  - tigが便利らしい [tigでgitをもっと便利に！ addやcommitも](https://qiita.com/suino/items/b0dae7e00bd7165f79ea)

### 秘密情報（予約時）
- クレジットカード
  - カード番号：1
  - 有効期限：未来の日付
  - セキュリティコードは123（なんでもよかったかも）

### 診察予約時のエラー回避
app/forms/online_clinic/clinics/create_reservation_form.rb:20
```
# @examination.web_mtg_url = @examination.create_mtg_url(doctor_schedule)
@examination.web_mtg_url = 'https://www.google.com/'
```

### Gemfile.lock
branch: issue-2099/delete_doc_picture_from_cancellation_mail
➜ diff Gemfile.lock ~/Desktop/anamune/Gemfile.lock
425d424
<     jp_prefecture (1.1.1)
459,460c458
<     libv8 (7.3.492.27.1)
<     line-bot-api (1.23.0)
---
>     libv8-node (16.10.0.0)
484,485c482,483
<     mini_racer (0.2.6)
<       libv8 (>= 6.9.411)
---
>     mini_racer (0.6.3)
>       libv8-node (~> 16.10.0.0)
857d854
<   jp_prefecture
864d860
<   line-bot-api

#### Gemfile.lockのコンフリクトの予防法（作業の手順の改変）
- まず、issueブランチ削除して、developブランチから(pull後)issueブランチを新たに切る
  - issue-2099/delete_doc_picture_from_cancellation_mail
- `bundle update mini_racer`を実行
- `bin/rails s`や`bin/rails db`が実行できることを確認
- issueに取り組む

### issueに取り組む準備
- MySQL起動していなかったら`sudo mysql.server start`
- Redis起動していなかったら`redis-server`
- Sidekiq起動していなかったら`bundle exec sidekiq`
- `bin/webpack-dev-server`
- `bin/rails s`

### featureブランチでのmigrationのミスの対処
- featureブランチでのミス（カラム名のタイプミスなど）は、マイグレーションファイルに残さず、正しい処理のみ残すようにする
  - （`rails db:migrate:reset`すれば正しいテーブル設計が反映される）

