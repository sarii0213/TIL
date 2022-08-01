## with_attached_images.includes(:user)

`with_attached_images`メソッドを使うことで、内部的にincludes結合扱いになり、N+1クエリ問題を回避できる

eager loading(一括読み込み)：  
Model.findによって返されるオブジェクトに関連付けられたレコードを、クエリの利用回数をできるかぎり減らして読み込むためのメカニズム。includesメソッドなどでeager loadingを実現。  
includes: Active Recordは指定された全ての関連付けを最小限のクエリ回数で読み込む

https://railsguides.jp/active_record_querying.html#%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91%E3%82%92eager-loading%E3%81%99%E3%82%8B　
