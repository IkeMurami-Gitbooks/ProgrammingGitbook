# Getting Started

## rbenv

Самый простой способ управления версиями Ruby на хостовой системе — [rbenv](https://github.com/rbenv/rbenv)

### Install

```
$ brew install rbenv
(with rbenv also will install ruby-build automaticly)

// Upgrade
$ brew upgrade rbenv ruby-build

// Init
$ rbenv init -

// Check all is OK via rbenv-doctor
$ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash
```

### Set and check Ruby version

```
// Set ruby version
// 1. list latest stable versions:
$ rbenv install -l

// 2. list all local versions:
$ rbenv install -L

// 3. install a Ruby version:
$ rbenv install 2.0.0-p247

// 4. set version ruby global or local
$ rbenv global 2.0.0-p247
$ rbenv local 2.0.0-p247

// Check target version of your project
$ rbenv version

// All versions
$ rbenv versions
```

### Notes

```
// Полный путь до исполняемого бинаря
$ rbenv which gem

// Все версии окружений, в которых установлена команда
$ rbenv whence gem
```

## Dependencies

### Gems

Gem — сторонний пакет / библиотека Ruby. **Лучше** управлять зависимостями через **Bundler** (следующий пункт)

Работа с зависимостями:

```
// 1. Check target version of your project

// 2. Set local version
$ rbenv local 2.0.0-p247

// 4. Check the location where gems are being installed
$ gem env home
// Если вдруг не увидели здесь нужный путь (до окружения 2.0.0-p247, например) 
// — перезапустите консоль

// 3. Install needed gem (we don't need sudo permissions for that)
$ gem install bundler
```

### Bundler

Все зависимости лучше устанавливать и контролировать через Bundler (например, fastlane)

```
// Install Bundler
$ gem install bundler

// Update dependencies (Будут созданы ./Gemfile и ./Gemfile.lock)
$ bundle update

// Add & Install dependencies
$ bundle add fastlane

// Install Dependencies
$ bundle install

// Update
$ bundle update fastlane
```

## Run Ruby Code

irb console:

```
$ irb
irb(main):001:0> puts 'Hello Ruby'
Hello Ruby
=> nil
irb(main):002:0>
```

ruby:

```
$ ruby -e 'print "Hello Ruby!\n"' -e '1+1'
$ ruby hello.rb
$ ./hello.rb
```
