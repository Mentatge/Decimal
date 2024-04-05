CC = gcc -Wall -Werror -Wextra -g -std=c11
SRC= $(wildcard s21_*.c)
OBJ= $(patsubst %.c, %.o, ${SRC})
LFLAGS= -lcheck -lpthread 
GCF= -ftest-coverage -fprofile-arcs --coverage

ifeq ($(shell uname -s), Linux)
	LFLAGS += -lrt -lsubunit -lm
endif


all: clean s21_decimal.a gcov_report


rebuild: clean s21_decimal.a

clean:
	rm -rf *.o && rm -rf *.gcno
	rm -rf *.a
	rm -rf *.gcda
	rm -rf *.dSYM
	rm -rf *.info && rm -rf *.gcov
	rm -rf ./test && rm -rf ./gcov_report
	rm -rf ./report/
	rm -rf test_output/

s21_decimal.o:
	$(CC) -c ${SRC} 

s21_decimal.a: s21_decimal.o
	ar rcs s21_decimal.a ${OBJ} 
	ranlib s21_decimal.a
	rm *.o


test: unit_tests.c s21_decimal.a
	$(CC) -o test $^ ${LFLAGS}
	./test

gcov_report: test
	$(CC) $(GCF) unit_tests.c $(SRC)  $(LFLAGS) -o gcov_report
	./gcov_report
	lcov -t "test" -o gcov_report.info -c -d .
	genhtml -o test_output gcov_report.info
	open ./test_output/index.html\

style:
	cp ../materials/linters/.clang-format ./
	clang-format -style=Google -i *.c
	rm -f .clang-format


