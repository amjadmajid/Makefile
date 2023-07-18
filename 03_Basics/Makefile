all:main.o module.o
	gcc -I. main.o module.o -o app.out

# Comment out the main.o ... and the module.o ... to let make applies the implicit rules
main.o: main.c
	gcc -I. -c main.c 

module.o: module.c module.h
	gcc -I. -c  module.c


clean:
	rm -rf *.o
	rm -rf *.out
