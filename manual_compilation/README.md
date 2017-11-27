## Manual compilation
To manually compile a project and produce an executable follow the following instructions:

1. compile all the source files to produce the object files (.c -> .o)

```
$ cc -I . -c main.c  		// this command will produce  main.o

$ cc -I . -c module.c 		// this command will produce  module.o

// -I is used to include the current directory (.) as a header file location.```
2.  link all the object files to produce the executable 
```$ cc main.o module.o -o prog	 // this command produces prog.out```

