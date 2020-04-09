---
layout: post
title: Null objects for the win
date: 2013-09-04 23:18:00
comments: true
Tags:
- python
- code design
Category: tech
---

So I've been watching a lot of OO refactoring screencasts and reading posts and I'm able to say I've implemented some of the advice I've heard. Life's all about learning eh?

So the main example I want to talk about here is *Null objects*. 

## Null objects

In dynamic languages, and Ruby in particular[^1] the concept of the *lack of something* needs to be encapsulated. For example: you're wrapping the database and no entry exists; what to you return? In Ruby it's often `nil` but this is not great for app design. This `nil` will proliferate through your app causing errors in its wake (`NoMethodError`) and causing de-localised stacktraces. Another example is the concept of a guest user. How do you represent this? Subclass the normal User class? *`nil`*?!
One way to combat this is to introduce Null Objects, where they look like a standard user but don't respond to the methods in the same way.

My implementation was in Python whilst working on a plotting script. I wanted the option to disable the lower subplot with a given command line argument, so I started adding `if` conditions everywhere, which unfortunately was spread all over the code. My recent learning suddenly kicked in and I thought I could create a null axis object (on which all of the plotting commands were being called) which wouldn't respond to any plotting calls.

In Ruby it's nice and simple to stub out methods, or use metaprogramming. For example by defining a class and a "null" `method_missing` method a null object can be created

``` ruby
class NullAxis
  def method_missing(name, *args, &block)
  end
end

NullAxis.new.any_method_that_doesnt_exist
# => nil
```

The implementation for Python is almost as simple

``` python
class NullAxis(object):
    def __getattr__(self, name):
        '''
        Like Ruby's `method_missing`, this just returns a 
        blank function that does nothing
        '''
        def fn(*args, **kwargs):
            pass

        return fn

```

With this I can leave the axis creation and previous plotting commands intact, but under the condition that the `matplotlib` axis object does not exist the code will just silently do nothing.

This example is small, but I felt good implementing it.



[^1]: due to the prevalence of `nil`s
