# Writing a Makefile for Source and Object Files in the Same Directory

In this explanation, we'll assume the existence of a `src` directory in the current working directory containing `main.c` and `module.c` files.

```makefile
# Variables
EXEC := app
SRC  := $(wildcard src/*.c)  # The `wildcard` function in `make` expands the wildcard `*`
OBJS := $(SRC:.c=.o)         # Substitution reference; expands to `OBJS = src/main.o src/module.o`
                             # IMPORTANT: Don't put a space between `:` and `.`

GCC   := gcc                 # Compiler; change it here if needed
CFLAGS := -Wall              # Compiler flags; `-Wall` enables all compiler's warnings
CFLAGS += -I include/        # Append `-I include/` to compiler flags

# Phony Targets
.PHONY: all clean            # Explicitly declare `all` and `clean` as phony targets to prevent conflicts with files of the same name

# Default target
all: $(OBJS)                 # Link the object files to produce the executable
	$(GCC) $^ -o $(EXEC).out

# Pattern rule for building object files
%.o: %.c                     # The `%.o: %.c` rule means 'for each .o file, use the corresponding .c file to compile it'
	@echo Building $@...      # Print a message
	$(GCC) $(CFLAGS) -c $< -o $@  # Compile the source file into an object file

# Clean up the build
clean:
	rm -rf $(OBJS) $(EXEC).out
```

This Makefile compiles all `.c` files in the `src` directory into corresponding `.o` files, then links all object files to create an executable named `app.out`. The `clean` target removes all generated files. 

- `$@`: The file name of the target of the rule.
- `$^`: The names of all the prerequisites, with spaces between them.
- `$<`: The name of the first prerequisite.
- `@`: Suppress command echo.

This Makefile is versatile and scalable: you can add more source files in the `src` directory and they will automatically be included in the build.
