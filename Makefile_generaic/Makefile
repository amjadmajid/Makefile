GCC = gcc # if the compiler needs to be changed, we need to change it only here (the benefit of using a variable)
EXEC := app
SRC = $(wildcard *.c) # get all the source files
OBJS = $(SRC: .c = .o) # all object files depend on the source files

# although the object files do not exits, we made them depending on the source files when we defined the variable OBJS
$(EXEC):$(OBJS) 
	$(GCC) -W $^ -o $@


# a general rule to build the object files
# the wildcard must be expanded to the same string on the both sides of the colon 
%.o: %.c
	$(GCC) -I. -c $(CFLAGS) $< -o $@

clean:
	rm -rf $(EXEC) *.o

# to avoid conflict with any file named test or clean we indicate to make that 
# these targets are PHONY
.PHONY: test clean

test: $(OBJS)
	@echo $(SRC)
	@echo $(OBJS)
