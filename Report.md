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
각 함수들은 거의 동일한 순서로 진행된다.
1. valid 값을 전부 1(참)으로 만든다.
2. check[] 배열을 만들고 검사 중인 각 부분(행, 열, 서브그리드)의 요소 값이 n이라고 할 때 check[n]의 값을 1 늘린다. (만약, 각 부분의 요소 중 중복된 값이 있거나 빠진 값이 있을 경우, 올바르지 않기 때문)
3. check[] 배열의 값을 검사해 만약, 1이 아닌 값이 있다면 valid 값을 0(거짓)으로 바꾼다.

위 순서로 진행하면 올바르지 않은 행, 열, 서브그리드는 거짓으로 설정되게 되므로 위 순서로 흐름을 만들었다.

---
## 소스파일
### 기본 파일 설명
* Makefile: make를 행함.
* proj2-1.skeleton.c: main 함수가 존재한다. 모든 기능은 이 파일에서 전부 생성한다.

### 소스코드
* Makefile

## 컴파일

<img src="https://raw.githubusercontent.com/Leafsan/sudoku/master/Report/%EC%BB%B4%ED%8C%8C%EC%9D%BC.png">
pthread.h 헤더파일을 가진 프로그램을 gcc로 컴파일 하기위해서 -lpthread 옵션을 넣었다.

<img src="https://raw.githubusercontent.com/Leafsan/sudoku/master/Report/%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EC%98%A4%EB%A5%98.png">
넣지 않을 경우에는 위와 같이 undefined reference 오류가 발생한다.


## 실행 결과
* 정적 검사
<img src="https://raw.githubusercontent.com/Leafsan/sudoku/master/Report/01.png">
의도한대로 원래의 스도쿠 퍼즐과 자리를 바꾼 스도쿠 퍼즐의 검사결과가 나오게 된다.
원래의 스도쿠 퍼즐은 올바른 퍼즐이기에 검사 결과가 모두 YES로 뜨지만 자리를 바꾼 이후의 스도쿠 퍼즐은 서로 바뀐 행, 열, 서브그리드에서 NO가 뜬다.

* 동적 검사
<img src="https://raw.githubusercontent.com/Leafsan/sudoku/master/Report/02.png">
스도쿠 퍼즐을 셔플 중인 스레드가 작동한 이후에 검증을 곧바로 시작했는데도 여전히 셔플 직전의 정렬된 스도쿠 퍼즐이 나오게 된다. 예상과는 사뭇 다른 동작이 발생했다. 셔플 도중에 읽기와 쓰기가 일어날 것이라 생각했는데 셔플 이전에 검증을 미리 끝낸 후에 셔플이 완료되는 동작을 보여주고 있다.
<img src="https://raw.githubusercontent.com/Leafsan/sudoku/master/Report/03.png">
그래서 약간의 시간차를 주기 위해서 셔플과 검증 함수 사이에 usleep(1)을 넣어 보았다.
<img src="https://raw.githubusercontent.com/Leafsan/sudoku/master/Report/04.png">
그 결과는 처음과 달리 셔플 이후에 검증이 이루어지게 되었다. usleep으로 준 시간차 정도로는 부족했던 것으로 생각된다. 일단은 이 결과로 스레드가 생성되고 실행되고 있다고 판단을 할 수 있다고 생각한다.

## 결론
동적 검사 부분에서 일반적으로 생각하는 방식과는 달리 작동하는 것에 약간 의구심이 들지만 작동 자체는 잘 되고 있는 것을 볼 수 있었다.
