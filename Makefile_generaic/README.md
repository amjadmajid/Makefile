### Dealing with assignment operator

##### Simple assignment (:=)
A simple assignment expression is evaluated only once, at the very first occurrence.
For example, if `CC :=${GCC} ${FLAGS}` during the first encounter is evaluated to `gcc -W` then
each time `${CC}` occurs it will be replaced with `gcc -W`.

##### Recursive assignment(=)
A Recursive assignment expression is evaluated everytime the variable is encountered
in the code. For example, a statement like ` CC = ${GCC} {FLAGS}` will be evaluated only when
 an action like `${CC} file.c` is executed. However, if the variable `GCC` is reassigned i.e
`GCC=c++` then the `${CC}` will be converted to `c++ -W` after the reassignment.

##### Conditional assignment (?=)
Conditional assignment assigns a value to a variable only if it does not have a value

##### Appending (+=)
Assume that `CC = gcc` then the appending operator is used like `CC += -w` 	
then `CC` now has the value `gcc -W`


### Using patterns and special variables

When wildcard % appears in the dependency list, it is replaced with
the same string that was used to perform substitution in the target.
* Inside actions we can use:
  - $@ to represent the full target name of the current target 	
  - $? returns the dependencies that are newer than the current target 	
  - $\* returns the text that corresponds to % in the target 	
  - $< returns the name of the first dependency 	
  - $^ returns the names of all the dependencies with space as the delimiter


### Action modifiers
* Prefixing an action with `-` tells make to ignore any error occurs in that line.
. By default, execution of a Makefile stops when any command returns
a non-zero (error) value. 	
* @ (at) suppresses the standard print-action-to-standard-output behaviour of make  
For exampele,`@echo OutputMessage` will print "OutputMessage" and suppresses 	
printing the action "echo OutputMessage".

### Using PHONY to avoid file-target name conflicts
If the project directory contains a file with same names as a special target 	
in the Makefile (i.e. all, clean), that will result in a conflict and make will
produce an error. Using `.PHONY` directive to specify which targets are not to be  
considered as files, for instance, `.PHONY: all clean`.


 ## Check make execution before the actual building (dry run)
At times, maybe when developing the Makefile, we may want to trace the make 	
execution (and view the logged messages) without actually running the actions,  
which is time consuming. Simply use `make -n` to do a “dry run”.

### Nested Makefiles	
To run a multiple make files in different directories, first  change directory 	
and then invoke `make`. Using the environment variable `$(MAKE)` gives greater 	
flexibility to run multiple Makefiles. For example, `$(MAKE)` enables passing the
`-n` option for the "dry run"
	subdir:
		cd subdir %% $(MAKE)


### Using the shell command output in a variable 	

Sometimes we need to use the output from one command/action in other places in the 	
Makefile.To do that a sell command can be used. For example, to get the list of files in
in the current directory we would run: LS_OUT = $(shell ls).
