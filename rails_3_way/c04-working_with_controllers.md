I know Rack is simple, but I always like to write down the Hello World app. :)

    class HelloWorld
      def call(env)
        [200, {"Content-Type" => "text/plain"}, ["Hello World"]]
      end
    end
    
So there is `render :json => @record`, but you can also register additional rendering options. [More details via Yehuda Katz](http://www.engineyard.com/blog/2010/render-options-in-rails-3/).

I wasn't aware of `307 Temporary Redirect`. This would be used if you need to continue processing a POST request in a different action. Other redirects will use a GET request regardless of the original verb. A custom redirect would include assigning a path to `response.header["Location"]` and then rendering with `render :status => 307`.

You can redirect with an Active Record object:

    redirect_to post
    
I know about `redirect_to :back`, but I did not know that if no referrer is set a `RedirectBackError` is raised. I bet my old `redirect_back_or_default` methods don't make use of these techniques, rescuing `RedirectBackError`.

You can now assign a flash message inline with redirection:

    redirect_to @post, :alert => "Watch it, mister!"
    redirect_to @post, :notice => "Pay attention!"
    redirect_to @post, :flash => { :updated_post_id => @post.id }
    
Controller filter methods should be protected or private (natch). If they are not for some reason, you can use `hide_action` to assure the method is not available for dispatch.

I've never used a filter class before. Seems a good alternative for reuse of generic filters as opposed to creating the methods in some super class.

    class OutputCompressionFilter
      def self.after(controller)
        controller.response.body = compress(controller.response.body)
      end
    end

Call with

    after_filter OutputCompressionFilter
    
The class method can be `#after`, `#before`, or `#around` depending on the filter you're targeting.

I've been aware of, but never really used `prepend_before_filter` and `prepend_after_filter`.

Don't forget about `around_filter`!

I'm very unfamiliar with `send_data` and `send_file`. Good to have a discussion of it starting on pp 113. I know we've used the `:disposition` option to display a Ruby code file inline onscreen in our API documentation.