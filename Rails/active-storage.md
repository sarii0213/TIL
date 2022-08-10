## Active Storageの仕組み

- active_storage_blobsテーブル（ActiveStorage::Blobモデル）： 
  添付されたファイルに対応。識別key、ファイル名、Content-Type、ファイルのメタデータ、サイズなどを管理

- active_storage_attachementsテーブル（ActiveStorage::Attachmentモデル）: 
  ActiveStorage::Blobとアプリ内のモデルを関連付ける中間テーブル。  
  関連付けるモデルのクラス名(:record_type)や連携するFKカラム名(:name)を、FK値(:record_id)と共に保持。  
  ポリモーフィック関連。  
  `t.references :record, polymorphic: true`とすることで、record_typeとrecord_idが生成される。

  - 疑問点
    ```
    t.index [ :record_type, :record_id, :name, :blob_id ], name: :index_active_storage_attachments_uniqueness, unique: true
    ```

    なんで`t.references :record, polymorphic: true, index: false`と書いて、上記のようにindex追加しているの？  
    → 複合キーを作っている！関連づけるモデルのクラス名、そのモデルのFKカラム名、FK値、BlobモデルのFK値を合わせた複合キー。

  - ポリモーフィック結合
  	「ポリモーフィック関連付け（polymorphic association）:  
	  ある1つのモデルが他の複数のモデルに属していることを、1つの関連付けだけで表現  
	  →ファイルがアップロードされるたびにactive_storage_attachementsテーブル&ActiveStorage::Attachmentモデルを更新する必要がなくなる  


- active_storage_variant_records: variantテーブル：
  （サイズの変更などの加工情報）を記録する

- 参考：
https://qiita.com/itkrt2y/items/32ad1512fce1bf90c20b


## Actibe Storage関連ヘルパー
- ` rails_storage_proxy_path/url`: 特定の添付ファイルを明示的にプロキシする
  - プロキシとは： リクエストに応じて、appサーバが、ストレージサービスからファイルをダウンロードする。（署名付きURLにリダイレクトすることなく、背後のストレージサービスからファイルを配信できるようになった）
    - メリット：プロキシサーバにキャッシュが残るため、ファイル配信速度が上がる＆名前の指定によってサイズも指定できる?
    
  - デフォルトの挙動： Webサーバーから直接ファイルを配信していた。（→appサーバへの負荷減）　ActiveStorageが生成するファイル用のURLにアクセスすると、短時間だけ有効な署名付きURLにリダイレクトされる。
  - [参考](https://techracho.bpsinc.jp/hachi8833/2021_07_30/110040)
  - [rails github](https://github.com/rails/rails/pull/34477/files#diff-6d57479f3b0a37809da807fc49880b7edcd1067f64f0df24fef8bfdee3ee332eR113)
