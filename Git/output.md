## pre-commit
commit毎に、RSpec, erblint, RuboCopを回すのがめんどくさい  
pre-commit gemを教えてもらった（ただ、RuboCop＆RSpecのみの対応）  
調べていくと、pre-commit を自作で書いていくこともできるそう！  
https://qiita.com/yuku_t/items/ad072418290a2b01a35a  
これをやってみたいと思った  
ただ、みんなどうしているか気になる  
コミット毎に手動で静的解析＆テストを走らせているの？  

## git stashのよく使うコマンド
- 個別にファイルをstash
	`git stash push -- FILE_PATH`
- ステージに上げてない全ファイルのstash 〜メッセージを添えて〜
    - `git stash -u “STASH_MESSAGE”`
- stash一覧表示
    - `git stash list`
- stash適用＆一覧から削除
    - `git stash pop stash@{STASH_NUM}`
