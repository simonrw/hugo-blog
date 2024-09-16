---
title: "Go (and Rust)"
date: 2020-06-24T08:36:37+01:00
draft: true
tags:
- go
- rust
summary: How I feel about Go, and how it compares to Rust.
---

This is not your usual language comparison post. It is**_highly_** opinionated,
and therefore much more subjective than other posts.

I also know that comparisons of these languages was very popular a while ago,
and have slightly fallen away, as I believe people have realised the differences
between these languages, and no longer consider them to be suitable for the same
roles (despite Go's early marketing).

I find also that they are suitable for different situations.

I have been _very_ productive with Go over the last few months. It is currently
my go-to language for writing little helper applications, or basic web servers.
It is replaced [Flask][flask] for me as a handy web server, particularly as a
lot of interactive functionality is transitioning to front-end technologies.

For example, my last project was to build a system to record when I run out of
consumables (in my case, contact lens solution) so that I can understand how
often I need to replenish my stocks. It integrated with a cloud database
(DynamoDB) for ease of deployment, and the Go web server was very minimal. In
this case, I wanted to host on my RPi so the ability to cross-compile was
extremely useful, as was the ability (with the help of [pkger][pkger]) to deploy
a single binary.

I also agree that Go is a great fit where it is being currently used -
networked and distributed (and replicated) services.

## Notes

- go has replaced Python
- I worry about modelling complex situations with Go (cf. Rust enums)

[flask]:
[pkger]:
