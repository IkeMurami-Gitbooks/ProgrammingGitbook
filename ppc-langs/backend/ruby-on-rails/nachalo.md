# Начало

Свое простое приложение на RoR: [https://guides.rubyonrails.org/getting\_started.html](https://guides.rubyonrails.org/getting\_started.html)

## Install

через rbenv ([https://github.com/rbenv/rbenv](https://github.com/rbenv/rbenv))

```
// Install
$ brew install rbenv
(with rbenv also will install ruby-build automaticly)

// Upgrade
$ brew upgrade rbenv ruby-build

// Init
$ rbenv init -

// Check is OK via rbenv-doctor
$ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash

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

// Gems
// 1. Check target version of your project
// 2. Set local version
$ rbenv local 2.0.0-p247
// 3. Install needed gem (we don't need sudo permissions for that)
$ gem install bundler
// 4. Check the location where gems are being installed
$ gem env home
```
