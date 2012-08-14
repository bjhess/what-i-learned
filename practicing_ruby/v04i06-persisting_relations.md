### [Persisting Relations](http://practicingruby.com/articles/shared/kzosgsxjjbar)

There was a lot of stuff in here, simultaneously too much and too little information. So my main takeaway was the existence of [`PStore`](http://ruby-doc.org/stdlib-1.9.3/libdoc/pstore/rdoc/PStore.html), Ruby's built-in persistence mechanism. The example uses `YAML::Store`:

```ruby
require 'yaml/store'
store = YAML::Store.new 'quotes.yml'

# quotes are author + text structures
Quote = Struct.new :author, :text

store.transaction do   # a read/write transaction...
  store['db'] ||= []
  store['db'] << Quote.new('Charlie Gibbs',
    'A database is a black hole into which you put your data.')
  store['db'] << Quote.new('Will Jessop',
    'MySQL is truly the PHP of the database world.')
end                    # ...is atomically committed here
```

`quotes.yml`:

```yaml
---
db:
- !ruby/struct:Quote
  author: Charlie Gibbs
  text: A database is a black hole into which you put your data.
- !ruby/struct:Quote
  author: Will Jessop
  text: MySQL is truly the PHP of the database world.
```

An interesting option for small, especially local, apps.