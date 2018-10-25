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

.PHONY: bin bld_dir clean details

all: bld_dir bin
	
bin: $(OBJS)
	gcc $^ -o $@.out

bld_dir:
	@mkdir -p $(BLD_DIR)

$(BLD_DIR)/%.o: $(SRC_DIR)/%.c 
	@echo $@ 
	gcc -I $(INC) -c $< -o $@

clean:
	rm -rf $(BLD_DIR) 
	rm -rf *.out

details:
	@ echo "Source files:" $(SRC)
	@ echo "Object files" $(OBJS)
