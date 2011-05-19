I did not realize adding records to an association with `<<` is transactional, but with `create` it is not. Another difference is that `<<` triggers `:before_add` and `:after_add` callbacks, but `create` does not. [186]

How did I not know about the `many?` method available on association collections? Detect if there is more than one item in the association collection. I've already used it once since reading about it. [187]

I'm not sure if I knew `new` was available on association collections. So `user.timesheets.new(attr)` is the same as `user.timesheets.build(attr)`. [187]

An association can have a `counter_sql` option, which will override the query when calling `count` on the association. Werd. [187]

Remember with `:counter_sql` and `:finder_sql` the string should be in single-quotes to prevent premature interpolation by Rails. [203] [212]

You can pass a block to `create` or `create!`, which will get yielded the newly created instance after the passed-in attributes are assigned, but before saving the record to the database. That's cool! [188]

You can use `:touch => true` on a `belongs_to` association. That much is obvious. But you can also provide a column name so the touch activity happens on a column other than `updated_at`. Like `:touch => :timesheets_updated_at`. [199]

On many-to-many relationships especially, `:uniq => true` could be useful. Consider a `has_many :through` situation where multiple identical objects on the other side of things get returned. [222]