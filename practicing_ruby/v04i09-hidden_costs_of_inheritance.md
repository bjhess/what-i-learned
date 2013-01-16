### [The Hidden Costs of Inheritance](https://practicingruby.com/articles/shared/xlqsnomjhtvo)

I really enjoyed problem 3, specifically it's description of the `inspect`/`to_s` inheritance chain. Balancing reuse and customization can be tricky. `inspect` and `to_s` result in different output by default, but once your class overrides `to_s`, `inspect` outputs the same thing! And so overriding one method often leads to a need to override another method.

One of the comments shared a collision between a mixed in method and an attribute/variable. That can be really maddening!