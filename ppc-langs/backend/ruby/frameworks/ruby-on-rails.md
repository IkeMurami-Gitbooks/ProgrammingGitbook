# Web: Ruby on Rails

Свое простое приложение на RoR: [https://guides.rubyonrails.org/getting\_started.html](https://guides.rubyonrails.org/getting\_started.html)

Так же, тут можно найти структуру типичного приложения на rails.

По умолчанию используется http сервер puma.

## Rails Routes

Описаны в `/config/route.rb` и скриптах `/config/routes/...` .

Про DSL Rails Routes: [https://guides.rubyonrails.org/routing.html](https://guides.rubyonrails.org/routing.html)

Этот механизм перенаправляет url на контроллеры или Rack приложению. Позволяет не хардкодить в куче мест строки.

Если все зависимости у вас локально подтянуты, то можно посмотреть все роуты командой :

```
./bin/rails routes
```

Routing to Rack: [https://guides.rubyonrails.org/routing.html#routing-to-rack-applications](https://guides.rubyonrails.org/routing.html#routing-to-rack-applications)

### Routes directives

#### resources

С помощью этой директивы можно создать эндпоинты для конкретного ресурса. Например:

```ruby
Rails.application.routes.draw do
    resources :photos
end
```

создаст семь эндпоинтов, обрабатываемых контроллером Photos:

| HTTP Verb | Path             | Controller#Action | Used for                                     |
| --------- | ---------------- | ----------------- | -------------------------------------------- |
| GET       | /photos          | photos#index      | display a list of all photos                 |
| GET       | /photos/new      | photos#new        | return an HTML form for creating a new photo |
| POST      | /photos          | photos#create     | create a new photo                           |
| GET       | /photos/:id      | photos#show       | display a specific photo                     |
| GET       | /photos/:id/edit | photos#edit       | return an HTML form for editing a photo      |
| PATCH/PUT | /photos/:id      | photos#update     | update a specific photo                      |
| DELETE    | /photos/:id      | photos#destroy    | delete a specific photo                      |

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

#### Singular Resources

В некоторых случаях нам не надо кучу эндпоинтов для ресурсов.

```ruby
Rails.application.routes.draw do
    # /profile -> Users controller and action 'show' 
    get 'profile', to: 'users#show'
    # or
    get 'profile', action: :show, controller: 'users'
    
end
```

#### namespaces

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

#### scope

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

#### concern

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

