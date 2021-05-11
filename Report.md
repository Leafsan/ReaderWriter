# 한양대학교 ERICA Operating Systems Project3 보고서
기계공학과 2015040719 최우경


## 기본 설명
이번에는 주어진 골격 코드의 Reader/Writer 스레드를 뮤텍스로 제어하는 프로젝트였다.

## Reader/Writer

### 요약

이번 프로젝트에서는 두개의 파일이 존재한다.

* writer_prefer.c: 이 소스코드에서는 Writer 스레드가 항상 우선해서 동작한다.
* fair_reader_writer.c: 이 소스코드에서는 Reader와 Writer 스레드가 동등하게 경쟁해서 동작한다.

### 알고리즘 요약
각 소스코드는 다음과 같이 동작한다.
* writer_prefer.c: count_write와 count_read 두개의 전역변수를 이용해서 현재 critical section에 접근한 Writer, Reader 스레드의 수를 센다. 뮤텍스는 Reader 스레드가 critical section을 현재 접근한 경우가 아니면, Writer에게 우선적으로 주어지고, Writer의 스레드 이후로 다른 Writer 스레드가 대기 중이면 뮤텍스를 해제하지 않고 기다린다.
* fair_reader_writer.c: count_read만의 전역변수를 이용해서 critical section에 접근한 Reader 스레드의 수를 센다. mutex_fair 뮤텍스는 critical section에 접근하는 스레드에게 접근하는 순서대로 주어지고 순서대로 처리하도록 한다(FIFO).


---
## 소스파일
### 기본 파일 설명
* Makefile: make를 행함. 이하 소스코드 별로 실행 파일을 각각 생성한다.
* writer_prefer.c: main 함수가 존재한다. writer 스레드를 우선적으로 뮤텍스를 제공하는 방식.
* fair_reader_writer.c: main 함수가 존재한다. 먼저 접근한 스레드에게 우선적으로 뮤텍스를 제공하는 방식(FIFO).

### 소스코드
* Makefile

## 컴파일

pthread.h 헤더파일을 가진 프로그램을 gcc로 컴파일 하기위해서 -lpthread 옵션을 넣었다.


## 실행 결과


## 결론

