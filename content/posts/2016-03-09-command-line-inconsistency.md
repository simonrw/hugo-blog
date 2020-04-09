---
title: Command line inconsistency
date: 2016-03-09 15:20:14
tags:
- linux
---

## RTFM!

Today I brought down our head node at work, because of a
misunderstanding of command line arguments for a linux program.

_In fairness, I should have read the man page more carefully for the
entry in question!_

I was using xargs for some nice command line parallelism and process
running. The command I ran was:

```bash
ls | grep action119 | grep exposureCycle | xargs -n 1 -I {} find {} -name 'IMAGE*.fits' | xargs -n 1 -P 0 bzip2 -k
```

In summary this finds the directories containing "action119" and
"exposureCycle" in the current directory, finds all of the files called
`IMAGE*.fits` in each directory and then in parallel compresses them
while keeping the original file (`-k`).

`xargs` is used twice in this invocation, and is one of my new favourite
tools. It takes a multi-line input on stdin, and passes each line to
whatever command that follows, e.g.:

```bash
ls | xargs echo
```

is the same as

```bash
ls | while read filename; do echo $filename; done
# or
# for file in $(ls); do echo $filename; done
```

It just dumps the input on the end of the command that follows `xargs`.
The argument `-n` says run the following command for each line on the
input, otherwise it dumps them all[1] on the end of the command. `-I {}`
replaces all occurrences of `{}` in the command with the input argument.

The first xargs invocation runs `find` for each directory, which then
prints a long list of files. The second xargs invocation bzips the files
in parallel, whilst keeping the original.

The key error was the `-P` argument. From the man page:

```
--max-procs=max-procs
-P max-procs
    Run up to max-procs processes at a time; the default is 1.  If
    max-procs is 0, xargs will run as many processes as possible at a
    time.  Use the -n option with -P; otherwise chances are that only
    one exec will be done.
```

In other linux programs I've used (e.g. GNU parallel) specifying `0` for
the number of processes means run the same as the number of processors
on the machine. For xargs, importantly it says "run as many processes as
possible", pretty much meaning one per input file.

I expected 16 processes to be spawned, one for each (virtual) core. It
spawned ~5k processes one per input file.

It brought the system down for about an hour, but it did seem to recover
ok.

In conclusion: **RTFM!**


*[1] actually because of linux command length restrictions, it splits the
lines up to follow this limitation, and runs the command multiple times
in groups if need be*

