---
layout: post
title:      "Planning out a take home Rails Application"
date:       2021-03-14 19:57:14 -0400
permalink:  planning_out_a_take_home_rails_application
---


This week I had a successful first interview with a company and was given a take home Rails challenge. I thought I should go over how I went about approaching the challenge and show off what I produced.

Firstly I was given a week for this task but it was suggested that it should only take me 6 or so hours to complete. As an unemployed job seeker with a great motivation to find my first developer job I knew that I wasn't tied to this and would happily put in whatever required to code out the solution required - and hopefully in a way that would impress the employer.

So the challenge was to create a Rails webapp for a cinema company who wanted to have customers buying tickets on their website. In particular, a customer could visit the website and see all the movies and their showtimes. The Cinema had various auditoriums which each had a capacity. If a showing had reached capacity by customers buying all of the tickets then the showing displayed sold out and customers could no longer buy tickets to that show.

So to tackle this first paragraph of requirements I wanted to plan out my models -  which is always the first thing I work out as making changes to the database tables can be tricky and annoying. 
The way I thought best was to have Users, Movies, Showings, Auditoriums, and Orders. At this stage I knew I could put together the relationships between all of these models as well. 
So, Users could have many orders, Movies has many showings as I needed to display all showings of each movie in an index or homepage. Auditoriums has many showings and has many movies through showings  - in case I needed to show the data via a auditorium index. Showings belonged to movies and a auditorium they were also set at a certain time and due to the relationship with the auditorium I could easily find the capacity of each showing. Finally, orders belonged to a user and a showing. 
I decided I didn't need a model for theater, as this challenge only required the website to be built for a single theater or cinemas. If I wanted to expand or maybe make the site more scalable I would could add this model in later and build it out further.

What would you have done? Next week I'll be back to explain the next set of requirements.
