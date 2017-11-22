# How to write a Makefile
				
 ## Manual compilation
To manually compile a project and produce an executable follow the following instructions:
```
$ gcc -I . -c main.c  ->  main.o
$ gcc -I . -c module.c ->  module.o
$ gcc main.o module.o -o target_bin -> target binary 
// -I is used to include the current directory (.) as a header file location.
```

 ## General syntax of a Makefile

	The general syntax of a Makefile rule is as follows:

	arget: dependency1 dependency2 ...
	[TAB] action1
	[TAB] action2
    ...

## General remarkes 
1. By convention,  variable's names are written in upper-case form, i.e. CC = gcc.
2. A varaible can be accessed using one of these ${VAR}, $(VAR) syntaxes.
3. If no target is specified, make is defaulted to target the first target in the Makefile.
4. Each make line is executed in a separate sub-shell environment. Therefore, a command like `cd newdir` will not affect the next lines.


##  Simple make file for our project (dependency tree structure)
```	
all: main.o module.o 				 //dependencies of target all 	
	gcc main.o module.o -o target_bin	 //action to make target all	
main.o: main.c module.h 			 //dependencies fo target main.o 	
	gcc -I . -c main.c 			 //action to make target main.o ,
module.o: module.c module.h														
	gcc -I . -c module.c 			//-I indicate header file locations	
clean:						This target has no dependencies						
	rm -rf *.o 													
	rm target_bin
```

## How does make utilize the timestamp of files	
If make found a dependency with a newer timestamp than the target, it will 
remake that target and all the targets that are depending on it. 
For example, if the source file *module.c* is modified, make will remake
the *module.o* and the target *all*  when the program is rebuilt. However, make will not remake
 the *main.o* since it has a newer timestamp than the source file *main.c*.
Furthermore, special targets are not files and such their related actions are always executed 
if they are specified.


## Dealing with assignment operator	
	
### Simple assignment (:=)	
A simple assignment expression is evaluated only once, at the very first occurrence. 
For example, if `CC :=${GCC} ${FLAGS}` during the first encounter is evaluated to `gcc -W` then 
each time `${CC}` occurs it will be replaced with `gcc -W`.
	
### Recursive assignment(=)	
A Recursive assignment expression is evaluated everytime the variable is encountered 
in the code. For example, a statement like ` CC = ${GCC} {FLAGS}` will be evaluated only when
 an action like `${CC} file.c` is executed. However, if the variable `GCC` is reassigned i.e
`GCC=c++` then the `${CC}` will be converted to `c++ -W` after the reassignment. 
	
### Conditional assignment (?=)	
Conditional assignment assigns a value to a variable only if it does not have a value	
	
### Appending (+=)	
Assume that `CC = gcc` then the appending operator is used like `CC += -w` 	
then `CC` now has the value `gcc -W`


 ### Using patterns and special variables	
	
When wildcard % appears in the dependency list, it is replaced with	
the same string that was used to perform substitution in the target.
* Inside actions we can use:	

  -$@ to represent the full target naem of the current target 	
  -$? returns the dependencies that are newer than the current target 	
  -$* returns the text that corresponds to % in the target 	
  -$< returns the name of the first dependency 	
  -$^ returns the names of all the dependencies with space as the delimiter


 Action modifiers	
	
	- (minus) Prefixing an action with - tells make to ignore any error occurs	
	in that line. By default, execution of a Makefile stops when any command returns 
	a non-zero (error) value. 	
	@ (at) suppresses the standard print-action-to-standard-output behaviour of make  
	For exampele, @echo OutputMessage will print "OutputMessage" and suppresses 	
	printing the action echo OutputMessage. 


 Using PHONY to avoid file-target name conflicts	
	
	If the project directory contains a file with same names as a special target 	
	in the Makefile (i.e. all, clean), that will result in a conflict and make will	
	produce an error. Using .PHONY directive to specify which targets are not ot be  
	considered as files, for instance, .PHONY: all clean


 Check make execution before the actual building (dry run)	
	
	At times, maybe when developing the Makefile, we may want to trace the make 	
	execution (and view the logged messages) without actually running the actions,  
	which is time consuming. Simply use make -n to do a “dry run”.

 Nested Makefiles	
	
	To run a multiple make files in different directories, first  change directory 	
	and then invoke make. Using the environment variable $(MAKE) gives greater 	
	flexibility to run multiple Makefiles. For example, $(MAKE) enables passing the 
	-n option for the "dry run"	
	subdir:	
		cd subdir %% $(MAKE)


 Using the shell command output in a variable 	
	
	Sometimes we need to use the output from one command/action in other places in the 	
	Makefile — for example, checking versions/locations of installed libraries, or other 
	files required for compilation. We can obtain the shell output using the shell 		
	command. For example, to return a list of files in the current directory into a 	
	variable, we would run: LS_OUT = $(shell ls).
The dependencies of the target all 	
		gcc main.o module.o -o target_bin	
