I knew the `get`/`post` syntax prior, but I didn't really make the "shortcut" connection. The second line below is a shorthand of the first line:

    match 'products/:id' => 'products#show', :via => :get
    get 'products/:id' => 'products#show'
  
Defining constraints on the `:id` param has a shorthand:

    match 'products/:id' => :show, :constraints => {:id => /\d+/}
    match 'products/:id' => :show, :id => /\d+/
  
I knew about route globbing, but useful to note:

    # Route
    match 'items/list/*specs' => 'items#list'

    # URL
    /items/list/base/books/fiction/dickens/little_dorrit

    # Controller
    def list
      specs = params[:specs]  # => "base/books/fiction/dickens"
    end
  
How did I never know how to test a named route in the console before? Hint: `app` object:

    >> app.clients_path
    => "/clients"
  
    >> app.clients_url
    => "http://www.example.com/clients"
  
I sometimes forget what sugar works and what doesn't. `link_to` argument sugar shorthand:

    link_to "Auction of #{item.name}", item_path(:id => item.id)  # Is the same as...
    link_to "Auction of #{item.name}", item_path(item.id)         # Is the same as...
    link_to "Auction of #{item.name}", item_path(item)

This can work without the model ID as well, if you define `to_param`:

    def to_param
      description.parameterize
    end
  
Controller scoping sugar:

    scope :controller => :auctions do  # Is the same as...
    scope :auctions do
  
Constraints can be bundled within a scope:

    scope :controller => :auctions do
      match 'new' => :new
      constraints :id => /\d+/ do
        match 'edit/:id'  => :edit
        match 'pause/:id' => :edit
      end
    end
  
Constraints can be objectified given a method `matches?`. I did not know this. I'd usually through the regular expression into a variable for reuse:

    class DateFormatConstraint
      def self.matches?(request)
        request.params[:date] =~ /\A\d{4}-\d\d-\d\d\z/  # YYYY-MM-DD
      end
    end

    constraints(DateFormatConstraint) do
      match 'since/:date' => :since
    end