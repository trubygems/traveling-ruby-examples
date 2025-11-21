# traveling-ruby demos

## Scenarioes

1. Ruby script with no external dependencies
2. Ruby app with ruby gem dependency (faker)
3. Ruby app with ruby & native gem dependency (faker + sqlite3)
4. Rack app with ruby & native gem dependency (pact_broker)
5. Rails app with ruby & native gem dependency (rails)

## Building/Testing a demo app

See the GitHub Actions build script [.github/workflows/package.yml](.github/workflows/package.yml) to see the demos built and tested cross platform.

1. cd `examples/<demo>`
2. `bundle exec rake package` - package app for all platforms
    - `bundle exec rake package:darwin-arm64` for indiv platform
    - `DIR_ONLY=1 bundle exec rake package:darwin-arm64` to skip packaging, useful for testing
3. To test each demo app, from the root of this repo
    - `APP_NAME=<demo> ./scripts/unpack-and-test.sh`

## Caveats

1. You can only use applications with native gems, and versions that have been published for the required traveling-ruby release. If other gems are required, these can be built by forking the repository and updating the Gemfile.

2. If using bundler and rubygems (anything other than a simple script that uses [standard gems](https://stdgems.org/)), you will need to write a `bundler-config` file to your `packaging` directory

        BUNDLE_PATH: .
        BUNDLE_WITHOUT: development
        BUNDLE_DISABLE_SHARED_GEMS: '1'

3. Add all supported platforms to your Gemfile.lock

        PLATFORMS
        aarch64-linux
        aarch64-mingw-ucrt
        arm64-darwin
        arm64-darwin-20
        arm64-darwin-21
        arm64-darwin-22
        arm64-darwin-23
        ruby
        x64-mingw-ucrt
        x86_64-darwin
        x86_64-darwin-20
        x86_64-darwin-21
        x86_64-darwin-22
        x86_64-darwin-23
        x86_64-linux

4. Remove any platform/arch specific resolutions from your Gemfile.lock

    - example

            ffi (1.17.2)
            ffi (1.17.2-aarch64-linux-gnu)
            ffi (1.17.2-arm64-darwin)
            ffi (1.17.2-x64-mingw-ucrt)
            ffi (1.17.2-x86_64-darwin)
            ffi (1.17.2-x86_64-linux-gnu)

    - should be updated to

            ffi (1.17.2)

5. A wrapper file should be created for `unix` and `windows` platforms, depending on your required level of support

## Script

### Structure

    hello
    ├── Rakefile
    ├── hello.rb
    └── packaging
        ├── wrapper.bat
        └── wrapper.sh

### Testing

1. Print running platform & arch along with fake name

## Gem

### Structure

    gem
    ├── Gemfile
    ├── Gemfile.lock
    ├── Rakefile
    ├── gem.rb
    └── packaging
        ├── Gemfile
        ├── Gemfile.lock
        ├── bundler-config
        ├── wrapper.bat
        └── wrapper.sh

### Testing

1. Generate fake name with faker gem
2. Print running platform & arch along with fake name

## Native Gem

### Structure

    native
    ├── Gemfile
    ├── Gemfile.lock
    ├── Rakefile
    ├── native.rb
    └── packaging
        ├── Gemfile
        ├── Gemfile.lock
        ├── bundler-config
        ├── wrapper.bat
        └── wrapper.sh

### Testing

1. Generate fake name with faker gem
2. Print running platform & arch along with fake name
3. Create sqlite3 db
4. Insert fake name into sqlite3 db
5. Read database contents back

## Rack app

### Structure

    pact_broker
    ├── Gemfile
    ├── Gemfile.lock
    ├── Rakefile
    ├── packaging
    │   ├── Gemfile
    │   ├── Gemfile.lock
    │   ├── GettingStartedOrderWeb-GettingStartedOrderApi.json
    │   ├── bundler-config
    │   ├── config.ru
    │   ├── wrapper.bat
    │   └── wrapper.sh
    └── pact_broker.rb

### Testing

1. Pact broker (Rake based) app is configured via `config.ru`
2. Pact broker app is started cross platform
3. curl request is made to the homepage to check server is available
4. Pact broker client app (in ruby) publishes pact contract file to pact broker

## Rails app

### Structure

    rails
    ├── Gemfile
    ├── Gemfile.lock
    ├── Rakefile
    └── packaging
        ├── Gemfile
        ├── Gemfile.lock
        ├── bundler-config
        ├── wrapper.bat
        └── wrapper.sh

### Testing

1. New rails app is created during packaging
2. Rails app is started cross platform
3. curl request is made to the homepage
