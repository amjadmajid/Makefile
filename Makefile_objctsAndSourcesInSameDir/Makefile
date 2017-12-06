EXEC = app
SRC = $(wildcard src/*.c)

# The explanation is based on the following assumption: There are a src directory in the current working directory
# and the src directroy contains main.c and module.c files

OBJS=$(SRC:.c=.o) 	# Using the "substitution References" this line will be expand to  OBJS = src/main.o src/module.o
			# DON'T put a white space between the : and .  in this line. 
		
GCC := gcc 		# If the compiler needs to be change only here we need to change it (benefit of using variables)
CFLAGS := -Wall  	# here the simple assignment operator is used 
CFLAGS+= -I include/   	# The append operator is used to add to CFLAGS variable 

.PHONY: all clean	# To avoid conflicts, we explicitly telling make that all and clean are not to be considered as real files

all:$(EXEC)

$(EXEC):$(OBJS)
	$(GCC)  $^ -o $@.out

%.o: %.c 
	@echo $@ 
	$(GCC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf  $(OBJS) *.out

