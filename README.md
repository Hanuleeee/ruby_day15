# 20180702_Day15

#### 복습

* gem file에 `carrierwave` -> 이미지 업로더

* 이미지 업로드할때 여러가지 버전설정함 (버전에 따라 썸네일 등등 으로 사용가능)

  비용아끼기위해, 용량아끼기위해..

  `version :`

* 다른 파일들이 들어와서 오류범하지 않도록 `whitelist` 만듦

* `config/initialize/fog.rb` key 저장해서 사용

  `initialize`는 서버를 동작시켰을때 최초 1회만 동작





### devise

 : 로그인 로직하나하나 다 구현하는게 아니라 이거 잼파일 하나로 모든 것이 구현되어 있다.

> https://github.com/plataformatec/devise

'*Gemfile*'

``` ruby
gem 'devise'
```



```bash
hanullllje:~ $ rails 5.0.6 new devise_app   #새로운 프로젝트 만듦

hanullllje:~/devise_app $ rails g controller home index

hanullllje:~/devise_app $ rails g devise:install
Running via Spring preloader in process 2008
      create  config/initializers/devise.rb
      create  config/locales/devise.en.yml
===============================================================================

Some setup you must do manually if you haven't yet:

  1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

  4. You can copy Devise views (for customization) to your app by running:

       rails g devise:views

===============================================================================

hanullllje:~/devise_app $ rails g devise users
```

-> 1~3 까지 하기 (2번은 `  root 'home#index'`으로 미리설정)



devise는 기본적으로 email을 id로 받는다.



'*app/views/home/index.html.erb*'

```erb
<h1>제 홈페이지에 오신분 환영!환영!</h1>

<!-- 로그인 상태 시-->
<% if user_signed_in? %> <!-- 이제 이걸 따로 정의(구현)하지않고도 사용가능 -->
    <%= current_user.email %>
    <%= link_to '로그아웃', destroy_user_session_path, method: "delete" %>
<% else %>
<!-- 로그아웃 상태 시 -->
    <%= link_to '로그인', new_user_session_path %>
<% end %>

```



```bash
hanullllje:~/devise_app $ rails g devise:views
```

: view에 많은 파일들이 만들어진다.



앞으로 로그인은 devise gem 활용

```bash
hanullllje:~/devise_app $ rails g devise:controllers
Running via Spring preloader in process 2329
Usage:
  rails generate devise:controllers SCOPE [options]

Options:
  -c, [--controllers=one two three]  # Select specific controllers to generate (confirmations, passwords, registrations, sessions, unlocks, omniauth_callbacks)

Runtime options:
  -f, [--force]                    # Overwrite files that already exist
  -p, [--pretend], [--no-pretend]  # Run but do not make any changes
  -q, [--quiet], [--no-quiet]      # Suppress status output
  -s, [--skip], [--no-skip]        # Skip files that already exist

Create inherited Devise controllers in your app/controllers folder.

Use -c to specify which controller you want to overwrite.
If you do no specify a controller, all controllers will be created.
For example:

  rails generate devise:controllers users -c=sessions

This will create a controller class at app/controllers/users/sessions_controller.rb like this:

  class Users::ConfirmationsController < Devise::ConfirmationsController
    content...
  end
  
hanullllje:~/devise_app $ rails g devise:controllers users
Running via Spring preloader in process 2343
      create  app/controllers/users/confirmations_controller.rb
      create  app/controllers/users/passwords_controller.rb
      create  app/controllers/users/registrations_controller.rb
      create  app/controllers/users/sessions_controller.rb
      create  app/controllers/users/unlocks_controller.rb
      create  app/controllers/users/omniauth_callbacks_controller.rb
===============================================================================

Some setup you must do manually if you haven't yet:

  Ensure you have overridden routes for generated controllers in your routes.rb.
  For example:

    Rails.application.routes.draw do
      devise_for :users, controllers: {
        sessions: 'users/sessions'
      }
    end

===============================================================================
```

*app/controllers/users* 에 devise 상속받은 users controller들이 생성된다.