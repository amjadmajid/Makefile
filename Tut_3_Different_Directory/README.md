# Defining Variables

The following variables are declared for convenience and easy modification. `EXEC` holds the name of the final executable file, `SRC_DIR` is the source directory, `BLD_DIR` is the directory to hold the built objects, and `INC` is the directory of include files.

```makefile
EXEC 	:= app
SRC_DIR := src
BLD_DIR := build
INC	 	:= include
```

`SRC` is assigned a list of source files using the `wildcard` function, which safely expands the `*` wildcard to match all `.c` files in the source directory.

```makefile
SRC  	:= $(wildcard $(SRC_DIR)/*.c)
```

`OBJS` is assigned the list of object files. Since these files don't exist yet, we construct their names from the source file names, replacing the necessary path sections to generate the object file paths. This is done using the `patsubst` function.

```makefile
OBJS	:= $(patsubst $(SRC_DIR)/%.c,$(BLD_DIR)/%.o, $(SRC))
```

`.PHONY` is used to declare targets that aren't associated with files, to prevent conflicts with files of the same name.

```makefile
.PHONY: bin bld_dir clean details
```

An error will occur if you try to build `all` target like this:

```makefile
all: bld_dir $(OBJS)
    gcc $^ -o $@.out
```

`$^` is a special `make` variable representing all the prerequisites of a target. In this case, it will be expanded to `bld_dir build/main.o build/module.o ...`. However, `bld_dir` is not an object file, so `gcc` will throw an error. To solve this problem, `bld_dir` and `bin` targets should be separated, as shown below:

```makefile
all: bld_dir bin
	
bin: $(OBJS)
	gcc $^ -o $(EXEC).out

bld_dir:
	@mkdir -p $(BLD_DIR)
```

In the following pattern rule, note several points:

```makefile
$(BLD_DIR)/%.o: $(SRC_DIR)/%.c 
	@echo $@ 
	gcc -I $(INC) -c $< -o $@
```

1. `%` is a wildcard that is replaced with the same string on both sides of the `:`. This ensures that an object file with the same name as the corresponding source file is generated.

2. The `make` utility will build the targets in the `BLD_DIR` because the target string is of the format `build/filename.o`.

3. Each target (like `build/filename.o`) depends on the corresponding source file (`src/filename.c`). This ensures that if the source file changes, the corresponding object file will be rebuilt.
