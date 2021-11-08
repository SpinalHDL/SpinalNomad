# Introduction

This makefile will allow you to create packages which can be used to run SBT on machines which have no internet connections.

# Create a package

```shell
make clean fetch_all collect compress
```

This will create a home.tgz. Be carefull, this will copy some folder from the machine, be sure you don't have any private key in it, see the makefile `collect` rule.

# Apply the package

You basicaly need to decompress the home.tgz in the home folder of the isolated machine.

```shell
make decompress apply
```

# Adding more depedencies

If you project use some additional depedencies, what you need to do is to add their usage in the makefile `fetch_xxx` rule.
