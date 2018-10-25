## Writing a Makefile when the source and object files are in the same directory (folder)
#### The explanation is based on the following assumption: there are a _src_ directory in the _current working directory_, and the _src_ directroy contains _main.c_ and _module.c_ files
```
EXEC := app
SRC  := $(wildcard src/*.c) # The `wildcard` is a `make` function that ensure the expansion of the wildcard `*`
OBJS =$(SRC:.c=.o) 	    # Using the "substitution References" this line will be expand to  OBJS = src/main.o src/module.o
			    # DON'T put a white space between the ':' and '.'  in this line. 
		
GCC     := gcc 		    # If the compiler needs to be change only here we need to change it (benefit of using variables)
CFLAGS  := -Wall  	    # here the simple assignment operator is used 
CFLAGS+= -I include/   	    # The append operator is used to add to CFLAGS variable 

.PHONY: all clean	    # To avoid conflicts, we explicitly tell `make` that 'all' and 'clean' are not to be considered as real files


all:$(OBJS)                 # link the object files to produce the executable
	$(GCC)  $^ -o $@.out

%.o: %.c                    # build the object files
	@echo $@ 
	$(GCC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf  $(OBJS) *.out
```
