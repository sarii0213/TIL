### let!とbeforeの違い  
let, let! -> インスタンス変数の定義、生成後に参照したいもの （なるべくlet!を利用。可読性◎） 
before -> メソッドの定義  
let!とbeforeにおいては、定義した順番に実行される  

### create_list(:INSTANCE_NAME, NUM)
- インスタンスを任意の個数分作られたコレクションを返す
- 不必要に作らない！
  - FactoryBotのtransient+evaluatorの機能を使い間連先レコード生成数をコントロールする
    - transient: 作成時に挙動を変更するためのフラグや追加データとして利用
    - evaluator: コールバックブロック内のcreate*, build*メソッドの第二引数に、evaluator.<transient内変数>を置き、specのcreate*, build*メソッドでこの変数の値を変更できる

### xit
example全体をskip   
- テスト成功失敗に関わらずテストを実行しない
- pending扱い
- プルリクエスト出したいけどこのテストは一旦保留にしたい、テストが未完成だけどとりあえずプルリク出したい時などに便利

### テストの前提条件を上書きしたり修正したりしない
- `let`や`update()`とデータの一部を更新すると、データの全容がつかみづらくなる
- 上書きするよりコピペが◎

### フィクスチャ

- ＝サンプルデータ
- テストの前提となる状態を作り出す
- テストごとに何度も繰り返し呼ばれるため、効率的なデータ生成が◎
- FactoryBotはフィクスチャのツール

https://railsguides.jp/testing.html#%E3%83%95%E3%82%A3%E3%82%AF%E3%82%B9%E3%83%81%E3%83%A3%E3%81%AE%E3%81%97%E3%81%8F%E3%81%BF

モデルのテストでfixtureファイルをアタッチしたい時は、
`post.images.attach(io: File.open('db/fixtures/dummy.jpg'), filename: 'dummy’)`
とopen IOオブジェクトとファイル名を１つ以上含むハッシュを渡す

https://railsguides.jp/active_storage_overview.html#file-io-objects%E3%82%92%E3%82%A2%E3%82%BF%E3%83%83%E3%83%81%E3%81%99%E3%82%8B



## FactoryBot

### sequence
- 連番データを作成できる
- ユニークな値になる

### trait
- FactoryBotで複数データを作成する際に使用(継承)
- 記述の重複を減らして可読性を高める
- traitをspecファイルで呼び出すには、引数として渡す必要がある
- `trait(:with_ASSOCIATED_MODEL)`で、関連のあるモデルも一緒に作成できる

### 『createじゃなきゃだめなの？buildでもいいのでは？』 ケースに注意
- ユニットテストの最小限のデータを用意すべし（インスタンスだけあれば良い場合はbuild!）
- SQLが走る分、パフォーマンス下がる
- created_at, updated_atの値がnilだと困るなら…
  - **build_stubbed** : nilの値をダミーデータで置き換えてくれる 

### FactoryBot デフォルト値はランダムに
- FactoryBotのデフォルト値に依存するようなspecは書かない
- `status %i[ready doing done].sample }`などとランダムになるように書く
- テストがこけた時の再現方法：`rspec --seed 1234`のようにseed値を指定すればよい

## 参考
- [RSpecスタイルガイド](https://github.com/willnet/rspec-style-guide)
- [Clean Test Code Revised](https://speakerdeck.com/willnet/clean-test-code-revised)
- [sequenceとtraitについて](https://note.com/izuha0/n/n212843da0bd4)
- [FactoryBot the right way](https://www.youtube.com/watch?v=n0epZM-lZvw)
- [rspecを読みやすくメンテしやすく書くために](https://zenn.dev/yuji_developer/articles/52cc0e356b3748)
