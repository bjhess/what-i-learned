An interesting suggestion: Default values are part of your domain logic and should be kept together with the rest of the domain logic of your application, in the model layer. A sample default value in the model [124]:

    class Entry < ActiveRecord::Base
      def category
        # Alternatively could use read_attribute(:category)
        self[:category] || 'n/a'
      end
    end
    
Defaulting serialized attributes [126]:

    def preferences
      read_attribute(:preferences) || write_attribute(:preferences, {})
    end
    
I hadn't registered that there is an opposite method to `ActiveRecord#new_record?`: `ActiveRecord#persisted?`. [127]

I did not new Active Record constructors could take a block [127]:

    c = Client.new do |client|
      client.name = "Nile River Co."
      client.code = "NRC"
    end
    
`ActiveRecord#attributes` returns a hash with each attribute and its value as returned by `read_attribute`. This means custom reader methods will _not_ be called. `ActiveRecord#attributes=` does invoke custom writers, however. [130]

You can access attributes before typecasting [131]:

    class Timesheet < ActiveRecord::Base
      before_validation :fix_rate
      def fix_rate
        self[:rate] = rate_before_type_cast.tr('$', '')
      end
    end
    
You want that special method missing magic like `find_all_by_state`, but you still want the new Active Relation method chaining? Dynamic scopes:

    >> AreaCode.find_all_by_state("NJ").order(:created_at)
    NoMethodError: undefined method 'order' for ...

    >> AreaCode.scoped_by_state("NJ").order(:created_at)
    => [#<AreaCode id: 5, ...]
    
I'd probably rather just do `AreaCode.where(:state => "NJ")`. [133]

`update` can update multiple records with a list of keys and a list of values. Useful when a form submission includes multiple rows of records [137]:

    # Updates multiple records
    people = { 1 => { "first_name" => "David" }, 2 => { "first_name" => "Jeremy" } }
    Person.update(people.keys, people.values)
    
Don't forget, `update_attribute` skips validations and callbacks. I only use this when writing tests. [139]

Touch is always good to remember. Call `touche` on an Active Record object to update the `updated_at` attribute to the current time. You can also use it against any other timestamp field:

    user.touch(:viewed_at)
    
You can touch relationships whenever an object is updated automatically with [140]:

    class User < ActiveRecord::Base
      belongs_to :client, :touch => true
    end
    
You can designate attributes as readonly (primarily for calculated attribute) [141]:

    class Customer < ActiveRecord::Base
      attr_readonly :social_security_number
    end
    
Optimistic locking is a startegy for apps where collisions should be infrequent. Add `lock_version` column to a given table to make use of it. An `ActiveRecord::StaleObjectError` will be raised if a conflict is detected. [143]

Pessimistic Locking can basically be described with this code:

    Timesheet.transaction do
      t = Timesheet.lock.first
      t.approved = true
      t.save!
    end

Keep pessimistic locking transactions small and quick to execute. [145]

For long parameterized conditions, try bind variables instead of `?` [147]:

    Product.where("name = :name AND sku = :sku AND created_at > :date",
                  :name => 'Space Toilet', :sku => 80800, :dated => '2009-01-01')
                  
A portable and performant way to get a random record is to generate a random offset in Ruby [149]:

    Timesheet.limit(1).offset(rand(Timesheet.count)).first
    
How did I not know about the `offset` method? It's used for paging results. Find the second page of 10 results [149]:

    Timesheet.limit(10).offset(10)

There are similar methods to `includes` for eager loading. `eager_load` and `preload`. [151]

You can mark returned objects as readonly by chaining in the `readonly` method. I like this! [152]

    c = Comment.readonly.first
    
Multiple databases in your project? Make a new abstract class to use as the super class for the second DB. Setup the DB in your databse.yml file. Mark the super class `abstract_class = true` so it does not default to using single-table inheritance [153]:

    class LegacyProjectBase < ActiveRecord::Base
      establish_connection "legacy_#{Rails.env}"
      self.abstract_class = true
    end
    
Using the database connection directly is sometimes helpful. I've used it to build pseudo migration tables for data I was bringing in from a third-party API (think keeping track of the latest ID I brought in from syncing with the third-party API). The simplest form is `ActiveRecord::Base.connection.execute("show tables").all_hashes`. Lots more discussed in the book. [154]