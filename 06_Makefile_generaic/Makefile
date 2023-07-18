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

