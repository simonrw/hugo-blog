---
title: Fighting the compiler
date: 2016-06-01 08:40:18
tags:
- rust
---

I'm learning [Rust][rust] at the moment, which I'm finding quite an interesting
challenge.  I agree with a lot of the Rust principles and find it extremely
comforting that the compiler has got my back, but it's bringing me back to my
early times learning C and "fighting with the compiler".

How many hours did I spend adding "&" and "\*" to variables to pass into
functions before I really understood what it meant for a function to take a
pointer? Simple rules like:

1.  if a function takes a pointer argument and expects a "simple" data type,
    e.g.  integer then pass the address of a variable with "&"
1.  if a function expects an array, then the array variable is equivalent to a
    pointer already so just pass the array variable
1.  if a function expects a _double_ pointer "\*\*" then it expects to change
    the value of a normal pointer, so treat the pointer itself like a single
    variable in point 1 above.

During one of his talks, Steve Klabnik stated that he no longer thinks of
"fighting the compiler", but instead considers it like a helpful friend passing
warnings about inadvisable behaviour.

I can't say I've reached that stage yet.  It very much feels to me like when I
was learning C again.  Quite nice in a way, reminding me of my roots but on the
other hand I find it very un-reassuring.  The key is that I don't understand
_why_ the compiler is complaining - I haven't taken the language rules to heart
yet.  I've read the excellent [rust book][rust-book] in its entirity, but this
does not mean I _know_ the rules.  I know that with time it will come, and the
more I use the language the easier it will be. 

I have two learning projects on the go at the moment.  The first is an attempt
to wrap a [C astronomy library][cfitsio] enabling access to the FITS data
format, and hopefully allowing me and others in the astronomy community to
start using Rust.  The second is a small remote API fetcher, with the idea to
run on my raspberry pi with a temperature sensor, and compare the predictions
made by weather models to the measured temperature.

My FITS library is [up on crates.io][rust-fitsio], and the API fetcher is on
[github][met-office-checker], comments are welcome!

I look forward to productive times with Rust.  As a Python user with a
background in C/C++, I like the way Rust straddles the two - the memory safety
of Python but without a garbage collector, and the close-to-the-metal speed and
efficiency of C/C++.  I've been looking for a compiled language with RAII that
isn't C++ for ages.

[rust]: https://www.rust-lang.org/
[rust-book]: https://doc.rust-lang.org/book/
[cfitsio]: https://heasarc.gsfc.nasa.gov/docs/software/fitsio/fitsio.html
[rust-fitsio]: https://crates.io/crates/fitsio
[met-office-checker]: https://github.com/mindriot101/met-office-tester
