EXEC = app
VPATH = ../src/ ../src/include

all:$(EXEC)

$(EXEC):$(OBJS)
	gcc  $^ -o $@.out

%.o: %.c 
	@echo $@ 
	gcc -I include/ -c $< -o $@

clean:
	rm -rf src/*.o
	rm -rf *.out

