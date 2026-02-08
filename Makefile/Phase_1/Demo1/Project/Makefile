app: main.o add.o
	gcc main.o add.o -o app

main.o: main.c add.h
	gcc -c main.c

add.o: add.c add.h
	gcc -c add.c

clean:
	rm -f *.o app
