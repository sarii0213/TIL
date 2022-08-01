## let!とbeforeの違い  
let, let! -> インスタンス変数の定義   
before -> メソッドの定義  
let!とbeforeにおいては、定義した順番に実行される  

## create_list(:INSTANCE_NAME, NUM)
RSpecで、インスタンスを任意の個数分作れる

## フィクスチャ

＝サンプルデータ

https://railsguides.jp/testing.html#%E3%83%95%E3%82%A3%E3%82%AF%E3%82%B9%E3%83%81%E3%83%A3%E3%81%AE%E3%81%97%E3%81%8F%E3%81%BF

モデルのテストでfixtureファイルをアタッチしたい時は、
`post.images.attach(io: File.open('db/fixtures/dummy.jpg'), filename: 'dummy’)`
とopen IOオブジェクトとファイル名を１つ以上含むハッシュを渡す

https://railsguides.jp/active_storage_overview.html#file-io-objects%E3%82%92%E3%82%A2%E3%82%BF%E3%83%83%E3%83%81%E3%81%99%E3%82%8B
