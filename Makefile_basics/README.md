## Simple Makefile
 ### General syntax of a Makefile

The general syntax of a Makefile *rule* is as follows:
```
	target: prerequisite
	[TAB] recipe
	[TAB] recipe
    ...
```

### General remarkes
1. By convention,  variable's names are written in upper-case form, i.e. CC = gcc.
2. A varaible can be accessed using one of these ${VAR}, $(VAR) syntaxes.
3. If no target is specified, make is defaulted to target the first target in a Makefile.
4. Each make line is executed in a separate sub-shell environment. Therefore, a command like `cd newdir` will not affect the next lines.



### How does make utilize the timestamp of files
If make found a dependency ,which is one of the prerequistes for a target, with a newer timestamp than the target's timestamp, it will
rebuild that target and all the targets that depend on it.
Considering, for example, the associated Makefile. If the source file *module.c* is modified, `make` will remake
the *module.o* and the target *all*---since *all* depends on *module.o*---when the program is rebuilt. However, make will not remake
 the *main.o* since it has a newer timestamp than the source file *main.c*.

A *phony target* is a target that is not associated with a real file, i.e. clean.
When it is targeted by a `make` command, i.e. `make clean`, its actions, e.g. `rm -rf *.o`, will always be executed.
