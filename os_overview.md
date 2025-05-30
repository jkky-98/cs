# 운영체제

`운영체제(OS)`는 실행할 프로그램에 필요한 **자원을 할당**하고, 프로그램이 올바르게 실행되도록 돕는 특별한 프로그램이다.

운영체제 역시 **프로그램**이기 때문에 메모리에 적재되어 실행된다.

운영체제는 컴퓨터를 동작시키는 매우 중요한 프로그램이기 때문에 메모리의 영역에서도 `커널 영역`이라는 고정된 영역에 할당된다.

우리가 일반적으로 사용하는 실행중인 프로그램(메모장, 웹 브라우저, 디스코드)들은 `사용자 영역`에 할당된다.

우리가 프로그램을 실행 할 때, 익숙한 GUI기반 윈도우 OS에서 "크롬" 아이콘을 더블 클릭해서 크롬을 실행할 때
우리는 더블 클릭만 했을 뿐 메모리 X번지에 크롬을 적재해달라는 명령을 요청한 적이 없다. 하지만 크롬은 자연스럽게 실행되고 메모리 특정 번지에 안착한다.

실행한 프로그램이 메모리 사용자 영역 어딘가에 안착하도록하는 것이 OS의 역할이다.

## 운영체제의 CPU 관리

디스코드, 메모장, 크롬이 실행중인 상태라고 가정해보자.

이 상황에서 운영체제또한 당연히 실행중일 것이다.

컴퓨터를 이용하는 실사용자는 디스코드, 메모장, 크롬이 동시에 실행되는 것 처럼 느낀다.

하지만 실제로는 이 세 개의 응용프로그램을 동시에 동작하는 것 처럼 보일 만큼 아주 빠르게 번갈아가며 CPU가 실행하는 것이다.

### ❓❓멀티코어-멀티스레드 CPU기반이 아니라 싱글코어-싱글스레드 기반으로만 유효한 설명 아닌가?

반은 맞고 반은 틀리다.

3개의 프로그램이 50개의 프로그램 쓰레드를 가진다고 가정해보자.

8코어 16쓰레드 CPU를 가정해보자.

이 상황에서 쓰레드를 처리할 수 있는 하드웨어 쓰레드는 16개이며 프로그램 쓰레드는 50개이다.

프로그램 쓰레드는 손님이며 하드웨어 쓰레드는 손님의 주문을 처리하는 워커이다.

워커가 1명이든 16명이든 50명의 손님은 줄을 서야한다. 

다만 50명의 손님은 16명인 시스템에서 더 빨리 주문이 처리되는 것 처럼 느낄 것이다.

이 처럼 다중코어 다중스레드 CPU에서는 프로그램 쓰레드를 동시에 빠르게 처리해나간다.

### ❓❓구체적으로 프로그램 쓰레드가 멀티코어 쓰레드에서 어떻게 실행되나?

응용프로그램 : A, B, C
응용프로그램의 쓰레드 : A1~A10, B1~10, C1~10

운영체제는 `프로그램 쓰레드`가 A소속인지 B소속인지 C소속인지 고려는 하지만 기본적으로는,
**쓰레드 단위로 CPU를 스케줄링한다.**

8코어 16스레드 CPU라면 16개의 하드웨어 쓰레드가 존재하는 것이며

16개의 하드웨어 쓰레드들은 A1~C10(30개의 프로그램 쓰레드)를 아주 잠깐 하나씩 담당해서 프로그램 쓰레드의 작업을 실행시키고 다른 프로그램 쓰레드의 작업을 실행시키고 이것을 반복해나간다.

1개의 하드웨어 쓰레드가 아주 짧은 시간 하나의 프로그램 쓰레드를 담당해서

그 프로그램 쓰레드의 작업(기계어 명령어)을 아주 잠깐 처리하고 다른 프로그램 쓰레드의 작업으로 스위칭한다.

쓰레드 단위로 스케줄링 하기에

16개의 하드웨어 쓰레드 CPU는 1개의 프로그램을 여러 하드웨어 쓰레드에서 동시에 실행시킬 수 있는 것이다.

| 하드웨어 쓰레드 | 실행 중인 쓰레드 | 소속 프로그램 |
| -------- | --------- | ------- |
| 1        | A의 쓰레드 1  | 프로그램 A  |
| 2        | A의 쓰레드 2  | 프로그램 A  |
| 3        | B의 쓰레드 1  | 프로그램 B  |
| 4        | B의 쓰레드 2  | 프로그램 B  |
| 5        | C의 쓰레드 1  | 프로그램 C  |
| 6        | C의 쓰레드 2  | 프로그램 C  |
| 7        | A의 쓰레드 3  | 프로그램 A  |
| 8        | A의 쓰레드 4  | 프로그램 A  |
| ...      | ...       | ...     |

## 운영체제의 입출력 장치 관리

A응용 프로그램이 프린터를 사용중이라면 B응용 프로그램에서의 프린터 사용을 막는 것의 예가 운영체제의 일이다.

또한 보조기억장치의 데이터를 파일이나 디렉토리(폴더)로 묶어서 관리할 수 있게 해주는 것도 운영체제의 일이다.

# 운영체제를 왜 알아야 하나?

일반적으로 응용프로그램을 다운로드 받고 사용하는 실사용자는 운영체제를 알 필요는 없어보인다.

운영체제는 프로그램을 위한 프로그램이기 때문이다.

하지만 프로그램을 만드는 `개발자`는 운영체제를 고려해야 한다.

결과적으로 개발자가 만드는 프로그램은 운영체제의 도움을 받아 실행되는 프로그램이기 때문이다.

# 운영체제의 커널

운영체제는 여러 종류가 존재하고 프로그램 중 가장 거대한 프로그램에 속한다.

그 중심에 있는 커널은 놀이공원의 운영 본부처럼, 수많은 사람(프로그램)이 놀이기구(자원)를 안전하게 즐길 수 있도록 통제하고 조율하는 역할을 한다.

각 사람이 자유롭게 기계를 조작하면 사고가 나듯, 프로그램이 하드웨어를 직접 다루면 시스템이 망가질 수 있기 때문에, 커널이 모든 요청을 받아 적절히 나눠주고 문제없이 돌아가게 한다.

# 이중 모드와 시스템 호출

## 이중 모드

CPU가 명령어를 실행하는 모드를 크게 `사용자 모드`와 `커널 모드`로 구분한다.

이러한 이중 모드는 CPU 플래그 레지스터의 `슈퍼바이저 플래그`기준으로 구분되어 실행된다.

플래그가 1일 경우 커널 모드로, 0일 경우 사용자 모드로 실행된다.

## 사용자 모드

응용 프로그램이 실행되는 모드로 제한된 권한을 가져 커널 모드와 달리 하드웨어에 직접 접근이 불가능하다.

파일 시스템, 메모리, I/O장치 등에 접근하기 위해 꼭 `시스템 콜(system call - 인터럽트의 일종)`을 통해 접근이 가능하다.

자원 접근에 대해 커널 모드를 거치게 하는 이유는 사용자 모드에서 오류가 발생하더라도 시스템 전체에 영향을 주지 않도록 보호하기 위해서이다.

## 커널 모드

모든 하드웨어 자원에 대한 접근 권한을 보유한다.

메모리 관리 및 파일 시스템 등 핵심 기능을 수행하며

`시스템 콜`을 처리하고 결과를 반환한다.

즉 사용자 모드에서 자원 접근이 필요할 경우 시스템 콜을 통해 커널 모드로 하여금 응용프로그램에 결과를 제공할 수 있는 것이다.

혹은 사용자 모드에서의 다른 인터럽트를 통해 커널 모드로의 전환이 가능하다.

# 운영체제의 핵심 서비스

운영체제(OS)는 하드웨어와 응용 프로그램 사이에서 중재자(mediator) 역할을 하며, 다양한 핵심 서비스를 제공합니다. 이 과정에서 **이중 모드(Dual Mode: 사용자 모드 & 커널 모드)**를 적절히 활용해 안정성과 보안성을 확보한다.

## 프로세스 관리

프로세스는 간단하게는 `실행 중인 프로그램`이라고 부른다.

운영체제는 이러한 프로세스들을 관리한다.

운영체제는 프로세스 생성, 종료, 상태 전이 등을 관리하고 컨텍스트 스위칭을 통해 여러 프로세스가

동시에 실행되는 것 처럼 짧고 빠르게 프로세스들을 실행시킨다.

## 자원 접근 및 할당

운영체제는 CPU, 메모리, 입출력 장치등 하드웨어에 해당하는 시스템 자원을 효율적으로 분배한다.

CPU 스케줄러는 어떤 프로세스에 CPU를 할당 할지 결정하고 메모리 관리자는 가상 메모리, 페이징을 통해

메모리를 할당하고 보호한다.

## 파일 시스템 관리

운영체제는 보조저장장치 즉 디스크 상의 데이터를 파일이라는 단위로 관리한다.

파일의 생성, 읽기, 쓰기, 삭제등의 작업을 운영체제의 파일 시스템 인터페이스로 하여금 처리한다.