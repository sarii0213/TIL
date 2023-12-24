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
- getメソッド：引数で指定したkeyのvalueを返す。引数で指定したkeyがない場合、classは`NoneType`に
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

### def
- 定義した関数は`function`型のクラスなので、変数に代入してそこで呼び出すこともできる
- 型を指定できるが、異なる型が入っても実行はできる
  ```python
  def add_num(a: int, b: int) -> int:
    return a + b
  ```
- 複数引数がある場合
  - 位置引数（順番に引数をとる）
  - キーワード引数
  - 位置引数とキーワード引数を混ぜる場合は順序に注意
- デフォルト引数の注意
  - デフォルト引数にlistは指定しない。参照渡しになり、バグに繋がりがち
- `*args`で引数をtuple化できる -> 任意の数の引数をとれる
- `**kwargs`で引数をdictionary化できる（関数呼び出し時には`**<dictionary型変数>`としてdictionaryを展開させる）
- 位置引数、`*args`、`**kwargs`は同時に使えるが、並べる順序は左のとおりでないとエラーになる

- docstrings
  - 関数内に`''''''`コメントアウトして、documentを書く
  - `<method>.__doc__`で、関数のドキュメントを出力

- 関数内関数
  - どんな時に使うか？　- 関数内だけで何度も使う関数がある場合

- クロージャ
  - 関数内関数objectをreturn
  - 引数だけ入れて関数を実行する準備をしてから、他のことをしてor用途を分けて、その関数の実行をしたいタイミングで実行させる

- デコレータ
  - 関数をラップしてなにか装飾をつけたい時に使う
  - デコレータを定義後、`@<decorator>`と書いた次の行に関数を定義することで、その関数のデコレータであることが表せる
  - １つの関数に複数デコレータを書けるが、書く順によって包まれる順番が変わるのに注意

- ラムダ
  - 関数を引数として渡す
  - for文で引数を渡して関数を実行し通したり
  - 例：`change_words(l, lambda word: word.lower())`

- ジェネレーター
  - 反復処理（単なるfor loopとは違い、ループ処理を抜けて他の処理ができたり）
  - `yield`を使うと状態を保持しつつ、一旦ループから抜けて次へ
  - `next(<generator>)`で次の処理へ
  - 重たい処理を一気に行うより、呼ばれるたびに小分けにして実行させられる

### リスト内包表記
```python
t = (1, 2, 3, 4, 5)
r = []
r = [i for i in t if i % 2 == 0]
```
- リストにappend
- for文 * 1, if文 * 1くらいなら可読性OK

### 辞書内包表記
```python
w = ['mon', 'tue', 'wed']
f = ['coffee', 'milk', 'water']
d = {x: y for x, y in zip(w, f)}
```
- dictionaryにappend
- 集合内包表記も上記と同様

### ジェネレーター内包表記
```python
g = (i for i in range(10))
```
- tupleは`tuple(i for i in range(10))`と書く

### 名前空間とスコープ
- グローバル変数で宣言済みの変数を関数内で再度宣言すると、グローバル変数は無視される
- -> `global <global_var>`のように書けばグローバル変数として認識させられる
- `locals()`, `globals()`でローカル/グローバル変数の一覧が表示できる
  - global変数
    - `__doc__`: スクリプトの一番上に書いた関数のドキュメント
    - `__name__`: 実行しているモジュール（globalだと`__main__`）

### 例外処理
```python
try:
  <実行する処理>
except <error_class> as <error_variable>:
  <例外時の処理>
else:
  <正常終了時に実行する処理>
finally:
  <例外に関わらず必ず最後に実行する処理>
```
- Exception hierarchy（クラスの継承）
  BaseException > Exception > IndexError, NameError
- 独自例外の作成
  - `class <error_class>(継承するErrorClass): pass `で独自の例外を定義して、
  - `raise <error_class>(<error_message>)`で任意の条件で例外発生させられる

### コマンドライン引数
```python
import sys
print(sys.argv)
```
- `python <file_name> <arg1> <arg2>`とコマンド入力した値を`sys.argv`で取れる


### モジュールとパッケージ
- `__init__.py`: パッケージが呼ばれた際に最初に読み込まれるファイル（必須）
- `import <package_name>.<module_name>`でインポート後、`<package_name>.<module_name>.<func_name>`で関数実行
- `from <package_name> import <module_name>`でインポート後、`<module_name>.<func_name>`で関数実行
- 上記２つは組織ごとに好みが分かれる
  フルパス（前者）はコード行が長くなってもどのパッケージ由来かわかりやすいが、文字数多くなる
- package内の__init__.pyで`__all__ = [<module_name>,,,]`と列挙しておくと、`import *`で読み込むmoduleを指定できる（`*`でのimportは非推奨。何importしてるか把握しずらいから）
- package, moduleのファイル構造を変えている場合は、`try: <import_sentence> except ImportError: <other_import_sentence>`でエラーハンドリングできる
- setup.pyを書いてpackage内のファイル群をひとつまとめてpackage化できる
  - `python setup.py sdist`-> tarファイル作られる
- 組み込み関数：pythonにあらかじめて読み込まれている(=builtin)moduleの関数（print, range, zip, sorted,,）
- 標準ライブラリ：importして使えるmodule(https://docs.python.jp/3/library/index.html)
  - 例：`collections.defautldict`: dictionaryの値の初期値を設定してdictを宣言できる
- PyPI：サードパーティのライブラリ。誰かが作ったpackageをダウンロードして使える
- 注意！importすると、importされたmoduleで呼び出している関数はimport時に実行される。以下のように書けばimport時には実行されない
  ```python
  def main():
    <このスクリプトがメインスクリプトの場合に実行したい関数>

  if __name__ == '__main__':
    main()
  ```
```python
# importするpackage, moduleの書く順
import <標準ライブラリ>

import <サードパーティのライブラリ>

import <自作のパッケージ>

import <ローカルのmodule>
```


### オブジェクトとクラス
```python
# python3では(object)は不要になったが、BaseClassとして継承させることを考慮するとつけておいた方がいい、という暗黙のコードスタイルがある
class Person(object):

  # コンストラクタ
  def __init(self, <引数>):
    <クラス初期化時に実行する処理>

  def say_something(self):
    print('hello')

  # デストラクタ
  def __del__(self):
     <objectが使われなくなった時点で実行する処理>

# 継承
class ToyotaCar(Car):
  pass  # 継承元の関数そのまま流用
  super()  #親クラスの関数呼び出し

  # デコレータでpropertyと宣言することで、読み込み専用のクラス変数になる
  @property
  def enable_auto_run(self):
    return self._enable_auto_run

  # デコレータでpropertyでwrapした関数.setterを宣言することで、書き込めるクラス変数に
  # ある条件下のみ値の書き換えが行える、といった制限を適用できる◎
  @enable_auto_run.setter
  def enable_auto_run(self, is_enable):
    self._enable_auto_run = is_enable
```
- `self.__<クラス変数>`と書くと、クラス外からは`<クラスオブジェクト>.__<クラス変数>`と書いてもエラーになる（外からのアクセス制限を明示）
- 抽象クラス：子クラスで定義すべき関数を指定できる（定義されていないとエラーに）。抽象クラスの多用はコードスタイル的に非推奨
  ```python
  import abc

  class <親クラス>(metaclass=abc.ABCMeta):

    @abc.abstractmethod
    def <子クラスで定義必須の関数>:
      pass
  ```
- 多重継承（複数の親クラスを継承）では、継承された順に関数を探す（＝左に書かれたものが優先）。継承の優先順で複雑化するのでバグに繋がり非推奨
- クラス変数：同クラスの他のobjectとも共有できる。objectになっていない状態（クラス）でも使える
- クラスメソッド：objectとして生成されていない（selfがない）状態でもアクセスできる
  ```python
  @classmethod
  def what_is_your_kind(cls):
    return cls.kind
  ```
- スタティックメソッド：クラス、オブジェクトに関係するけどデータは使わない関数
  ```python
  @staticmethod
  def about():
    print('about human')
  ```
- 特殊メソッド：`__`で囲まれた関数。
  - `__str__`: 文字列として扱われた時の処理
  - `__len__`: len関数の引数として呼ばれた時の処理
  - `__add__`: object同士が`+`された時の処理
  - `__eq__`: object同士が`==`された時の処理


### ファイル操作とシステム
- ファイル操作
  ```python
  with open('<file_name>', '<mode w, r, a, c, w+,,,>') as f:
    # w+: 開いた瞬間に中身消してから、読み書き可能に
    # r+: ファイルの中身がないとエラーに
    f.write('<strings to write in the file>')
    print('<strings to write>', file=<file_name>)
  # f.close()しなくても勝手にクローズしてくれる。メモリの節約。

  with open('<file_name>', 'r') as f:
    # withの中で宣言した変数はwith外でも使える
    while True:
      chunk = 2 # ２文字ごとに抽出
      line = f.read(chunk)
      print(line)
      if not line:
        break
  ```
- seekメソッド：ファイル内のカーソル位置を指定 `<arg>`文字目へ

- template 
  - `import string`して、`string.Template(<文字列>)`のオブジェクトをsubstituteメソッドで文字列内の`$<変数>`を引数で置換

- csv
  ```python
  import csv
  with open('test.csv', 'w') as csv_file:
    fieldnames = ['Name', 'Count']
    writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({'Name': 'A', 'Count': 1})
  ``` 

- ファイル操作
  ```python
  import os
  import pathlib
  import glob
  import shutil

  print(os.path.exists('test.txt'))
  print(os.path.isfile('test.txt'))
  print(os.path.isdir('design'))

  os.rename('test.txt', 'renamed.txt')
  os.symlink('renamed.txt', 'symlink.txt')
  os.mkdir('test_dir')

  pathlib.Path('empty.txt').touch()


  # ディレクトリ内のファイル一覧 を表示
  print(glob.glob('test_dir/test_dir2/*'))

  shutil.copy('<copy元ファイル>', 'copy先ファイル')
  shutil.rmtree('<中身のファイルごとディレクトリ削除>')
  ```

- tarfile
  ```python
  import tarfile

  with tarfile.open('test.tar.gz', 'w:gz') as tr:
    tr.add('test_dir')

  with tarfile.open('test.tar.gz', 'r:gz') as tr:
    tr.extractall(path='test_tar') # test_tarディレクトリ内に展開
    with tr.extractfile('test_dir/sub_dir/sub_test.txt') as f: # 展開せずにtarfileの中身を読む
        print(f.read())
  ```
- zipfile
  ```python
  import glob
  import zipfile


  with zipfile.ZipFile('tet.zip', 'w') as z:
    for f in glob.glob('test_dir/**', recursive=True):
        print(f)
        z.write(f)

  with zipfile.ZipFile('tet.zip', 'r') as z:
    # z.extractall('zzz2')
    with z.open('test_dir/test.txt') as f:
        print(f.read())
  ```

- tmpfile: ioバッファの上でファイルを作って、使い終わったら勝手に消してくれる
  ```python
  import tempfile

  with tempfile.TemporaryFile(mode='w+') as t:
    t.write('hello')
    t.seek(0)
    print(t.read())

  # tmpファイルを消したくない場合
  with tempfile.NamedTemporaryFile(delete=False) as t:
    with open(t.name, 'w+') as f: # t.name = file path
        f.write('test\n')
        f.seek(0)
        print(f.read())

  with tempfile.TemporaryDirectory() as td:
    print(td)
  ```
- subprocess: terminalでやるcommandを実行できる
  - `os.system`でも`subprocess.run`と同様にコマンド実行できるが、subprocessの方が推奨されてる