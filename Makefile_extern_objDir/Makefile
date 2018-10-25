EXEC 	:= app
SRC_DIR := src
BLD_DIR := build
SRC  	:= $(wildcard $(SRC_DIR)/*.c)
INC	 	:= include
OBJS	:=$(patsubst $(SRC_DIR)/%.c,$(BLD_DIR)/%.o, $(SRC))

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
