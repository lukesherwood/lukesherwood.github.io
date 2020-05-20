---
layout: post
title:      "Setting up a basic rails app"
date:       2020-05-18 21:23:27 -0400
permalink:  setting_up_a_basic_rails_app
---


When it came time to start my rails project I decided early on that I wanted to get to grips with the gems and libraries to make an attempt to create a more professional looking app. I spent a lot of time deep diving into documentation and Stack Overflow when I got stuck when trying to get these gems functioning. Here is my attempt to detail what I would have liked be able to follow the first time around.

FYI I am using VS Code with Ubuntu for Windows

## Creating a Rails App

'rails new recipebook'

While thats running get your github repo ready.  [new github repo](https://github.com/new) 

Remember Rails creates readme and gitignore files so no need to add these again.

CD into newly created app folder

Follow instructions from Github...

> git add .
> git commit -m "first commit"
> git remote add origin .........
> git push -u origin master

refresh github page and everything should be uploaded.

when I run rails s I recieve the following warning

The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.

I hate warnings so to get rid of it just remove the following lines from gemfile and bundle install

> Windows does not include zoneinfo files, so bundle the tzinfo-data gem
> gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

'rails s' and visit [localhost](http://localhost:3000/) if eveything is working it should show you the ruby on rails page.

close the server and generate the required scaffolds

Don't create a scaffold for the User model yet.

remember to rails db:migrate


## User Model and Authentication with Devise
[Devise]https://github.com/heartcombo/devise)

> add gem 'devise' to your gemfile and bundle
> rails generate devise:install
> rails generate devise USER
> rails db:migrate

visit [New User](http://localhost:3000/users/sign_up)

you should have working users at this point but everything will be rather ugly. Lets add some styling with the Bootstrap gem.

## CSS styling without the hardwork - Bootstrap
add to your gemfile then bundle:
> gem 'sprockets-rails', :require => 'sprockets/railtie'
> gem 'devise-bootstrap-views'
> gem 'bootstrap', '~> 4.5.0'
> gem 'jquery-rails'

add the following to \app\javascript\packs\application.js
> //= require jquery3
> //= require popper
> //= require bootstrap-sprockets

rename the file \app\assets\stylesheets\application.css to application.scss
add @import "bootstrap";  and remove all the *= require and *= requiretree statements

restart rails server and the new user page is now stylised

## Simple forms with Simple_forms

Now you can easily integrate bootstrap into forms by adding [Simple Form](https://github.com/heartcombo/simple_form) to your gemfile and bundle 
> gem 'simple_form'
> rails generate simple_form:install --bootstrap

now if you remove the scaffold form and create your own with simple form it'll be stylised

checkout [Bootstrap Documentation](https://getbootstrap.com/docs/4.5/getting-started/introduction/) for more information about styling with bootstrap


## Other gems worth checking out

*** Omniauth** - For creating a Facebook/Github etc signup/login
*** Cocoon** - Add extra sections to your forms when they are out of space or to add extra items
*** Pundit** - Adding authorization to specify access to certain views and actions
*** Better Errors** - a nice development tool to better understand errors and to never have to write "raise params.inspect" again.





