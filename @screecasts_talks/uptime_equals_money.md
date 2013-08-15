**[Uptime == Money](http://vimeo.com/61255649)** ([slides](http://www.pgrs.net/wp-content/uploads/2013/02/rubyconf_australia_high_availability.pdf)) – Paul Gross

Posgres does migrations near instantly. Can create indexes with little impact. Transactional migrations. Sounds like it solves a lot of pain MySQL has adjusting schemas on large tables.

Redis in front of Rails holy crap! Their balancing and pausing capabilities are pretty sweet. Web requests go into Redis, then they get sent to Rails, then Rails puts the response into Redis, which goes back to the requestor. This allows them to pause things in Redis for 20-30 seconds for quick maintenance without dropping requests.

They also load balance most every integration point, involving writing some of their own software. Pretty neat.

Rolling deploys with dark features until all servers up-to-date (flags). Let migrations progress (adds or deletes) then flip the flag after it's all through.

One command automation for almost everything they do. Reduce human errors to a minimum. Gives confidence that task will work. Use them in staging and production.

http://www.outages.org/ & http://internetpulse.com/
