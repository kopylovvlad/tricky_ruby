## Rails 6 load order

The source code of Rails::Application gives us a quick reminder of how the boot process goes:

01. require `config/boot.rb` to setup load paths
02. require `railties` and engines
03. Define `Rails.application` as `class MyApp::Application < Rails::Application`
04. Run `config.before_configuration` callbacks
05. Load `config/environments/ENV.rb`
06. Run `config.before_initialize` callbacks
07. Run `Railtie#initializer` defined by railties, engines and application. One by one, each engine sets up its load paths, routes and runs its `config/initializers/*` files.
08. Custom `Railtie#initializers` added by railties, engines and applications are executed
09. Build the middleware stack and run `to_prepare` callbacks
10. Run `config.before_eager_load` and `eager_load!` if `eager_load` is true
11. Run `config.after_initialize` callbacks


Said differently:

- Set `APP_PATH` to `./config/application.rb`
- Set `ENV['BUNDLE_GEMFILE']` to `./Gemfile`
- Setup Bundler (without requiring gems yet)
- Initialize a new `Rails::Server` (subclass of `Rack::Server`)
- Require all Rails components (`ActiveRecord`, `ActionPack`, etc.)
- Require all gems from your `Gemfile`
- Define an `Application` that is a subclass of `Rails::Application`
- Change directory to the root of your Rails application
- Start the `Rails::Server` initialized earlier
- Run Rails hooks in an orderly manner (load configuration, run initializers, etc.)
- Your server is now waiting for requests!

[Source](https://makandracards.com/makandra/678-load-order-of-the-environment)
