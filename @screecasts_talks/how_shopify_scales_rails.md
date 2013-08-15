**[How Shopify Scales Rails](http://www.confreaks.com/videos/2303-bigruby2013-how-shopify-scales-rails)**  
John Duff

51ms average response time?!

Know the most important part of the system to optimize:

* data entry (timesheet)
* reports probably don't have to be super-fast, though those are the people paying you
* getting customers paid sorts of things

Calculating RPM:

RPM = Workers * 60/Response time  
RPM = 1172 * 60 / 0.072  
RPM = 976,666 (potential RPM)

Increasing workers or decrease response time is how you improve. (Or reduce number of requests needed?)

Know the system:

* avoid network calls during requests
* speed up unavoidable network calls (drop to C, caching, etc)
* know most important parts
* be prepared for known spikes – know when your spikes happen

New Relic, Splunk, StatsD (extend with what New Relic doesn't provide), Cacti (MySQL stats – [Percona monitoring plugins](http://www.percona.com/software/percona-monitoring-plugins)?), Conan (test prod under load - internal)

I'm missing New Relic watching this. App-wide stats would be nice.

Splunk + alerts looks kind of nice. 500 response spikes, for instance.

They have performance dashboards all over. RPM, Response time.

[cacheable](https://github.com/Shopify/cacheable):

* gzip'd content
* gzip stored in memcache
* generational caching
* no explicit expiry.

Cacheable example:

```ruby
class PostsController < ApplicationController

  def show
    response_cache do
      @post = @shop.posts.find(params[:id])
      respond_with(@post)
    end
  end

  def cache_key_data
    {
      :action       => action_name
      :format       => request.format,
      :params       => params.slice(:id),
      :shop_version => @shop.version
    }
  end
end

```

Caching 404s had an impact! Saved like 1000-2000 RPM hitting Rails.

[Identity Cache](https://github.com/Shopify/identity_cache) - cache models in memcache (If I'm reading his graph right, ends up being like a 2ms win. Hrm.):

* cache full model objects in memcached
* can include associated objects in cache
* must opt in
* explicit, but automatic expiry

They do tons of background jobs. Use Resque for:

* Sending email (nice!)
* Process payments
* Geolocation
* Import / Export
* Search index
* +86 other things…

Backgrounding payment processing cut response times in half. Normalized traffic patterns.

Redis:

* Inventory reservation system (use lua to make fast inventory reservations)
* Sessions
* Theme uploads
* Throttling
* Sequenced Column (increment columns scoped to a particular store … ?)

Full working set in MySQL memory.

MySQL query optimization:

* pt-query-digest
* Avoid queries that generate temp tables
* Adding the right indexes
* Forcing / Ignoring indexes

To the latter, their orders table sounds like day entries. "Sometimes MySQL can make the wrong decisions about which index to use."

They tuned:

* disable innodb_stats_on_metadata (cuts out some unnecessary work)
* increase table_open_cache (keep this in sync with connections and file descriptors)
* replace glibc memory allocator with tcmalloc (for their write heavy load)
* innodb_autoinc_lock_mode='interleaved' (optimizes inserts into tables with auto increment. may degrade performance with bulk inserts.)

DB has limited number of undo slots. So using `after_commit` (vs `after_save` or such I suppose) is big because it gives up transaction and opens a slot. Also `after_commit` assures data is in DB. They use for webhooks, cache expiry, update associated objects. They'll use `after_save` to [flag](http://monosnap.com/image/MRV0YYbdtoEdDLTvVFbqMXj5A.png) the need for `after_commit` on changes (since `changes` aren't available in `after_commit`). Then they'll use `after_commit` to do more work.

Don't extract services until you absolutely need to. They split out Imagery service. Dynamic resizing of images. When image requests overshadowed application requests, it felt like time to move.
