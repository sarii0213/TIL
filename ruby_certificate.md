#  Ruby技術者試験 Silver

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

