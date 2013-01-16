### [Implementing the Active Record pattern, Part 1](https://practicingruby.com/articles/shared/mtonkqpvfgnn)

I clearly don't enjoy thinking about abstractions at this level, but I occasionally through myself against the wall and hope something more sticks this time.

One interesting take away was a simple, not-so-performant technique for making deep copies in Ruby:

```ruby
Marshal.load(Marshal.dump(object))
```

Someone on Stack Overflow did a [performance comparison](http://stackoverflow.com/questions/5643432/whats-the-most-efficient-way-to-deep-copy-an-object-in-ruby) of a few techniques.