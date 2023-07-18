# Writing a Makefile for Source and Object Files in the Same Directory

A Makefile is a special file containing shell commands that you might type to compile and/or update your program. It allows you to automate the build process of your project, avoiding manual compilation.

In this explanation, we will use a simple example with a `src` directory in the current working directory containing `main.c` and `module.c` files.

```makefile
# Variables
EXEC := app
SRC  := $(wildcard src/*.c)  # The `wildcard` function in `make` expands the wildcard `*`
OBJS := $(SRC:.c=.o)         # Substitution reference; it replaces the .c extension with .o for each file

GCC   := gcc                 # Compiler; change it here if needed
CFLAGS := -Wall              # Compiler flags; `-Wall` enables all compiler's warnings
CFLAGS += -I include/        # Append `-I include/` to compiler flags to include the directory in the header file search path

# Phony Targets
.PHONY: all clean            # Explicitly declare `all` and `clean` as phony targets to prevent conflicts with files of the same name

# Default target
all: $(OBJS)                 # Link the object files to produce the executable
	$(GCC) $^ -o $(EXEC).out

# Pattern rule for building object files
%.o: %.c                     # Rule for generating .o files from .c files
	@echo Building $@...      # Print a message before compiling
	$(GCC) $(CFLAGS) -c $< -o $@  # Compile the source file into an object file

# Clean up the build
clean:
	rm -rf $(OBJS) $(EXEC).out
```

This Makefile works as follows:

1. The variables `EXEC`, `SRC`, `OBJS`, `GCC`, and `CFLAGS` are defined. These are used throughout the Makefile to refer to the executable name, the source files, the object files, the compiler, and the compiler flags, respectively.

2. `.PHONY` is used to declare `all` and `clean` as phony targets. This means that they are not associated with files but are carried out regardless.

3. `all` is the default target. It depends on the `$(OBJS)` target. When `make` is run without arguments, it builds the first target specified in the Makefile, which in this case is `all`. The command `$(GCC) $^ -o $(EXEC).out` is executed to link the object files and produce the executable.

4. `%.o: %.c` is a pattern rule for building the object files. It indicates how to create a `.o` file from a `.c` file. In the command `$(GCC) $(CFLAGS) -c $< -o $@`, `$<` refers to the first prerequisite (the `.c` file), and `$@` refers to the target (the `.o` file). The `-c` option tells GCC to compile the source file into an object file.

5. The `clean` target has no prerequisites and its associated command removes all the object files and the executable, thereby cleaning up the build directory.

Makefile Special Variables:

- `$@`: The file name of the target.
- `$^`: The names of all the prerequisites, separated by spaces, with duplicate names removed.
- `$<`: The name of the first prerequisite.
- `@`: Prefixing a command with `@` prevents that command from being printed to the terminal before it is executed.
