Gemfile
```ruby
group :development, :test do
  ...
  gem 'factory_bot_rails'
  gem 'faker'
  ...
  end
```
<br>

spec/factories/\*.rb
```ruby
FactoryBot.define do
  factory :post do
    body { Faker::Lorem.sentence }
    user
    after(:build) do |post|
      post.images.attach(io: File.open('spec/fixtures/dummy.jpg'), filename: 'dummy.jpg', content_type: 'image/jpeg')
    end
  end
end
```
↑FactoryBot :post の定義
- `rails c`で`FactoryBot.create(:post)`などと使える
- postに必要なuserはFactoryBotが自動で呼び出されている？
- Fakerはローカライズされるっぽい

## 参考
[factory_bot_github](https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md#configure-your-test-suite)
[faker_github](https://github.com/faker-ruby/faker#generators)
