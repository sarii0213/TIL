## pullできない時
stashするか、取り消していい変更なら`git checkout HEAD .`

## pre-commit
commit毎に、RSpec, erblint, RuboCopを回すのがめんどくさい  
pre-commit gemを教えてもらった（ただ、RuboCop＆RSpecのみの対応）    
調べていくと、pre-commit を自作で書いていくこともできるそう！   やってみたい  
ただ、みんなどうしているか気になる    
コミット毎に手動で静的解析＆テストを走らせているの？  ←ぽい  
「pre-commit gemは毎回lintが走るのがしんどい」という意見をもらった  
気が向いたら一度試そう

参考  
- [pre-commitファイル自作](https://qiita.com/yuku_t/items/ad072418290a2b01a35a)
- [Quickhook: Faster Git Hook (pre-commit, Etc.) Runner](https://morioh.com/p/7f7cfefb24e8)
- [pre-commit gem docs](https://www.rubydoc.info/gems/pre-commit/0.31.0)
- [pre-commit gem](https://qiita.com/ryoff/items/9ab8958b2570bde3ab4d)
- [pre-commit gem 2](https://ohmyenter.com/rubocop-with-pre-commit/)
- [pre-commit gem 3](https://dev.classmethod.jp/articles/pre-commit-rubocop/)
- [pre-commit gem 4](https://qiita.com/yn-misaki/items/adbc9a02be226bc10354)

## git stashのよく使うコマンド
- 個別にファイルをstash
	`git stash push -- FILE_PATH`
- ステージに上げてない全ファイルのstash 〜メッセージを添えて〜
    - `git stash -u “STASH_MESSAGE”`
- メッセージを添えてstash
  - `git stash push -m "MESSAGE"`
- stash一覧表示
    - `git stash list`
- stash適用＆一覧から削除
    - `git stash pop stash@{STASH_NUM}`

## オートコンプリートを有効にするとめっちゃ良い

https://apple.stackexchange.com/questions/55875/git-auto-complete-for-branches-at-the-command-line
