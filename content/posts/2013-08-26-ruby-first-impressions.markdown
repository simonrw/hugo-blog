---
layout: post
title: "Ruby: first impressions"
date: 2013-08-26 22:14:00
comments: true
Tags:
- ruby
Category: tech
---

So I've been trying [Ruby](https://www.ruby-lang.org/en/) recently,
partially trying to add some substance to my CV, and partially out of
curiosity. I used to be a complete python snob; I didn't understand
why anyone *wouldn't* use python. It is a dynamic language so no more
manual memory management (though there are plenty of advantages *cough*
*cough*); duck typing deserves its own special place in hell but it
is bloody convenient; the amazing standard library which is great for
system administration; and many other things.

I use python in my day job[^1] and was perfectly happy with it. As I
came from a C++ background I agreed with the "explicit is better than
implicit" philosophy, I understood the power of decorators and context
managers - I still had yet to master metaclasses but that's another
story.

I was amazed with some of the metaprogramming that was possible with
python, like the list comprehension, or the memory saving generators,
it makes working with scientific data a breeze with `numpy` snd
`matplotlib`, and the significant whitespace really works well: you are
all indenting your code properly aren't you?

I heard that Ruby has some amazing metaprogramming ability, but I didn't
realise until I tried that you can add functionality to an existing
class by just redefining it! Even core classes like numbers.

Rails does this exclusively creating such lines as

``` ruby
3.days.ago
```

by adding functionality to the Fixnum class, for example:

``` ruby
class Fixnum
  def foo
    "Hello world"
  end
end

puts 3.foo
# => "Hello world"
```

Metaprogramming
---------------

So the metaprogramming outlined above is great, and extremely powerful.
I was worried initially about this and how it may cause confusion.
Function overloading in C++ is already bad enough as you have to dive
into the documentation for the simplest methods. In ruby the possibility for
this is even worse! But it seems the common libraries are trustworthy in this
respect and do not abuse this functionality. **Beginners warning: do not abuse
this feature!**

Another amazing feature is `method_missing` where if a method is not recognised
(Ruby uses the message sending style where you could send any method to any
object, it just may not do anything - and will probably throw an exception) this
method - if defined - is called.

``` ruby
class TestClass
  def method_missing(*args, &block)
    puts "Message '#{args.shift}' called with arguments: #{args}"
  end
end

TestClass.new.bad_method(100, 'stuff')
# => Message 'bad_method' called with arguments: [100, "stuff"]
```

## Context

In Python context managers were added in 2.6 to perform code before and after
the block, such as automatically closing files (note: the `contextlib` library
is brilliant for this, creating one manually is a lot of boilerplate)

``` python
from contextlib import contextmanager

@contextmanager
def to_stuff():
    print 'Before'
    yield 10
    print 'After'

with to_stuff() as value:
    print value == 10

# Prints:
# Before 
# True
# After
```

Ruby takes this to the next level with blocks. The syntax looks similar but it's
baked into the language:

``` ruby
def to_stuff
  puts 'Before'
  yield 10
  puts 'After'
end

to_stuff do |value|
  puts value == 10
end

# Prints:
# Before 
# True
# After
```

You see these blocks *everywhere*! This context has great power, a new object
when generated can be passed into a block for more syntactically nice setup

``` ruby
TestObject.new.tap |o|
  o.value = 10
end
```
## Syntax

Against Python's "explicit is better than implicit" philosophy, Ruby seems to
have the opposite. I don't know if this is completely true but that's how it
seems. For example class methods can be treated as if they were local variables,
and parentheses are (mostly) optional, the last statement of any block is
returned, and other things. For example

``` ruby
class TestClass
  def foo
    10
  end

  def print_value(value)
    puts value
  end

  def print_foo
    value = 100
    puts foo        # Is foo a variable or a method?!

    print_value 150 # No parentheses!
  end

end
```

## Generating DSLs

Due to the free syntax of Ruby, especially the lack of parentheses, a custom DSL
(domain specific language, essentially a mini programming language for a very
specific task) is easy to create in Ruby. Consequently to a beginner it's
easy to read Ruby code written like this and believe it is a different language.

For example a sort-of Makefile alternative `rake` can look like

``` ruby
desc "This is a simple task. You can run it with `rake foo`"
task :foo do
  Dir["*"].each do |f|
    puts f
  end
end
```

whereas the testing framework `rspec` can look like

``` ruby
describe FooModel do
  before(:each) do
    puts 'Before'
  end

  it "should have some specified behaviour" do
    expect(FooModel).to not_be(nil)
  end
end
```

or even

``` ruby
print_stuff "foo"
change_directory "/tmp"
add_file "test"
```

These would all be valid ruby and generally apart from the odd keyword (do/end
etc.) they can have a completely different style. This makes each implementation
fresh and different and exciting, providing a lot of power to the language.

Rails
-----

Now we get on to the poster child of Ruby: Rails. Who can mention Ruby without
talking about rails eh?

Well it's a great thing to have for a language. Can you name a defining Python
library? Django? Numpy? Twisted? In some sense Python is a little *too* good at
everything which makes it not excel as much as Rails. A lot of the things
outlined above are prevalent in Rails, and make the framework a dream to work
with. Ok so a dream for me, perhaps I'm just naive and haven't worked day to day
in it, but I'd definitley like to try!

In all Ruby is worth learning, system administrators are using it for tasks, or
one can use it as a Makefile replacement and Rails' emphasis on convention makes
it a lot to learn initially but once you have a basic knowledge it's not too
difficult to pick up. I've outlined some of the resources I used to learn below
for more information. I hope to learn more of what Ruby has to offer in the
future, and possibly to pass on my knowledge to the next generation.

## Resources

### Beginner

[Rails for Zombies](https://railsforzombies.org/)

### Technical

[Railscasts](https://railscasts.com/)

### Advanced

[Thoughtbot](https://robots.thoughtbot.com/)

(I'll try and keep these updated)


[^1]: "job"
