Defining variables
```
EXEC 	:= app
SRC_DIR := src
BLD_DIR := build
INC	 	:= include
```
The `wildcard` function ensure a safe expansion to `*`.
The variable `SRC` will contain a list of paths to the specified source files. 
```
SRC  	:= $(wildcard $(SRC_DIR)/*.c)
```
The object files do not exist yet. Therefore, we cannot directly construct paths to them. 
Instead, we rely on the source files' paths and replace the necessary path sections to generated
the object paths. 
The `make` function `patsubst` enables us to manipulate strings.
For example, if SRC_DIR is 'src' and BLD_DIR is 'build' and the SRC is 'src/main.c', then 
OBJS after running the blow command will be `build/main.o`

```
OBJS	:=$(patsubst $(SRC_DIR)/%.c,$(BLD_DIR)/%.o, $(SRC))
```
```
.PHONY: bin bld_dir clean details
```
Why the following two lines of code will generate an error? 
```
all: bld_dir $(OBJS)
    gcc $^ -o $@.out
```
The `make` special varialbe `$^` represents the entire prerequistes.
Therefore, `gcc $^ -o $@.out` will be interpreted as `gcc bld_dir build/main.o build/module.o ...`; Since 
bld_dir is not an object file this command will generate an error. The solution to this problem is to 
separate these two prerequistes as it has been done below. 

```
all: bld_dir bin
	
bin: $(OBJS)
	gcc $^ -o $@.out

bld_dir:
	@mkdir -p $(BLD_DIR)
```
There are a few points that require attention in the following code snippet.
1. The `%` will expand to the same string on both sides of the colon `:`. Therefore, the object file will have the same name as the correspoinding source file. 
2. The `make` utility will build the targets in the build directory since the target string is of such format `build/filename.o`
3. Each traget, i.e. `build/filename.o`, depends on the corresponding source file, i.e. `src/filename.c`.
```
$(BLD_DIR)/%.o: $(SRC_DIR)/%.c 
	@echo $@ 
	gcc -I $(INC) -c $< -o $@
```
