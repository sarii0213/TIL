#  Ruby技術者試験 Silver

## 真偽値
- `nil`と`false`以外は真

## 予約語
true, false, class, BEGIN, END, and begin, break, case, do, end, else, elsif, def, for, if, in, nil, not, or, rescue, return, until, then, super, when, while,,,

## クラス
- `@@var`: クラス変数（継承されたクラスにも変更反映される）
- `@var`: インスタンス変数

## 多重代入
```rb
(x, y), z = 1, 2, 3
p x, y, z
#=> [1, nil, 2]

x = 1, 2, 3, 4
p x
#=> [1, 2, 3, 4]

(x, y), z = [1, 2], 3
p x, y, z
#=> [1, 2, 3]
```

## Proc
- ブロックをオブジェクト化したもの
- メソッドの引数としてブロックを呼び出せる
  - `&引数名`だとブロックとして渡され、`引数名`のみだと普通の引数（procオブジェクト）として渡される
  - ブロックとして渡す場合、引数の数はひとつのみ可能。procオブジェクトを普通の引数として渡す場合は複数可能。
- Railsではscopeの第二引数にlambdaメソッドが渡されてProcインスタンスが生成されている（第一引数がメソッド名`:method_name`）
- procオブジェクトの生成方法
  - (`Proc.new` / `proc`) or `-> {}`(lambda) 
- lambdaと(`Proc.new` / `proc`)の違い
  - lambdaは、引数の数に厳密（数が違うとエラーになる）
  - procオブジェクト内で`return`使用時、lambdaはprocオブジェクトを抜けるのみだが、`Proc.new`ではprocオブジェクトを囲むメソッドを抜ける
```rb
# Proc.newの代わりにprocでも同じ
b = Proc.new { |a, b, c| p a, b, c  }
b.call(2,4)
#=> 2
    4
    nil

b = lambda { |a, b, c| p a, b, c }
b.call(2,4)
#=> wrong number of arguments

# &引数名でブロックとして引数を渡す場合は引数の数はひとつのみ
def sample_proc(&my_proc)
  puts my_proc.call(2)  #=> 4
end
sample_proc { |n| n * 2 }
```

## オブジェクトの比較メソッド
- `==`: 数値として等しいか
- `eql?`: 同じクラスのオブジェクト && `==`
- `equal?`: オブジェクトIDが等しいか
- `|`, `&`: 必ず右辺が評価
- `||`, `&&`, `or`, `and`: 短絡評価。必要な時のみ右辺も評価。

```rb
p 1 == 1.0 
#=> true
p 1.eql? 1.0
#=> false
p 1.eql? (0 + 1)
#=> true
p 1.equal? 1.0
#=> false
p 1.equal? (0 + 1)
#=> true
```

## フォーマット文字列
- `%x`, `%D` : `%m/%d/%y`   ex)  02/13/96
- `%F`: `%Y-%m-%d` ex) 1996-02-13

## String
### Stringクラスのメソッド
- `%`: フォーマットされた文字列を返す
```rb
p "Hello%d" % 5
#=> "Hello5"
```

- `split`: `()`をつけるとマッチしたものを含んだ結果を返す
```rb
p "Spring,Summer,Autumn,Winter".split(/(,)/)
#=> ["Spring", ",", "Summer", ",", "Autumn", ",", "Winter"]
```

- `gsub(pattern, replace)`: patternにマッチした文字列全てをreplaceで置き換えた文字列生成。`sub`は最初の該当箇所のみ。
```rb
p 'abcdefg'.gsub(/def/, '!!')          # => "abc!!g"
```

- `to_i`: 数字にできるところまで数値化。それ以降は無視。
- `delete_prefix`: 先頭から削除した文字列を返す。`delete`は該当する全ての文字列削除。
- `chop`: 最後の文字列削除。`\r\n`があればその二文字削除。
- 


## Array
### Arrayクラスのメソッド
- `shift`：先頭から削除
- `unshift`：先頭に追加
- `push`：末尾に追加
- `pop`：末尾から削除
- `product`: レシーバーの配列と引数の配列からそれぞれ１要素ずつ取り出して新配列作る
- `delete_if`: １要素ずつブロックへ渡し、その結果が真の要素をselfから削除。`reject!`と同じ動作。
- `zip`: 配列の配列を作成。`[1,2].zip([3,4]) #=> [[1,3],[2,4]]`
- `select`, `filter`: 与えられたブロックが真の値を返す要素の配列を返す

## Hash
`**hash`でキーワード変数に変換

### Hashオブジェクトの生成方法
- `Hash({})`, `{}`
- `Hash[]`
- `Hash.new()`
  - `Hash.new(default)`で`default`に引数が指定された場合はvalueに`default`が入る。ただ、`p`メソッドなどでHashの内容を参照する際は対象外に！

### Hashクラスのメソッド
- `member?`: ハッシュがキーを持つか。エイリアス：`has_key?`, `include?`, `key?`
- `to_a`：配列に変換
- `update`: 破壊的に更新
- `clear`： 空にしてselfを返す
- `merge`: selfと引数のハッシュをマージし、新しいHashを返す。非破壊的。

## Numeric
## Numericクラスのメソッド
- `Numeric#step(limit, step)`: `Numeric`オブジェクトを始点に、`step`ずつ`limit`まで繰り返し