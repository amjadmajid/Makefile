# How to Write a Makefile

## Manual Compilation
To manually compile a project and produce an executable, follow these instructions:
```
$ gcc -I . -c main.c  ->  main.o
$ gcc -I . -c module.c ->  module.o
$ gcc main.o module.o -o target_bin
```
The `-I` option is used to include the current directory (.) as a header file location.

## General Syntax of a Makefile
The general syntax of a Makefile rule is:

```
target: dependency1 dependency2 ...
	[TAB] action1
	[TAB] action2
    ...
```

## General Remarks
1. By convention, variable names are written in upper-case, e.g. CC = gcc.
2. A variable can be accessed using either of these syntaxes: ${VAR}, $(VAR).
3. If no target is specified, `make` defaults to the first target in the Makefile.
4. Each line in a Makefile is executed in a separate sub-shell environment. Therefore, a command like `cd newdir` won't affect subsequent lines.

## Simple Makefile for Our Project
```
all: main.o module.o 
	gcc main.o module.o -o target_bin	
main.o: main.c module.h 	
	gcc -I . -c main.c
module.o: module.c module.h
	gcc -I . -c module.c
clean:	
	rm -rf *.o 
	rm target_bin
```

## Timestamps in Make
If `make` finds a dependency with a newer timestamp than the target, it will remake that target and all the targets that depend on it. Special targets aren't files, and their associated actions are always executed if they're specified.

## Variable Assignments
1. Simple assignment (`:=`): The expression is evaluated only once, at the first occurrence.
2. Recursive assignment (`=`): The expression is evaluated every time the variable is encountered in the code.
3. Conditional assignment (`?=`): A value is assigned to a variable only if it doesn't already have a value.
4. Appending (`+=`): Adds to the current value of a variable.

## Using Patterns and Special Variables
When wildcard `%` appears in the dependency list, it's replaced with the same string used to perform substitution in the target. Inside actions, you can use special variables like `$@`, `$?`, `$*`, `$<`, and `$^`.

## Action Modifiers
Prefixing an action with `-` tells `make` to ignore any errors in that line. Prefixing an action with `@` suppresses the standard print-action-to-standard-output behavior of `make`.

## Using PHONY to Avoid Name Conflicts
If a file in the project directory has the same name as a special target in the Makefile (like `all` or `clean`), it will cause a conflict. Use the `.PHONY` directive to specify which targets shouldn't be considered as files.

## Dry Run
You can use `make -n` to do a "dry run" to check the execution of `make` without actually running the actions.

## Nested Makefiles
To run multiple Makefiles in different directories, first change directory and then invoke `make`. Using the environment variable `$(MAKE)` gives greater flexibility for running multiple Makefiles.

## Using Shell Command Output in a Variable
You can use the `$(shell ...)` function to capture the output of a shell command and store it in a variable. For example, to get the list of files in the current directory, you could use `LS_OUT = $(shell ls)`.
