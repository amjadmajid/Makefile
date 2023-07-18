Great! Your provided generic Makefile gives an excellent starting point for most C projects. Here is a slightly edited version to ensure clarity and correctness.

```Makefile
CC = gcc  # If the compiler needs to be changed, we need to change it only here
EXEC = app
CFLAGS = -I. -Wall  # Flags for the compiler: include the current directory and turn on all warnings
SRC = $(wildcard *.c)  # Get all the source files
OBJS = $(SRC:.c=.o)  # Replace the .c from SRC with .o

# Default target is EXEC
# It depends on the object files
$(EXEC): $(OBJS) 
	$(CC) $^ -o $@

# Generic rule for turning .c files into .o files
%.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@

# Clean the build
clean:
	rm -rf $(EXEC) *.o

# PHONY targets
# They are not associated with files
.PHONY: clean test

# A target for printing variables
test: 
	@echo Source files: $(SRC)
	@echo Object files: $(OBJS)
```
With this Makefile, simply typing `make` will build your program using GCC, and all C files in the current directory will be included. The `clean` target will remove all generated files. The `test` target is useful for checking what source files were found and how the object files would be named.

This generic Makefile assumes all C files in the directory are part of the project, and they all should be compiled and linked together. If the project structure becomes more complex, you'll need to adjust this Makefile accordingly.

Note: `-W` flag is replaced with `-Wall` flag in the edited version as `-W` alone doesn't represent a valid GCC warning flag.
