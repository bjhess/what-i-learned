 [JEG2's things](https://speakerdeck.com/jeg2/10-things-you-didnt-know-rails-could-do).

 Things I learned or had forgotten.

    # TODO: todo
    # FIXME: fixme
    # OPTIMIZE: optimize
    rake notes
    rake notes:todo
    rake notes:fixme
    rake notes:optimize
    # SPOON: spoon
    rake notes:custom ANNOTATION=SPOON

I don't generate migrations often enough to find value in this stuff, but:

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

Pluck on more recent versions of Rails:

    > User.pluck(:email)
       SELECT email FROM "users"
    > User.uniq.pluck(:email)
       SELECT DISTINCT email FROM "users"

More recent versions of Rails have:

    > Project.group(:client).count
    => {"1234"=>3, "4345"=>10}

[`deep_merge`](http://apidock.com/rails/v3.0.9/Hash/deep_merge) omg!

Use `Hash#except` to remove keys from a hash.

Magic inquiries:

    > "magic".inquiry.magic?
    => true


[Avoid assignments in views with blocks](https://gist.github.com/xdite/3109315#32-use-blocks-to-avoid-assignments-in-views).

I'm not familiar with `content_tag_for` (new). Cool how it can act as an iterator over an array of objects as well!

Adding special tags with `FormBuilder` probably would come in handy at some point. But definitely something I'd need to look up  when the day came. (And generally I just say "screw it.")

