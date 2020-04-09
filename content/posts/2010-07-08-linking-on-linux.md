---
Title: Linking on linux
Date: 2010-07-08 15:24:00
tags:
- linux
- c
---



I've just found out what some strange linking options mean with gcc!

Quite often i've seen at the linking stage


``` bash
gcc object.o -o executable.out -lsomelibrary -Wl,-rpath,somepath
```


I've discovered that the -Wl,-rpath,somepath expands basically to


``` bash
-Wl -rpath=somepath
```


and this means the location of the shared library in *somepath* is hard
coded into the executable to be used at runtime. *rpath* means runtime
path, all of which means the dangerous *LD\_LIBRARY\_PATH* environment
variable is not needed.
