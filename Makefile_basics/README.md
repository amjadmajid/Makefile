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
If make found a dependency with a newer timestamp than the target, it will
remake that target and all the targets that depend on it.
Considering, for example, the associated Makefile. If the source file *module.c* is modified, `make` will remake
the *module.o* and the target *all*  when the program is rebuilt. However, make will not remake
 the *main.o* since it has a newer timestamp than the source file *main.c*.
Furthermore, special targets are not files and such their related actions are always executed
if they are specified.
