## ルート（相対）パス

db/seeds.rb
```ruby
require './db/seeds/posts'
```
- ルート階層が基準の相対パス
- サーバーでしか機能しない

## LOAD_PATH
- Railsでは、ロードパスという、パスのリストが定義されている。
- `rails c`で`$LOAD_PATH`と叩くと一覧表示される
- `<APP_NAME>/spec`がロードパスにないのに、specファイルで`require 'rails_helper'`が通るワケは、specが起動されるとそのプロセスではspecのコアファイルに定義された、ロードパスにspecディレクトリを含めるメソッドが実行され、そのプロセス内のロードパスに`<APP_NAME>/spec`が追加される。specプロセスを抜けてしまうと、デフォルトのロードパスの設定が適用されているため、`<APP_NAME>/spec`はロードパスに存在しない。
