# The Problem

Sometimes our models are representing more than one thing.  Again, this gives us the problem of having to look at code and methods that are unrelated to our task at hand.

For example, the code below is inspired by the bnb-methods lab that asked us to write methods for both guests and hosts.  These methods currently live in the User class.  Now imagine that we want to add a method to user called `money_spent`.  This method operates very differently, and answers different questions if the user is a host vs if the user is a guest.  

And looking below, you can see that there is already code that is only relevant if the user is a guest and other code that is relevant if the user is a host.  

```ruby
class User < ActiveRecord::Base
  has_many :listings, :foreign_key => 'host_id'
  has_many :reservations, :through => :listings
  has_many :trips, :foreign_key => 'guest_id', :class_name => "Reservation"
  has_many :reviews, :foreign_key => 'guest_id'

  ## As a guest
  has_many :trip_listings, :through => :trips, :source => :listing
  has_many :hosts, :through => :trip_listings, :foreign_key => :host_id

  ## As a host
  has_many :guests, :through => :reservations, :class_name => "User"
  has_many :host_reviews, :through => :listings, :source => :reviews
end
```

### The Solution
The code above leads to a developer going to a model for one reason, and finding code unrelated to the developer's purpose.  Furthermore, it leads to confusion about a methods purpose, because a method can be expected to work differently based on the particulars of the object.  

The solution is to the break out the above code into three different models: a user, a guest, and a host.  This is called composition.  Read more about composition with the sources below.


* [Thoughtbot - Composition](https://robots.thoughtbot.com/reusable-oo-composition)
* [Wikipedia on Composition](https://en.wikipedia.org/wiki/Composition_over_inheritance)

### Other Resources
* *Clean Ruby*, pp. 34 - 45
* *Design Patterns In Ruby*, pp. 7 - 12
* *Practical Object Oriented Design In Ruby* pp. 163 - 190
<p class='util--hide'>View <a href='https://learn.co/lessons/composition'>Composition</a> on Learn.co and start learning to code for free.</p>
