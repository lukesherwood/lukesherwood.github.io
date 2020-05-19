---
layout: post
title:      "Setting up a basic rails app"
date:       2020-05-19 01:23:26 +0000
permalink:  setting_up_a_basic_rails_app
---


One of the best things about rails is how quickly and easy you can get an app up and working. However, I still find it daunting after I have a basic scaffold app by using either 'rails g scaffold' or creating the controller, models, views and routing your self.  I won't go into detail in this first part of app building as it will vary too much based on what kind of app you are creating, but rather, I will focus on installing the 3 best gems and having an app safe and functioning with some styling.

FYI I am using VS Code with Ubuntu for Windows

'rails new recipebook'

While thats running get your github repo ready.  [github](https://github.com/new) 
Remember Rails creates readme and gitignore files so no need to add these again.

CD into newly created app folder

Follow instructions from Github...

> git add .
> git commit -m "first commit"
> git remote add origin git@github.com:lukesherwood/recipebook.git
> git push -u origin master

refresh github page and everything should be uploaded.

when I run rails s I recieve the following warning

> The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.

I hate warnings so to get rid of it just remove the following lines from gemfile and bundle install

>  Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

'rails s' and visit [localhost](http://localhost:3000/) should show you the ruby on rails page.

close the server and generate the required scaffolds with the following...

> rails generate scaffold 'InsertModelName' 'InsertTableColumnName1':'ColumnNameType1'  'InsertTableColumnName2':'ColumnNameType2' etc
> rails generate scaffold book title:text author:string

remember to rails db:migrate
Don't create a scaffold for the User model yet. Which brings us to our first important gem - [Devise]https://github.com/heartcombo/devise)

add gem 'devise' to your gemfile and bundle
then rails generate devise:install
rails generate devise USER
Then run rails db:migrate

visit [New User](http://localhost:3000/users/sign_up)

you should have working users at this point but everything will be rather ugly. Lets add some styling with the Bootstrap gem.
add to your gemfile then bundle:
gem 'sprockets-rails', :require => 'sprockets/railtie'
gem 'devise-bootstrap-views'
gem 'bootstrap', '~> 4.5.0'
gem 'jquery-rails'

add the following to \app\javascript\packs\application.js

> //= require jquery3
//= require popper
//= require bootstrap-sprockets

rename the file \app\assets\stylesheets\application.css to application.scss
add @import "bootstrap";  and remove all the *= require and *= requiretree statements

restart rails server and the new user page is now stylised

Now to create styled and easy to create forms  to our app add [Simple Form](https://github.com/heartcombo/simple_form) to your gemfile and bundle 
> gem 'simple_form'
then run rails generate simple_form:install --bootstrap

now if you remove the scaffold form and create your own with simple form it'll be stylised

checkout [Bootstrap Documentation](https://getbootstrap.com/docs/4.5/getting-started/introduction/) for more information about styling with bootstrap








