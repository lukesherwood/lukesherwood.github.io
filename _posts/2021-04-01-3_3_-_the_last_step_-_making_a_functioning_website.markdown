---
layout: post
title:      "3/3 - The last step - making a functioning website"
date:       2021-04-01 16:37:00 +0000
permalink:  3_3_-_the_last_step_-_making_a_functioning_website
---


If you haven't already check out the other posts in this series where I've gone through my steps creating an app for a real-life rails take-home assessment. [Part 1](https://lukesherwood.github.io/planning_out_a_take_home_rails_application), [Part 2](https://lukesherwood.github.io/getting_started_on_a_take-home_rails_assessment).

I left last week with a working admin page where I could organize the auditoriums and the showings of movies. But I was having a hard time navigating the site so I set up a quick bootstrap navbar with links to the homepage and the admin page.

The homepage was where all the action for users would happen so I wanted it to be clean and to the point but to still contain everything needed. I added a navbar - the bootstrap standard one, so I could quickly switch between the admin and home pages. I then iterated through all movies, displaying the title, an image, and a little description. 

Each movie had a section for showings, and if there was showings it would display the time and date, and which auditorium it was showing at. I think the key piece in the requirements of this assessment was to create a function to only show showings that weren't sold out. It's not a particularly hard function: simply show a sold out sign if ticket sales = the capacity of the auditorium. I also thought it would be a good idea to show how many tickets were available to buy so I could easily see how many were left. So I created these two methods for showings.
```
  def number_of_seats_remaining
    self.auditorium.capacity - self.orders.length
  end

  def sold_out?
    number_of_seats_remaining <= 0
  end
```
The weird thing that is in the sold_out? method is <= 0. 

When would there be a case of there being less seats remaining than 0? 

Answer: When the developer didn't stop users from buying tickets once the showing was sold out!

OK, so I went and fixed that from occurring, but left the method as it was, who knows if a mass ordering event could break our site. Here's the orders create method where I did that:
```
	def create
		@showing = Showing.find(params[:showing_id])
		if @showing.sold_out?
			redirect_to '/', notice: "Sorry this showing is sold out"
		else
			... continue with the order
```

The last stage of the process was being able to create an order, with a credit card number, expiry, name and email.
So I set up a button to take customers to a new order form where they could enter this information. As soon as I saw credit card information entry there were red flags going off in my head, I knew it was extremely unsafe to save, or even send credit card information to and from the database. So I wanted to be able to complete the validations on the front-end without ever sending the data to my database. 

Unfortunately I was required to validate both the credit card number and the expiry date. Have you seen some algorithms for validating credit cards? Check out the [Luhn algorithm](https://gist.github.com/trietle/62fe5c4378780b04dec1) in Ruby. I was meant to complete this whole application in 6 hours so I didn't have time to implement this and I also didn't want to create a valid credit card number to be able to use while I was testing the functionality of the app. I settled for a quick check of length and that all characters are numbers, same for the expiry. Sadly the easiest way to do this was to send the data to the backend to be checked for validity - concerning, but I wasn't taking cvv numbers and surely noone would enter their actual credit card number when assessing the app. right?!
```
validates :credit_card_number, numericality: true, presence: true, length: { in: 13..16 }
```
With that sorted I created the form for the order, a little less simple as I nested the user data separately.
```
<%= simple_form_for [@showing, @order] do |f| %>
    <div class='user'>
        <%= f.simple_fields_for @user do |u| %>
            <%= u.input :name%>
            <%= u.input :email, as: :email %>
        <% end%>
    </div>
    <%= f.input :credit_card_number%>
    <%= f.input :credit_card_expiry, hint: 'Please enter in format MM/YY'%>
    <%= f.hidden_field :showing_id, value: @showing.id %>
    <%= f.button :submit, class:"btn btn-primary" %>
<% end%>
```

With some orders in the system I finished off the app by creating a quick index page for all orders for the admin page - displaying name and email (but not credit card information!).

I also deployed it to [Heroku](https://launchpadcinemas.herokuapp.com/) - check it out! 



