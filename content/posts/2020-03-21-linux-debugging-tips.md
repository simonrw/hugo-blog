---
title: Linux debugging tips
Authors: Simon Walker
date: 2020-03-21 19:04:41
category: tech
tags:
- linux
summary: 📘  💻 ℹ️ Tips designed to help new Linux users work out what is going on
---

_**📘  💻 ℹ️ These tips are designed to help new Linux users work out what is going on.**_


## Finding out what process is listening on a port

This is most common for web servers. When a web server listens on a specific
port (e.g. 5000 for Flask's default port), it is often useful to know more
information about this process. In particular, only one process can listen on a
port at once. If your program doesn't start because another process is
listening, then this will be useful for you.

### Problem example

In the example below I have started a Flask web server on port 5000. I try to
start another server and get the following error:

```
OSError: [Errno 48] Address already in use
```

This is because another process is listening on that port. In order to find out
what that process is, I run `lsof`.

### Solution

`lsof` **l**i**s**ts **o**pen **f**iles. On Linux, _everything_ is a file, so
this includes ports. In order to check for ports, use the `-i` flag and define
the port as `:<port>`:

```sh
$ lsof -i :5000

COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
flask   142550 simon    3u  IPv4 978282      0t0  TCP localhost:commplex-main (LISTEN)
```

This shows that process `142550` (`PID` column) is responsible for listening on
this port.

## Getting information about a process

In the example above, we found out that process 142550 is listening on port 5000.
What if we want to find more information about this process? For example, where
the program was started from, what command is being run?

### The `/proc` filesystem

_Note: the program `ps` can do quite a lot of this_

Linux stores information about all of the running processes on the system under
`/proc`. At the top level is one directory per process, so we find the path
`/proc/142550` exists:

```sh
$ ls /proc | grep 142550

142550
```

Inside this filesystem is a host of useful information:

```sh
$ ls /proc/142550

total 0
-r--r--r--  1 simon simon 0 Mar 21 20:22 arch_status
dr-xr-xr-x  2 simon simon 0 Mar 21 20:22 attr
-rw-r--r--  1 simon simon 0 Mar 21 20:22 autogroup
-r--------  1 simon simon 0 Mar 21 20:22 auxv
-r--r--r--  1 simon simon 0 Mar 21 20:22 cgroup
--w-------  1 simon simon 0 Mar 21 20:22 clear_refs
-r--r--r--  1 simon simon 0 Mar 21 20:18 cmdline
-rw-r--r--  1 simon simon 0 Mar 21 20:22 comm
-rw-r--r--  1 simon simon 0 Mar 21 20:22 coredump_filter
-r--r--r--  1 simon simon 0 Mar 21 20:22 cpuset
lrwxrwxrwx  1 simon simon 0 Mar 21 20:22 cwd -> /tmp/flasktesting
-r--------  1 simon simon 0 Mar 21 20:22 environ
lrwxrwxrwx  1 simon simon 0 Mar 21 20:18 exe -> /usr/bin/python3.8
dr-x------  2 simon simon 0 Mar 21 20:18 fd
dr-x------  2 simon simon 0 Mar 21 20:18 fdinfo
-rw-r--r--  1 simon simon 0 Mar 21 20:22 gid_map
-r--------  1 simon simon 0 Mar 21 20:22 io
-r--r--r--  1 simon simon 0 Mar 21 20:22 latency
-r--r--r--  1 simon simon 0 Mar 21 20:22 limits
-rw-r--r--  1 simon simon 0 Mar 21 20:22 loginuid
dr-x------  2 simon simon 0 Mar 21 20:22 map_files
-r--r--r--  1 simon simon 0 Mar 21 20:22 maps
-rw-------  1 simon simon 0 Mar 21 20:22 mem
-r--r--r--  1 simon simon 0 Mar 21 20:22 mountinfo
-r--r--r--  1 simon simon 0 Mar 21 20:22 mounts
-r--------  1 simon simon 0 Mar 21 20:22 mountstats
dr-xr-xr-x 62 simon simon 0 Mar 21 20:22 net
dr-x--x--x  2 simon simon 0 Mar 21 20:22 ns
-r--r--r--  1 simon simon 0 Mar 21 20:22 numa_maps
-rw-r--r--  1 simon simon 0 Mar 21 20:22 oom_adj
-r--r--r--  1 simon simon 0 Mar 21 20:22 oom_score
-rw-r--r--  1 simon simon 0 Mar 21 20:22 oom_score_adj
-r--------  1 simon simon 0 Mar 21 20:22 pagemap
-r--------  1 simon simon 0 Mar 21 20:22 personality
-rw-r--r--  1 simon simon 0 Mar 21 20:22 projid_map
lrwxrwxrwx  1 simon simon 0 Mar 21 20:22 root -> /
-rw-r--r--  1 simon simon 0 Mar 21 20:22 sched
-r--r--r--  1 simon simon 0 Mar 21 20:22 schedstat
-r--r--r--  1 simon simon 0 Mar 21 20:22 sessionid
-rw-r--r--  1 simon simon 0 Mar 21 20:22 setgroups
-r--r--r--  1 simon simon 0 Mar 21 20:22 smaps
-r--r--r--  1 simon simon 0 Mar 21 20:22 smaps_rollup
-r--------  1 simon simon 0 Mar 21 20:22 stack
-r--r--r--  1 simon simon 0 Mar 21 20:18 stat
-r--r--r--  1 simon simon 0 Mar 21 20:22 statm
-r--r--r--  1 simon simon 0 Mar 21 20:19 status
-r--------  1 simon simon 0 Mar 21 20:22 syscall
dr-xr-xr-x  3 simon simon 0 Mar 21 20:22 task
-r--r--r--  1 simon simon 0 Mar 21 20:22 timers
-rw-rw-rw-  1 simon simon 0 Mar 21 20:22 timerslack_ns
-rw-r--r--  1 simon simon 0 Mar 21 20:22 uid_map
-r--r--r--  1 simon simon 0 Mar 21 20:22 wchan
```

Some interesting entries:

* `cwd`: a symlink to the directory the process was started from
* `cmdline`: a file listing the full command running (strings separated by the
  `null` byte `\0` rather than newlines)
* `environ`: a file listing the process environment variables when the program
  was run (strings separated by the `null` byte `\0` rather than newlines)
* `fd`: a directory containing one entry for each file the process has open
* `exe`: a symlink to the process running (note for Python apps this will link to
  the python interpreter)
* `mountinfo`: list the mount points that the process can see (interesting for
  docker containers)

A process can investigate its _own_ information by looking at the symlink
`/proc/self` which points to its own `/proc` directory.

An alternative way of getting the process arguments is to run

```sh
$ ps -p <pid> -o args

COMMAND
/usr/bin/python /usr/bin/flask run
```

[More information about the `/proc`
filesystem](http://man7.org/linux/man-pages/man5/proc.5.html)

## Find out what system calls a process is running currently

Linux operates at two levels: _userspace_ and _kernel_. Normal programs are not
allowed to talk to files or memory directly, they must ask the kernel through
_system calls_. `strace` is a program that lets a normal user spy on what a
_userspace_ program is asking the kernel to do. This may be useful for example
if a program is stuck. You can try to see if it is stuck talking to the network,
or talking to the file system, or stuck in a wait loop.

For more information, check out [Julia Evans' blog
posts](https://jvns.ca/categories/strace/) on `strace` and its power.
