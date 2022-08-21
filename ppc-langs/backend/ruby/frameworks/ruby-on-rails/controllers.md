# Controllers

## Intro

После того, как определили routes, определяют контроллеры (это C в MVC паттерне). Эта сущность находится между слоем отображения (View) и слоем моделей данных (Model).

В Ruby контроллер — класс, наследованный от `ApplicationController` (наследован от `ActionController::Base`). Когда приложение получает запрос, маршрутизация определяет контроллер и действие для запуска; Rails создает инстанс контроллера и вызывает метод, соответствующий объявленному действию

## Пример контроллера

```ruby
# resources :clients, only: :new
# -> /clients/new -> Add new client

class ClientsController < ApplicationController
    def new
        @client = Client.new # @client becomes accessible in the view 
                             # Автоматом доступен модель Client
    end
end
```

## Параметры в контроллере

Доступ к GET/POST параметрам осуществляется через [params](https://api.rubyonrails.org/v7.0.3.1/classes/ActionController/StrongParameters.html#method-i-params)

```ruby
class ClientsController < ApplicationController
  # This action uses query string parameters because it gets run
  # by an HTTP GET request, but this does not make any difference
  # to how the parameters are accessed. The URL for
  # this action would look like this to list activated
  # clients: /clients?status=activated
  def index
    if params[:status] == "activated"
      @clients = Client.activated
    else
      @clients = Client.inactivated
    end
  end

  # This action uses POST parameters. They are most likely coming
  # from an HTML form that the user has submitted. The URL for
  # this RESTful request will be "/clients", and the data will be
  # sent as part of the request body.
  def create
    @client = Client.new(params[:client])
    if @client.save
      redirect_to @client
    else
      # This line overrides the default rendering behavior, which
      # would have been to render the "create" view.
      render "new"
    end
  end
end
```

### rescue\_from

Позволяет обработать определенный тип эксепшена в текущем и во всех дочерних контроллерах кастомным способом

```ruby
class WebController < ApplicationController
    rescue_from ActiveRecord::RecordNotFound, with: :error_not_found
    
    def error_not_found(error)
        return render template: 'errors/404', layout: false, status: :not_found
    end
end
```

## Strong Parameter API

### permit

Функция у params, которая позволяет вернуть только указанный список параметров. Делается для ограничения последующего использования.

```ruby
params.permit(:id)

def product_params
    params.require(:product).permit(:name, data: {})
end
```

### require

Указывает, что параметр обязателен

## Session

Your application has a session for each user in. The session is only available in the controller and the view and can use one of several of different storage mechanisms:

* [`ActionDispatch::Session::CookieStore`](https://api.rubyonrails.org/v7.0.3.1/classes/ActionDispatch/Session/CookieStore.html) - Stores everything on the client.
* [`ActionDispatch::Session::CacheStore`](https://api.rubyonrails.org/v7.0.3.1/classes/ActionDispatch/Session/CacheStore.html) - Stores the data in the Rails cache.
* `ActionDispatch::Session::ActiveRecordStore` - Stores the data in a database using Active Record (requires the `activerecord-session_store` gem).
* [`ActionDispatch::Session::MemCacheStore`](https://api.rubyonrails.org/v7.0.3.1/classes/ActionDispatch/Session/MemCacheStore.html) - Stores the data in a memcached cluster (this is a legacy implementation; consider using `CacheStore` instead).

All session stores use a cookie to store a unique ID for each session.

If you need a different session storage mechanism, you can change it in an initializer:

```ruby
# Use the database for sessions instead of the cookie-based default,
# which shouldn't be used to store highly confidential information
# (create the session table with "rails g active_record:session_migration")
# Rails.application.config.session_store :active_record_store
```

Rails sets up a session key (the name of the cookie) when signing the session data. These can also be changed in an initializer:

```ruby
# Be sure to restart your server when you modify this file.
Rails.application.config.session_store :cookie_store, key: '_your_app_session'
```

You can also pass a `:domain` key and specify the domain name for the cookie:

```ruby
# Be sure to restart your server when you modify this file.
Rails.application.config.session_store :cookie_store, key: '_your_app_session', domain: ".example.com"
```

Rails sets up (for the CookieStore) a secret key used for signing the session data in `config/credentials.yml.enc`. This can be changed with `bin/rails credentials:edit`.

### Accessing the Session

Session values are stored using key/value pairs like a hash:

```ruby
class ApplicationController < ActionController::Base

  private

  # Finds the User with the ID stored in the session with the key
  # :current_user_id This is a common way to handle user login in
  # a Rails application; logging in sets the session value and
  # logging out removes it.
  def current_user
    @_current_user ||= session[:current_user_id] &&
      User.find_by(id: session[:current_user_id])
  end
end
```

To store something in the session, just assign it to the key like a hash:

```ruby
class LoginsController < ApplicationController
  # "Create" a login, aka "log the user in"
  def create
    if user = User.authenticate(params[:username], params[:password])
      # Save the user ID in the session so it can be used in
      # subsequent requests
      session[:current_user_id] = user.id
      redirect_to root_url
    end
  end
end
```

To remove something from the session, delete the key/value pair:

```ruby
class LoginsController < ApplicationController
  # "Delete" a login, aka "log the user out"
  def destroy
    # Remove the user id from the session
    session.delete(:current_user_id)
    # Clear the memoized current user
    @_current_user = nil
    redirect_to root_url
  end
end
```

### flash

Есть объект flash, который позволяет передать значение только следующему запросу

### Cookies

Отдельный объект для доступа к cookie.

Можно определелить в конфиге, как будут сериализованы значения

```ruby
Rails.application.config.action_dispatch.cookies_serializer = :json
# :json — сериализация по умолчанию для новых приложений, для старый — :marshal.

# Custom serialized (implement load и dump методы):
Rails.application.config.action_dispatch.cookies_serializer = MyCustomSerializer
```

### Filters

Это методы, которые вызываются before, after и around действия контроллера. Фильтры наследуются. Если определить фильтр в ApplicationController, то он будет срабатывать в каждом контроллере вашего приложения:

```ruby
class ApplicationController < ActionController::Base
  before_action :require_login

  private

  def require_login
    unless logged_in?
      flash[:error] = "You must be logged in to access this section"
      redirect_to new_login_url # halts request cycle
    end
  end
end
```

Если мы хотим скипнуть какой-нибудь фильтр для нашего контроллера:

```ruby
class LoginsController < ApplicationController
  skip_before_action :require_login, only: [:new, :create]
end
```
