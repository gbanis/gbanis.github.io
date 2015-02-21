---
layout:     post
title:      "Ruby on Rails API Cheat Sheet"
description:  "A list of my notes from the Rails for Zombies course on CodeSchool."
date:       2014-11-02 14:00:00
tags:
  - coding
  - cheat sheets
  - rails
author:     George Banis
---

## Routes

```ruby
resources :zombies
```

```bash
$ rake routes

     Prefix Verb    URI Pattern                 Controller#Action
    zombies GET     /zombies(.:format)          zombies#index
            POST    /zombies(.:format)          zombies#create
 new_zombie GET     /zombies/new(.:format)      zombies#new
edit_zombie GET     /zombies/:id/edit(.:format) zombies#edit
     zombie GET     /zombies/:id(.:format)      zombies#show
            PATCH   /zombies/:id(.:format)      zombies#update
            PUT     /zombies/:id(.:format)      zombies#update
            DELETE  /zombies/:id(.:format)      zombies#destroy
```

### Except
```ruby
resources :zombies, except: :destroy
# same as above, but destroy action is unreachable
```

### Only
```ruby
resources :zombies, only: :index
# only allows index action
```

```bash
$ rake routes

     Prefix Verb    URI Pattern                 Controller#Action
    zombies GET     /zombies(.:format)          zombies#index
```

### Multitple Actions

```ruby
# Example
resources :zombies only: [:index, :show]
resources :humans, except: [:destroy, :edit, :update]
```

```bash
   Prefix Verb  URI Pattern             Controller#Action
  zombies GET   /zombies(.:format)      zombies#index
   zombie GET   /zombies/:id(.:format)  zombies#show
   humans GET   /humans(.:format)       humans#index
          POST  /humans(.:format)       humans#create
new_human GET   /humans/new(.:format)   humans#new
    human GET   /humans/:id(.:format)   humans#show
```

### Using WITH_OPTIONS On Routes

```ruby
# this:
resources :zombies, only: :index
resources :humans, only: :index
resources :medical_kits, only: :index

# is the same as:
with_options only: :index do |list_only|
  list_only.resources :zombies
  list_only.resources :humans
  list_only.resources :medical_kits
end
```

### Using Constraints to Enforce Subdomain
To point to subdomain `http://api.cs-zombies.com/zombies` you can use either of the following:

```ruby
# this:
resources :episodes
resources :zombies, constraints: { subdomain: 'api' }
resources :humans, constraints: { subdomain: 'api' }

# is the same as:
resources :episodes

constraints subdomain: 'api' do
 resources :zombies
 resources :humans
end
```

### Subdomains in Development

Configuring support for subdomains in dev.

The following code:

```ruby
# /etc/hosts
127.0.0.1    cs-zombies-dev.com
127.0.0.1    api.cs-zombies-dev.com
```

provides support for these URLs on local (dev) machine:

```ruby
http://cs-zombies-dev.com:3000 # you still need the port number
http://api.cs-zombies-dev.com:3000
```

### Organizing Web and API Controllers

```ruby
constraints subdomain: 'api' do
  # 'namespace' adds the 'api/' prefix
  namespace :api do
    resources :zombies
  end
end

resources :pages
```

```bash
     Prefix Verb URI Pattern            Controller#Action
    zombies GET  /zombies(.:format)     api/zombies#index {:subdomain=>"api"}
            POST /zombies(.:format)     api/zombies#create {:subdomain=>"api"}
 new_zombie GET  /zombies/new(.:format) api/zombies#new {:subdomain=>"api"}
      ...
      pages GET  /pages(.:format)       pages#index
            POST /pages(.:format)       pages#create
   new_page GET  /pages/new(.:format)   pages#new
      ...
 ```

```ruby
# app/controllers/api/zombies_controller.rb
module Api
  class ZombiesController < ApplicationController
  end
end
```

```ruby
# app/controllers/pages_controller.rb
class PagesController < ApplicationController
end
```

#### Removing Duplication from the URL

To remove duplication from the subdomain and trailing /path:

```ruby
constraints subdomain: 'api' do
  namespace :api, path: '/' do  # 'path' reassigns to root
    resources :zombies
  end
end

# which is the same as:
namespace :api, path: '/', constraints: { subdomain: 'api' } do
  resources :zombies
  resources :humans
end
```

#### API (vs Api) Case Consistency

Use 'inflections' so we can write API (all caps) without any Rails issues

```ruby
# config/initializers/inflections.rb
ActiveSupport::Inflector.inflections(:en) do |inflect|
  inflect.acronym 'API'
end

# this allows you to do:

# app/controllers/api/zombies_controller.rb
module API
  class ZombiesController < ApplicationController
  end
end
```

### Credits

[http://railsapis.codeschool.com/](http://railsapis.codeschool.com/)
