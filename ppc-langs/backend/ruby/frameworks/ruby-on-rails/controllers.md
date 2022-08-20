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
