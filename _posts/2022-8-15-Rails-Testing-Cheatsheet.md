---
title: "Rails Testing Cheatsheet"
author: zacgra
layout: post
categories: Code
tags: [rails, testing, cheatsheet]
---

# Setup

## Useful testing gems

```rb
group :test do
  gem 'faker'
  gem "capybara"
  gem "selenium-webdriver"
  gem "webdrivers"
  gem 'launchy'
  gem 'shoulda-matchers'
  gem 'rspec-rails'
  gem 'factory_bot_rails', :require => false
end
```

## Rails Config

```yml
# config/database.yml
test:
  adapter: postgresql
  database: music_db_test
  pool: 5
  timeout: 5000
```

```rb
# config/environments/test.rb
config.action_mailer.default_url_options = { :host => "localhost:3000" }
```

# Model Testing

## Model Testing Config

```console
rails generate rspec:model User
```

### shoulda-matchers

> https://github.com/thoughtbot/shoulda-matchers

shoulda-matcher config

```rb
# spec/rails_helper.rb
Shoulda::Matchers.configure do |config|
    config.integrate do |with|
        with.test_framework :rspec
        with.library :rails
    end
end
```

Don't forget to require it in your class!

```rb
# spec/models/user_spec.rb
require 'shoulda/matchers'
```

### FactoryBot & Faker

FactoryBot Config

```rb
# spec/rails_helper.rb
require 'support/factory_bot'
```

```rb
# spec/support/factory_bot.rb
require 'factory_bot'

RSpec.configure do |config|
    config.include FactoryBot::Syntax::Methods
end
```

FactoryBot example definition w/ Faker

```rb
FactoryBot.define do
    factory :user do
        email { |n| Faker::Internet.email }
        password { |p| Faker::Internet.password }
    end
end
```

## Model Testing - Example Usage

shoulda-matchers

```rb
it { should validate_presence_of(:email) }
```

FactoryBot in Model

```rb
subject(:user) do
    FactoryBot.build(:user)
end
```
