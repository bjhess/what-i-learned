[Practical Object-Oriented Design in Ruby](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330)  
Sandi Metz

### Quick Review

I was skeptical that this book would be anything but a refresher. Thirteen years writing software, most of it object oriented, suggested my experience would make it all review. Especially since the necessary "trivial" examples of a book cannot compare to real world.

Surprisingly POODR stands in both worlds, presenting relatively simple problems while remaining imminently practical. It has challenged my beliefs about how unentangled a production application is capable of being. I will be pushing myself to stay true to the concepts in upcoming implementations.

### Notes

What follows are my notes and favorites, organized by chapter.

##### Desiging Classes with a Single Responsibility

> You are constructing a box that may be difficult to think outside of. You will never know less than you know right now [when you're starting a designing]. Many of the decisions you make today will need to be changed later. When that day comes, your ability to successfully make those changes will be determined by your application's design. [16]

A definition for _easy to change_:

* Changes have no unexpected side effects
* Small changes in requirements require correspondingly small changes in code
* Existing code is easy to reuse
* The easiest way to make a change is to add code that in itself is easy to change [16]

Code should have the following qualities (_TRUE_):

* **Transparent** The consequences of change should be obvious in the code that is changing and in distant code that relies upon it
* **Reasonable** The cost of any change should be proportional to the benefits the change achieves
* **Usable** Existing code should be usable in new and unexpected contexts
* **Exemplary** The code itself should encourage those who change it to perpetuate these qualities. [17]

> A class should do the smallest possible useful thing; that is it should have a single responsibility. [17]

> How can you determine if [a class] contains behavior that belongs somewhere else? One way is to pretend that it's sentient and to interrogate it. [22]

> When the future cost of doing nothing is the same as the current cost, postpone the decsion. Make the decision only when you must with the information you have at that time. [23]

> For better or for worse, the patterns you establish today will be replicated forever. When the code lies you must be alert to programmers believing and then propagating that lie.[23]

Well-known techniques that you can use to create code that embraces change:

* Depend on Behavior, Not Data
  - Hide Instance Variables
  - Hide Data Structures
* Enforce Single Responsibility Everywhere
  - Extract Extra Responsibilities from Methods
    (p31 has a good list on the benefit of single-responsibilty, small methods)
  - Isolate Extra Responsibilities in Classes [24-33]

There is a nice example of using `Struct` to do some short-term SRP when you don't want to make a decision yet.

```ruby
Wheel = Struct.new(:rim, :tire) do
  def diameter
    rim + (tire * 2)
  end
end
```

##### Managing Dependencies

A good before/after of dependency injection. [40-41]

When you cannot completely rewrite problem code, you might choose to isolate dependencies as much as possible. This includes isolating instance creation and vulnerable external messages. [42-45]

Argument-order dependencies can be problematic. I would suggest methods with > 2 arguments, or unstable arguments in general, should go to hash argument lists. [46-48]

Defaulting paramters in Ruby:

* `@cog = args[:cog] || 18` is often fine
* `@cog = args.fetch(:cog, 18)` works well, especially for boolean parameters
* A `defaults` method that presents a hash to `merge` with the parameters hash is a pretty clear technique as well [48-49]

Choosing dependency direction is tricky. Great quote:

> Pretend for a moment that your classes are people. If you were to give them advice about how to behave you would tell them to **depend on things that change less often than you do**. [53]

That statement is based on three truths about code:

* Some classes are more likely than others to have changes in requirements
* Concrete classes are more likely to change than abstract classes
* Changing a class that has many dependents will result in widespread consequences [53]

> When you use dependency injection, depending on duck typing, you have an abstraction. Depending on an abstraction is always safer than depending on concretions. [54]

> A dependency-laden class is not only hard to change due to consequences of that change, but it's under enormous pressure **never** to change because of the pain involved. [55]

##### Creating Flexible Interfaces

About the shift in thinking about classes and what they knew to thinking about a message and where to send it:

> [The] transition from class-based design to message-based design is a turning point in your design career. The message-based perspective yields more flexible applications.

Turns out sequence diagrams could be quite useful. [67]

> Minimize context. Construct public interfaces with an eye toward minimizing the context they require from others. Keep the **what** versus **how** distinction in mind.; create public methods that allow senders to get what they want without knowing how your class implements its behavior. [79]

##### Reducing Costs with Duck Typing

> The ability to tolerate ambiguity about the class of an object is the hallmark of a confident designer. Once you being to treat your objects as if they are defined by their behavior rather than by their class, you enter into a new realm of expressive flexible design. [94-95]

Recognizing hidden ducks:

* Case statements that switch on class
* `kind_of?` and `is_a?`
* `responds_to?` [96]

##### Acquiring Behavior Through Inheritance

Variables with names like `style`, `type`, or `category` are a cue that you may have an underlying pattern/class. [111]

> The general rule for refactoring into a new inheritance hierarchy is to arrange code so that you can promote abstractions rather than demote concretions. [123]

`NotImplementedError` is something to force subclasses to write methods found in the abstract superclass. [128]

The `post_initialize` and `local_spares` idea for subclass decoupling is interesting. [134]

##### Sharing Role Behavior with Modules

There is a neraly complete explanation of method lookup (through classes and modules). [157]

Discussion of class heirarchies: Shallow & Narrow, Shallow & Wide, Deep & Narrow, Deep & Wide. Prefer Shallow & Narrow, with Shallow & Wide being more complicated. [161]

##### Combining Objects with Composition

`Forwardable` and `def_delegators` for delegating without ActiveSupport. [175]

Deciding between inheritance and composition. [184]

##### Designing Cost-Effective Tests

> Remember that you will forget; write tests that remind you of the story once you have [forgotten]. [193]

> Tests are the canary in the coal mine; when the design is bad, testing is hard. [194]

(But tests must also be well-designed. :)

Testing incoming messages is a great section. [200]

> Because tests are the first reuse of code, [problems are] but a harbinger of things to come for your application as a whole. [204]

> An object with many private methods exudes the design smell of having too many responsibilities. [Consider extracting private methods into a new object, being aware and careful of stability of new interface.] [214]

> For the right problems having enough confidence to write embarrassing code can save money. [Defer design decisions until more info.] [214]

Testing outgoing messages is a great section. [215]

Using role tests to validate doubles is a great section. [224]

Testing inherited code is a great section. [229]

