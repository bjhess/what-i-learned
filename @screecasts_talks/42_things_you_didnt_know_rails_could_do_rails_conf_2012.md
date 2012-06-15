James Edward Gray II  
RailsConf  
May, 2012  
(I did not attend)

<https://speakerdeck.com/u/jeg2/p/10-things-you-didnt-know-rails-could-do>

These things I learned or had forgotten. For instance, I kind of forgot all the built in `rake notes` stuff:

    # TODO: todo
    # FIXME: fixme
    # OPTIMIZE: optimize
    rake notes
    rake notes:todo
    rake notes:fixme
    rake notes:optimize
    # SPOON: spoon
    rake notes:custom ANNOTATION=SPOON

I don't generate migrations often enough to find value in this, but:

    rails g resource user name email token:string{6} bio:text

Begets:

    class CreateUsers < ActiveRecodr::Migration
      def change
        create_table :users do |t|
          t.string :name
          t.string :email
          t.string :token, :limit => 6
          t.text :bio

          t.timestamps
        end
      end
    end

And with indexes:

    rails g resource user name:index email:uniq token:string{6} bio:text

Begets:

    class CreateUsers < ActiveRecodr::Migration
      def change
        create_table :users do |t|
          t.string :name
          t.string :email
          t.string :token, :limit => 6
          t.text :bio

          t.timestamps
        end
        add_index :users, :name
        add_index :users, :email, :unique => true
      end
    end

You can find the status of migrations in your db with:

     rake db:migrate:status

Pluck:

    > User.pluck(:email)
       SELECT email FROM "users"
    > User.uniq.pluck(:email)
       SELECT DISTINCT email FROM "users"

Grouped counts:

    > Project.group(:client).count
    => {"1234"=>3, "4345"=>10}

[`Hash#deep_merge`](http://apidock.com/rails/v3.0.9/Hash/deep_merge) OMG!

Use `Hash#except` to remove keys from a hash.

Magic inquiries:

    > "magic".inquiry.magic?
    => true


Here's a nice pattern. Avoid assignments in views with blocks.

<img src="https://img.skitch.com/20120615-cfyb2qyxi37gspxprn2ht8brup.jpg" />

Becomes:

<img src="https://img.skitch.com/20120615-8gt82ui79tt9ejjb782x962gqw.jpg" />

With:

<img src="https://img.skitch.com/20120615-8gt82ui79tt9ejjb782x962gqw.jpg" />

Just never use [`content_tag_for`](http://apidock.com/rails/ActionView/Helpers/RecordTagHelper/content_tag_for). Cool how it can act as an iterator over an array of objects as well! (Though I don't think I see that last part in the documentation?)

Adding special tags with `FormBuilder` probably would come in handy at some point. I always know stuff like this is _possible_. Definitely something I'd need to look up when the day comes. (And generally I just say "screw it." :)

