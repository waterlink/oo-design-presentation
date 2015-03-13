# OO Design

by Oleksii Fedorov



## Object Oriented Programming


Most notable features:

- Encapsulation (data kept with behaviour and hidden from clients)
- Inheritance (subclassing, inheriting all behaviour/data, even private/protected behaviour/data)
- Polymorphism (ability to substitute instances of one class with instances of others)


### How important these features are for OO design?

Imagine, that given 100 points you want to distribute them between these 3 features, and each number will represent an importance of corresponding feature.


|Encapsulation|Polymorphism|Inheritance|
|:--:|:--:|:--:|
|80|40|-20|

*Warning: this is my personal opinion*


### 80 + 40 - 20 = 100 :)


### Inheritance increases coupling

Subclass usually depends on some data or/and behaviour of its superclass. Even worse: usually this is not public behaviour/data.


### But how does one go without inheritance?


### Composition and Delegation



## Coupling


### Why coupling is bad?


Imagine you need to change behaviour of class X.

You will have to change any other piece of code base, that directly depends on this behaviour of class X.

More coupling you have, more changes will have to take place, and probably, in totally unrelated parts of codebase.


- Exponentially increases time required for change
- Invites bugs


### Dependency


### How does one deal with it?


### Dependency Inversion and Dependency Injection


```ruby
class Answer
  def rating
    RatingCalculator.new.rating_for(comments, upvotes, downvotes)
  end
end
```


### And what the problem with this code?


### Dependency!


```ruby
class Answer
  def initialize(rating_calculator)
    @rating_calculator = rating_calculator
  end

  def rating
    @rating_calculator.rating_for(comments, upvotes, downvotes)
  end
end
```


### Now you can have multiple rating calculator services and A/B test them easily


### Couple to abstract interfaces instead of referencing foreign class/module/package of your system



## Test Driven Development


### How is it related to OO Design?


### Provides short feedback loop on your design


- If you have troubles writing test - your design is wrong
- If you don't like how your test look like - your design is wrong
- If you have troubles making test green - your design is wrong


### Unit Tests and Integration Tests


### Integration Tests are scam!

- Very slow => bad feedback loop
- Exponential count of paths to test


### Unit Tests just don't work. Are they?


Testing in isolation = providing fake objects and/or mocks for all your **dependencies**

Mocks and fake objects can make your unit test green, but in fact the code is broken, because one of the dependencies has changed its behaviour or even public interface


### Cut your system at value boundaries!

Instead of method call boundaries.



## Actor Model


```ruby
class RatingCalculator
  def rating_for(comments, upvotes, downvotes)
    # .. calculate rating somehow ..
  end
end
```


```ruby
actor RatingCalculator
  comments, upvotes, downvotes, outbox = inbox.fetch
  # .. calculate rating somehow ..
  outbox.send(rating)
end
```


### Almost always has exactly one single responsibility

Since it receives message on inbox and doesn't really have much space to have multiple responsibilities (and methods)


### Easy to Unit Test without fake objects and mocks


```ruby
# creating actor, but not starting it
rating_actor = RatingCalculator.new

# dependency injection of rating actor
answer_actor = Answer.new(answer_id, rating_actor)
answer_actor.start

render_actor = Render.new(answer_actor)
render_actor.run
expect(rating_actor.inbox)
  .to include([comments, upvotes, downvotes, outbox])

# fake response from rating actor
outbox.send(3.5)
expect(render_actor).to render_rating(3.5)
```



## When the code is done?


- It works!


- It works!
- It is readable (future me will not curse me for writing this code)


- It works!
- It is readable (future me will not curse me for writing this code)
- It has no duplication
- And it is as short as possible (while maintaining all of the above)


### Code comments


### Basically a code smell


If code needs comment:

- It is not readable
- It fails to communicate intent



## To sum it up


- Inheritance is good only in very rare cases
- Coupling and dependencies are not your friends, take them under control with dependency inversion & injection
- TDD as a shortest feedback cycle for your OO design
- Write code in such way, that you would thank yourself for that in the future


### That is not all there about OO Design

I recommend reading the book "Pragmatic Programmer: From Journeyman to Master", it is very good one and has a lot of references to other good resources



## Thanks

Q & A
