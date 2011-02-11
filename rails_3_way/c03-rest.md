I was unaware of the [shallow option](http://ryandaigle.com/articles/2008/9/7/what-s-new-in-edge-rails-shallow-routes) when nesting routes. It seem to be too useful on the surface, but I think that's tempered by the types of apps I've developed with Rails. Reading the [enhancement's author's comment](http://ryandaigle.com/articles/2008/9/7/what-s-new-in-edge-rails-shallow-routes#comment-7773) makes me think it could have some utility in a public-facing user-generated-content site. [68]

I was aware of the new member (and collection) syntax ala:

    resources :auctions do
      member do
        post :retract
      end
    end

But I didn't know this identical alternative (I don't think I'll use it as it isn't very readable to me) [71]:

    resources :auctions do
      post :retract, :on => :member
    end
    
Kind of a cool, concise `respond_with` syntax for very straightforward controllers:

    class AuctionsController < ApplicationController
      respond_to :html, :xml, :json
      def index
        @auctions = Auction.all
        respond_with(@auctions)
      end
    end
    
Basically the action will automatically return the appropriate content. Reminder, a JSON request will go through the following attempts:

* Render associated view with .json extension
* If no view, call `to_json` on the object passed to `respond_with`
* If that doesn't work, call `to_format` on the object

Again, this will only work on the most straightforward of applications. [77]

I think I've been using a wordy syntax for formatted named routes [77]:

    link_to "XML version of this auction", auction_path(@auction, :format => :xml)  # Is the same as...
    link_to "XML version of this auction", auction_path(@auction, :xml)

As recommended by this chapter, I did take a look at [Roy Thomas Fielding's dissertation](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm), which includes a deep discussion of REST in chapters 5 and 6. I read through chapter 5 and skimmed chapter 6. [55]