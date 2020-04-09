---
layout: post
title: Ruby is scary
date: 2013-08-29 10:41:00
comments: true
Tags:
- ruby
Category: tech
---

Ok so after my [previous post]({{< ref "posts/2013-08-26-ruby-first-impressions.md" >}}), I was feeling pretty excited about Ruby. I talked about its power and flexibility and the exciting things that could come from it.

I also talked of Ruby's scariness, it's ability to terrify new programmers and potential for difficulties.

I've found one particular example which I'm sure shouldn't be allowed. I can see the intention and how in experienced hands it is amazing, but I can't unsee it's potential for misuse.

In Ruby you can add functionality by redeclaring the class you want to extend and add methods, or overwrite/extend methods. 

This is not the same as inheriting: you must create a new name in a standard inheritance chain. Not with Ruby; you can put the desired functionality straight into the class.

This I talked about before. What I didn't talk about before (probably because I never thought of it) is that any subclasses of your "new" class will inherit this new behaviour.

The logical conclusion of this is thus: everything in Ruby inherits from `Object`. What about adding methods to `Object`?

``` ruby
class Object
  def foo
    puts "Printing foo"
  end
end

2.foo
# => Printing foo
```

This example is harmless - mostly harmless ;) - but what about in a gem you make public:

``` ruby
class Object
  def foo
    # Launch the nukes
    # ... or system "rm -rf *"
  end
end

# user: "The documentation tells me that to do something awesome, call 3.foo"
3.foo

# BANG!
```

How can anybody sleep at night?!
