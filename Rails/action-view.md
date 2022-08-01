## lメソッド（localizeのエアリアスメソッド）
- 日時の表示に利用
- 設定方法
  - タイムゾーンの設定（config/application.rb config.time_zone） ←必須
  - ロケールの設定（config/application.rb config.i18n.default_locale）　←必須
  - 日時フォーマット（config/locales/ja.yml ja: time: format: default: ***）
  - gem i18n_generatorsインストール＆実行でja.ymlを自動生成できる

https://qiita.com/jnchito/items/831654253fb8a958e　

## dom_idヘルパー
DOMのid規約に沿った名前を取得

## ハッシュ構文で値が省略可に
※キーと値の文字が一致している場合  　
https://blog.saeloun.com/2021/09/28/ruby-allow-value-omission-in-hash-literals.html 

