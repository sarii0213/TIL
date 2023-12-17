## python 基礎
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

### list
- stringの書き換えはNGだが、listの書き換えはOK