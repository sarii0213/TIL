## python 基礎

### help
`help(<class_name>)`でメソッド一覧確認できる

### print
- `print(r'<そのまま出力したい文字列>')`
- `print("""<複数行の文字列 with 改行>""")`
- `print('<リテラル>'<改行>'<リテラル>')`と書くと、改行は無視され連結されて出力される
  - リテラル = シングルクオートで囲むもの
  - 長い文字列の行を分けられるので見やすく書ける
  
### slice
-`<index以上>:<index未満>`
- `word[0] = j`はNG。`word = j + word[1:]`とすればOK

### string
- f-stringsはformat関数の上位互換
  - `a = 'a' print(f'a is {a}')`
- `join`メソッド: list -> string
- コード上で改行したい時: ()で囲むか、バックスラッシュ使う

### list
- stringの書き換えは不可だが、listの書き換えは可能
- 主要メソッド
  - append, insert, pop, del, remove, extend, index, count, sort, reverse, split
- copyメソッド
  - ただ代入するだけだと参照渡しになる（＝代入元の値が入っているメモリの先頭の番地を指す値が代入先に入る）
    - dictionaryも同様の挙動
  - copyメソッドによって、値渡し（値だけのコピー）ができる

### tuple
- 新しい値を入れるようなデータ操作はできない。読み込み専用で使う
  - 新たにtupleを作ればtuple同士を足すような操作は可能
- `t = 1,`と()で囲まなくても、カンマを書くだけでtupleの定義になる。逆に()で囲んでもカンマなしだとtupleにならない
- tupleの展開方法：`x, y = 10, 20`
- 使用イメージ：後から書き換えてほしくない変数をtupleにしておけば、他のエンジニアがコード見ても変更不可のものだとわかる

### dictionary
- `dict(a=10, b=20)`, `dict([('a', 10), ('b', 20)])`のようにも定義できる
- updateメソッド：重なるkeyを上書き、ないkeyの値とのペアを追加
- getメソッド：引数で指定したkeyがない場合、classは`NoneType`に
- delメソッドだとdictionary丸ごと消えて、clearメソッドなら中身だけ空に

### set
- `set = {1,2,3,4}`
- 集合を表すので、重複した値はない、indexもない
- AND = `&`, OR  = `|`, X0R = `^`
- listからsetへの変換（uniqueなもの抽出）：`set(<list_object>)`

### if
- `if x in y:`, `if x not in y:`
- `not`はboolean型の判定に使う
- `0`, `0.0`, `''`, `[]`, `{}`, `()` = False
- `is`: object同士が同じものかどうかを判定
  - `if is_empty is None:`
  - `None`: 変数は宣言するけど何も入れたくない（null object）

### while
- `break`: ループから出る
- `continue`: それ以下の処理をスキップして次のループへ
- `else`: breakされずにループが終わったら実行される

### input
```python
# input関数で入力結果を変数に代入
while True:
    word = input('Enter:')
    num = int(word)
    if num == 100:
        break
    print('next')
```

### for
- `range(<num>)`関数: num分forループ回せる
  - `range(<first_num>, <last_num>, <interval>)`
- `enumerate`関数: 要素のindexを使える
  ```python
  for i, fruit in enumerate(['apple', 'banana', 'orange']):
    print(i, fruit)
  ```
- `zip`関数: index指定不要で複数リストから同index同士の要素を取り出せる
  ```python
  days = ['Mon', 'Tue', 'Wed']
  fruits = ['apple', 'banana', 'orange']
  drinks = ['coffee', 'tea', 'beer']

  for day, fruit, drink in zip(days, fruits, drinks):
    print(day, fruit, drink)
  ```
- dictionaryをforループで回す
  ```python
  d = {'x': 100, 'y': 200}
  # items関数でtupleのリストが出力され、それがk, vに展開される
  for k, v in d.items():
    print(k, ':', v)
  ```