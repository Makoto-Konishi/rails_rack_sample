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