## 階層表現
```ruby
resources :posts do
  resources :comments, module: :posts
end
```
<br>
ルーティング（＝エンドポイント）の階層表現：

```ruby
resources :posts do
  resources :comments
end
```
<br>

コントローラの階層表現：

```ruby
module: :posts
```
