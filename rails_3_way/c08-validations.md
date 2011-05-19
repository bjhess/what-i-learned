Lots of good info in this chapter about any ol' way you might want to write up custom validations.

The list of available variables for interpolation in a validation `:message` descriptor is: model, attribute, value and count. Use like so [243]:

    validates_uniqueness_of :login, :message => "{{value}} is already registered"
    
If you have a conditional validation that runs over a few individual validations, you may use `with_options` [244]:

    with_options :if => :password_required? do |user|
      user.validates_presence_of :password
      user.validates_presence_of :password_confirmation
      user.validates_length_of :password, :within => 4..40
      user.validates_confirmation_of :password
    end
    
Validation contexts provide a way to call `valid?(context_name)` on a record and execute only validations that apply to that context. So:

    class Report < ActiveRecord::Base
      validates_presence_of :name, :on => :publish
    end
    
Then in a controller [245]:

    if report.valid?(:publish)
      # do something funkay
    end
    
Got a ton of validations on one field? Try short-form validation [245]:

    validates :login, 
      :presence => true,
      :format => { :with => /[A-Za-z0-9]+/ },
      :length => { :minimum => 3 },
      :uniqueness => true