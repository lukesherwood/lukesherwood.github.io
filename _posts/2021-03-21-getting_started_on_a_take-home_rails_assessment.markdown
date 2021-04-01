---
layout: post
title:      "Part 2 Getting started on a take-home Rails assessment"
date:       2021-03-21 09:01:15 -0400
permalink:  getting_started_on_a_take-home_rails_assessment
---


Last week I went over how I planned out my models and relationships for a take home test I was given by a prospective employer. If you haven't read it yet, check it out [here](https://lukesherwood.github.io/planning_out_a_take_home_rails_application). I proceeded to set up the Rails app with those models, routes and relationships and then after checking they were all functioning correctly within the console I moved onto the next section
of requirements was an admin page to manage the website. 

> The theater owner needs a way to manage the movies playing and seating capacities. She should be able to add auditoriums and seating capacities. In addition, the movies change all the time so our client should be able to set which movie is showing in which auditorium.
> 
> The theater owner also wants to keep track of her sales, so the order information should be saved to the database. She wants to see a list of all orders and a list of orders for each movie. We don't need any authentication on the app for now.

I wanted to set this up next so it would be easy to create data that I could work with when designing the frontpage. The important point here which saved a lot of time is that there is no required authentication, and without that there can't be any protected paths so I would design an open route - '/users/admin' where all these requirements would be solved.

I decided to tackle auditoriums creation and adjusting the capacity first. Auditorium only had names and capacity so a small button which routed to a new form with those fields was all that was needed. I thought that once an auditorium was in the system that the only thing really needed to change was perhaps the capacity - so I made that easy to do with a jQuery event-listener which rendered a small form once clicked. This was a bit of a fun process as I hadn't worked with jQuery before - so I actually took some time to learn the fundamentals as its purpose was exactly to do things like this and it was already installed as a dependency of Bootstrap. The function below is in '\app\javascript\packs\application.js'
```
    $('[data-js-hide-link]').click(function(event){
      event.preventDefault();
      var id = event.target.getAttribute('key')
      $("#edit_auditorium_" + id).toggle()
    });
```
With that set up and functional I went on to creating a form to create a showing - inputs for time/date, auditorium and movie. I did this and realized I hadn't made any movies to select so I went back and added seed data for movies. There wasn't a requirement to add or edit movies so I thought as long as there is a collection in the database to choose from then that'll work. I was using the  simple_form gem to create forms so the code was....simple.
```
<%= simple_form_for [@showing] do |f| %>
    <%= f.input :time, :as => :datetime %>	
    <%= f.association :auditorium %>
    <%= f.association :movie %>
    <%= f.button :submit, class:"btn btn-primary" %>
<% end%>
```

I went through and added the display of auditoriums, their showings and the movies/times. There wasn't a requirement for any frontend design so it didn't need to be pretty - but it did need to be functional. 

With those basic forms set up I could easily populate the database with some data while testing them the features as I was building them. Looking back at the requirements for the admin page I'd pretty quickly completed most of them, apart from the order and sale lists. I thought I'd do that right at the end once I had worked out how to create orders on the homepage. 

Next week I'll go over creating the homepage and designing a way that users can purchase tickets to our movie showings.

