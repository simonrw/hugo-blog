---
title: Nix codesigning on macOS
date: 2024-10-29
tags:
- nix
- macos
summary: How to fix a surprisingly common issue with nix on macOS
---

_This is mostly a note to myself._

Sometimes, I have experienced output like the following:

```sh
# st = status
$ git st
Killed with signal 9
```

when running `nix` binaries. Most recently, it involved the `git` binary, but weirdly only when I used an alias 🤷

The `Console.app` output showed a line like:

```
CODE SIGNING: cs_invalid_page(0x1045dc000): p=16538[git] final status 0x23020200, denying page sending SIGKILL
```

This says to me that somehow macOS has decided my `git` binary is invalid, and the process execution is terminated with `SIGKILL`, the most fatal of all signals.

After doing some digging, I found [this GitHub issue](https://github.com/NixOS/nixpkgs/issues/208951) which mentioned `nix store repair`.

I ran the command 

```fish
sudo nix store repair (command -v git)
```

which seemed to fix my problem nicely.


