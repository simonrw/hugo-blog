---
title: Installing rust on older linux systems
date: 2016-06-15 10:52:51
category: tech
tags:
- rust
---

At work we use SLES 11 which has quite old versions of openssl and
installed certificates. I was getting certificate errors trying to
install rust with the [rustup][rustup] tool. 

I tried searching for any help at all but in the end I followed the
following advice:

* download a more recent certificate bundle (e.g. from
[certifi][certifi] or mozilla)
* set the environment variable `SSL_CERT_FILE` to point to this new file

This works for both `rustup` and `cargo` meaning I can develop with rust
on my work machine.

[rustup]: https://www.rustup.rs/
[certifi]: https://certifi.io/en/latest/
