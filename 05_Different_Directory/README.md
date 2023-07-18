# Writing a Makefile for Source and Object Files in Different Directories

In many cases, it is helpful to keep the source code and the compiled objects in separate directories to make organization easier. This guide will explain how to write a Makefile for this purpose. 

The tutorial assumes that there is a `src` directory in the current working directory and a `build` directory is used to hold the object files. The `src` directory contains `main.c` and `module.c` files, and the `include` directory contains the header files.

Let's start with setting some variables:

```makefile
EXEC    := app
SRC_DIR := src
BLD_DIR := build
INC     := include
```
The variables defined above are:
- `EXEC`: Name of the final executable file
- `SRC_DIR`: Directory of the source files
- `BLD_DIR`: Directory to hold the built objects
- `INC`: Directory of the header files

Now, we can use the `wildcard` function to populate the `SRC` variable with all of the C source files in our `SRC_DIR` directory:

```makefile
SRC     := $(wildcard $(SRC_DIR)/*.c)
```

We will need to compile each of these source files into an object file to be linked. These object files should be placed in `BLD_DIR`. We use the `patsubst` function to create a corresponding `.o` file in `BLD_DIR` for each source file in `SRC`.

```makefile
OBJS    := $(patsubst $(SRC_DIR)/%.c, $(BLD_DIR)/%.o, $(SRC))
```

We will use `.PHONY` to prevent `make` from getting confused by files of the same name as our targets:

```makefile
.PHONY: all bld_dir clean
```

We will create two targets `all` and `bld_dir`. The `all` target is responsible for creating the final executable. `bld_dir` is responsible for creating the build directory if it does not exist.

```makefile
all: bld_dir $(EXEC)
	
$(EXEC): $(OBJS)
	gcc $^ -o $(EXEC)

bld_dir:
	@mkdir -p $(BLD_DIR)
```

We need to instruct `make` how to build a `.o` file in the `BLD_DIR` from a `.c` file in the `SRC_DIR`. 

```makefile
$(BLD_DIR)/%.o: $(SRC_DIR)/%.c
	@echo Building $@...
	gcc -I $(INC) -c $< -o $@
```

Finally, a `clean` target is useful for removing all built files:

```makefile
clean:
	rm -rf $(BLD_DIR) $(EXEC)
```

The complete Makefile:

```makefile
EXEC    := app
SRC_DIR := src
BLD_DIR := build
INC     := include

SRC     := $(wildcard $(SRC_DIR)/*.c)
OBJS    := $(patsubst $(SRC_DIR)/%.c, $(BLD_DIR)/%.o, $(SRC))

.PHONY: all bld_dir clean

all: bld_dir $(EXEC)
	
$(EXEC): $(OBJS)
	gcc $^ -o $(EXEC)

bld_dir:
	@mkdir -p $(BLD_DIR)

$(BLD_DIR)/%.o: $(SRC_DIR)/%.c
	@echo Building $@...
	gcc -I $(INC) -c $< -o $@

clean:
	rm -rf $(BLD_DIR) $(EXEC)
```

When you type `make`, it will build the `all` target, creating the `build` directory if it does not exist, compile the source files into object files in the `build` directory, and link the object files into the final executable. If you type `make clean`, it will remove all built files.
