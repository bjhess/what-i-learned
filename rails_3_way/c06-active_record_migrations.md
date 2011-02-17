I usually just explicitly write the migration methods for association ID's and polymorphic type columns, but there is an alternative [166]:

    create_table :accounts do
      t.belongs_to(:person)
    end
    
    create_table :comments do
      t.references(:commentable, :polymorphic => true)
    end
    
Opposite is `remove_references`. [167]

There are some inconsistencies here. First, it is suggested to avoid loading up models in migrations. Rather, execute raw SQL in these cases:

    execute("update phones set number = concat(area_code, prefix, suffix)")

The reasoning being that the object graph might change down the road. [173]

Then it says, well, you can use models but declare them inside your migration for when the object graph changes (they will be namespaced to the migration class) [174]:

    class HashPasswordsOnUsers < ActiveRecord::Migration
      class User < ActiveRecord::Base
      end
      
      def self.up
        User.find_each do |user|
          ...
        end
        remove_column :users, :password
      end
    end

All this is a crazy way of suggesting that a migration will probably not run in 6 months because the object graph had changed. Yet the book goes on to say:

> If you need to create the application database on another system, you should be using `db:schema:load`, not running all the migrations from scratch. The latter is a flawed and unsustainable approach (the more migrations you'll amass, the slower it'll run and _the greater likelihood for issues_).

Which is basically saying all that careful wrote SQL and namespaced model work was for nothing. [174]

I've never done it before, but database seeding can be setup with `db/seeds.rb`. [175]