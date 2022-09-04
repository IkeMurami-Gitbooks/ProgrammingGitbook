# SQLi

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
https://rails-sqli.org/](https://rails-sqli.org/)\
[https://rorsecurity.info/portfolio/ruby-on-rails-sql-injection-cheat-sheet](https://rorsecurity.info/portfolio/ruby-on-rails-sql-injection-cheat-sheet)
