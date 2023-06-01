# Routes

## Intro

Описаны в `/config/route.rb` и скриптах `/config/routes/...` .

Про DSL Rails Routes: [https://guides.rubyonrails.org/routing.html](https://guides.rubyonrails.org/routing.html)

Этот механизм перенаправляет url на контроллеры или Rack приложению. Позволяет не хардкодить в куче мест строки.

Если все зависимости у вас локально подтянуты, то можно посмотреть все роуты командой :

```
./bin/rails routes
```

Routing to Rack: [https://guides.rubyonrails.org/routing.html#routing-to-rack-applications](https://guides.rubyonrails.org/routing.html#routing-to-rack-applications)

## Routes directives

### resources

С помощью этой директивы можно создать эндпоинты для конкретного ресурса. Например:

```ruby
Rails.application.routes.draw do
    resources :photos
end
```

создаст семь эндпоинтов, обрабатываемых контроллером Photos:

<table><thead><tr><th width="147">HTTP Verb</th><th width="153">Path</th><th width="183">Controller#Action</th><th>Used for</th></tr></thead><tbody><tr><td>GET</td><td>/photos</td><td>photos#index</td><td>display a list of all photos</td></tr><tr><td>GET</td><td>/photos/new</td><td>photos#new</td><td>return an HTML form for creating a new photo</td></tr><tr><td>POST</td><td>/photos</td><td>photos#create</td><td>create a new photo</td></tr><tr><td>GET</td><td>/photos/:id</td><td>photos#show</td><td>display a specific photo</td></tr><tr><td>GET</td><td>/photos/:id/edit</td><td>photos#edit</td><td>return an HTML form for editing a photo</td></tr><tr><td>PATCH/PUT</td><td>/photos/:id</td><td>photos#update</td><td>update a specific photo</td></tr><tr><td>DELETE</td><td>/photos/:id</td><td>photos#destroy</td><td>delete a specific photo</td></tr></tbody></table>

Так же можно создавать подблок ресурса

```ruby
Rails.application.routes.draw do
    resources :photos do
        resources :comments
    end
end
```

Что создаст следующие эндпоинты для комментов

```
GET       /photos/:photo_id/comments
GET       /photos/:photo_id/comments/new
POST      /photos/:photo_id/comments
GET       /photos/:photo_id/comments/:id
GET       /photos/:photo_id/comments/:id/edit
PATCH/PUT /photos/:photo_id/comments/:id
DELETE    /photos/:photo_id/comments/:id
```

При определении ресурса можно указать доп опции:

```ruby
Rails.application.routes.draw do
    # Change /posts/new -> /posts/brand_new
    resources :posts, path_names: { new: "brand_new" }
    
    # Rename /blogs -> /myblogs
    resources :blogs, path: 'myblogs'
    
    # Only generate routes for the given actions.
    resources :clubs, only: :show
    resources :clubs, only: [:show, :index]
    
    # Generate all routes except for the given actions.
    resources :cat, except: [:edit, :update]
    
    # ...
end
```

### Singular Resources

В некоторых случаях нам не надо кучу эндпоинтов для ресурсов.

```ruby
Rails.application.routes.draw do
    # /profile -> Users controller and action 'show' 
    get 'profile', to: 'users#show'
    # or
    get 'profile', action: :show, controller: 'users'
    
end
```

### namespaces

Позволяет организовывать роуты в группы:

```ruby
=begin
  /admin/articles[/...]
  /admin/comments[/...]
=end
namespace :admin do
  resources :articles, :comments
end
```

### scope

Если мы не хотим, чтобы в пути был /admin, но хотим, чтобы существовала логическая группа — используем директиву scope:

```ruby
scope module: 'admin' do
  resources :articles, :comments
end

# or

resources :articles, module: 'admin'
```

If instead you want to route `/admin/articles` to `ArticlesController` (without the `Admin::` module prefix), you can specify the path with a `scope` block:

```ruby
scope '/admin' do
  resources :articles, :comments
end

# or

resources :articles, path: '/admin/articles'
```

### concern

Директива позволяет определять роуты, которые могут быть переиспользованы в других роутах. Позволяет писать меньше кода тем самым

```ruby
concern :image_attachable do
  resources :images, only: :index
end

resources :articles, concerns: :image_attachable
resources :messages, concerns: :image_attachable

# Это аналог вот этой записи:

resources :articles do
  resources :images, only: :index
end
resources :messages do
  resources :images, only: :index
end
```

Использовать это можно где угодно (например и в namespaces)

### draw

Это применяется для создания макросов. Для тех случаев, когда мы хотим вынести группу маршрутов в отдельный файл, например:

```ruby
# config/routes/admin.rb
MyApplication::Application.routes.draw do
    root to: 'index#index'
end
```

А так он будет подключен в главном route-конфиге:

```ruby
# config/route.rb
Rails.application.routes.draw do
    def draw(routes_name)
        instance_eval(File.read(Rails.root.join("config/routes/#{routes_name}.rb")))
    end
    
    draw :admin
end
```

Или по другому без оборота в routes.draw (как мне кажется это проще)

```ruby
# config/routes/admin.rb
# Здесь не надо оборачивать в Rails.application.routes.draw
# admin.rb может быть определен в любой подпапке config/routes/...
namespace :admin do
  resources :comments
end

# config/route.rb
Rails.application.routes.draw do
  draw(:admin)
end
```
