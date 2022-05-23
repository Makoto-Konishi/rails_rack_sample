config.ru
```ruby
# This file is used by Rack-based servers to start the application.

require_relative 'config/environment'

run Rails.application
```
- rackが利用するエントリーポイント用のファイル名として一般的に使用される`config.ru`が使用されていることから、rackアプリケーションであることがわかる。
- `config/environment.rb`を読み込んでおり、ここにRailsとしての前処理やRailsオブジェクトの構築を実行している
    ```ruby
    # config/environment.rb
  
    # Load the Rails application.
    require_relative 'application'

    # Initialize the Rails application.
    Rails.application.initialize!

    ```
- runにオブジェクトを渡す構成になっているのと、`environment.rb`を読み込んでいることから,ここで最終的に実行していると言える
- rackアプリケーションなので、`rails s`だけでなく、`rackup`でも起動できる。この際、9292番ポートで立ち上がる。
    ```
    $ rackup
    Puma starting in single mode...
    * Version 4.3.12 (ruby 2.7.1-p83), codename: Mysterious Traveller
    * Min threads: 5, max threads: 5
    * Environment: development
    * Listening on tcp://127.0.0.1:9292
    * Listening on tcp://[::1]:9292
    Use Ctrl-C to stop

    ```
  
## 自作ミドルウェアの追加

`lib/middlewares/upcase_middleware.rb`
```ruby
class UpcaseMiddleware
  def initialize(app)
    @app = app
  end

  def call(env)
    status, headers, body = @app.call(env)
    body.each { |s| s.gsub!(/ruby/i, 'RUBY') }
    [status, headers, body]
  end
end
```

`config/environments/development.rb`
```ruby
require 'middlewares/upcase_middleware'

Rails.application.configure do
  
  # 省略
  
  config.middleware.use UpcaseMiddleware # 自作ミドルウェアの読み込み
end

```
追加されていることがわかる。
[![Image from Gyazo](https://i.gyazo.com/08efab792064a5a47e9ac8331b1d61d6.png)](https://gyazo.com/08efab792064a5a47e9ac8331b1d61d6)

`Rack::ETag`の後ろに追加してみる。
```ruby
config.middleware.insert_after Rack::ETag, UpcaseMiddleware
```
[![Image from Gyazo](https://i.gyazo.com/0132e87f189c268b18eebaffe83fe9c0.png)](https://gyazo.com/0132e87f189c268b18eebaffe83fe9c0)