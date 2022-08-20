# Common

## IDOR in RoR

Добавление .json к пути, если у нас Ruby

```
/user_data/2341      --> 401 Unauthorized
/user_data/2341.json --> 200 OK
```

## Redirections

Обращать внимание на `redirect_to`. Пользовательский ввод не должен попадать сюда.

? Этот URL приведет к отрисовке формочки в Firefox и Opera:

```ruby
data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K
```

## RCE Flows

### Command Injection

```ruby
eval("ruby code here")
system("os command here")
`ls -al /` # (backticks contain os command)
exec("os command here")
open("\| os command here")
```

### Code Execution

```ruby
class_eval <<-RUBY, __FILE__, __LINE__ + 1
  def #{item}?
    code == '#{item.upcase}'
  end
RUBY
```

### open-uri

Если используется в коде этот пакет, то есть возможность исполнить код на стороне сервера (так как используется внутри [Kernel.open](https://ruby-doc.org/core-2.2.0/Kernel.html#method-i-open))

Пример кода уязвимого

```ruby
require "open-uri"

open(params[:url]) { |f|
    f.each_line {|line| p line}
}
```

Payloads:

```ruby
url = "|ls"

# open(params[:url]) if param[:url] =~ /^https:// , то сработает так:
url = "|touch n;\nhttps://url.com"  # Почему? читай про broken Regexp (use \A\z) в Ruby: https://homakov.blogspot.com/2012/05/saferweb-injects-in-various-ruby.html

# Если так: open(URI(params[:url])) -> Local File Read
url = "/etc/passwd"

# open(params[:url]) if URI(params[:url]).scheme == 'http'
url = "http:/../../../../../../../etc/passwd"
```

Mitigation: переходить на `openURI`.

## Unsafe Jobs

delayed jobs (e.g. ActiveJob, delayed\_job) whose classes accept sensitive data via a `perform` or `initialize` method. Jobs are serialized in plaintext, so any sensitive data they accept will be accessible in plaintext to everyone with database access. Instead, consider passing ActiveRecord instances that appropriately handle sensitive data (e.g. encrypted at rest and decrypted when the data is needed) or avoid passing in this data entirely.

```ruby
class RegistrationJob < ApplicationJob
  def perform(user:, password:, authorization_token:)
    # do something to the user with the password and authorization_token
  end
end
```

When a `RegistrationJob` gets queued, this job will get serialized, leaving both `password` and `authorization_token`accessible in plaintext. `Betterment/UnsafeJob` can be configured to flag parameters like these to discourage their use. Some ways to remediate this might be to stop passing in `password`, and to encrypt `authorization_token` and storing it alongside the user object. For example:

```ruby
class RegistrationJob < ApplicationJob
  def perform(user:)
    authorization_token = user.authorization_token.decrypt
    # do something with the authorization_token
  end
end
```

By default, this job will look at classes whose name ends with `Job` but this can be replaced with any regex.

## Macros & Regexp -> Dinamic Params

```ruby
%w(one two three)
%i(one two three)
%r{(\w+)-(\d+)}
"#{p}_name"
```

## Render / SSTI

```ruby
render <user_input>
ERB: <%= 7 * 7 %>
SLIM: #{ 7 * 7 }

Ищем raw, html_safe, safe_concat
```

SSTI: [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/6bcd2e8a6a39d26a547a70d83dfebef4c2c6f801/Server%20Side%20Template%20Injection/README.md#ruby---basic-injections](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/6bcd2e8a6a39d26a547a70d83dfebef4c2c6f801/Server%20Side%20Template%20Injection/README.md#ruby---basic-injections)
