# 명령어 집합

**명령어는 어떻게 생겨야 명령어 파이프라이닝에 유리할까?**

CPU가 다루는 명령어의 생김새는 제조사별, 심지어는 같은 제조사라도 CPU마다 다르다.

`명령어 집합(ISA - instruction set architecture)`이란 CPU가 이해할 수 있는 명령어들의 모음이다. 

인텔 CPU의 경우 일반적으로 x86 명령어 집합을, 애플 CPU는 일반적으로 ARM 명령어 집합을 따른다.

## CISC(Complex Instruction Set Computer)

`CISC`란 복잡한 명령어 집합을 사용하는 컴퓨터를 말한다.

인텔이나 AMD사가 사용하는 x86, x86-64가 CISC기반 CPU에 해당한다.

명령어의 형태와 크기가 다양한 `가변 길이 명령어`를 활용한다.

하나의 명령어가 복잡하고 크기가 큰 명령어가 될 수 있어 다양하고 강력한 명령어로 여겨지며

상대적으로 적은 수의 명령어로도 프로그램 실행이 가능하다.

### CISC의 단점

위의 설명으로 느낄 수 있는 CISC의 장점은 메모리를 아낄 수 있다는 것이다.

명령어 수 자체가 적고 하나의 명령어에 최대한 복잡하게 구성이 가능하기 때문이다.

이러한 CISC는 메모리를 최대한 아끼며 개발해야했던 시절에 인기가 매우 높았으나 명령어 파이프라이닝이 불리하다는 치명적인 단점이 존재했다.

즉 용량과 속도라는 측면에서 상대적으로 용량을 배려하기 위해 속도를 포기해야하는 구조인 것이다.

명령어의 크기와 실행되기까지의 시간이 일정하지 않았으며 복잡한 명령어 때문에 명령어 하나를 실행하는 데에
여러 클럭 주기가 필요했던 것이다.

아래의 표 예시를 참고하면, 명령어1의 실행 단계가 매우 길어짐에 따라 다음 명령어가 다음 클럭주기에 바로바로 붙지 못하는 상황을 표현한 것이다.

| 클럭 사이클 | 명령어1            | 명령어2      | 명령어3    |
|-------------|--------------------|--------------|------------|
| T1          | Fetch              |              |            |
| T2          | Decode             |              |            |
| T3          | Execute (1단계)     |              |            |
| T4          | Execute (2단계)     |              |            |
| T5          | Execute (3단계)     | Fetch        |            |
| T6          | Write              | Decode       |            |
| T7          |                    | Execute      | Fetch      |
| T8          |                    | Write        | Decode     |
| T9          |                    |              | Execute    |
| T10         |                    |              | Write      |

또한 CISC의 장점으로 여겨지는 큰 형태의 명령어는 실제로는 사용 빈도가 매우 낮다는 것이다.

즉 쓰던걸 또 쓰는 경우가 매우 많다는 것이다.

실제로 20%에 해당하는 명령어가 사용 빈도 80%이상을 가져간다는 사실이 밝혀지기도 했다.

## RISC(Reduced Instruction Set Computer)

명령어의 종류가 적고, 짧고 규격화된 명령어를 사용한다.

메모리 접근을 최소화 한다, 대신 레지스터를 잘 이용하도록 한다.(CISC에 비해 범용 레지스터의 갯수가 더 많은 경우가 많다.)

명령어의 종류가 적기 때문에 같은 동작을 취한다는 가정에서 CISC보다 더 많은 명령어 수가 필요할 것이다.

굳이 비유하자면 아래의 비유처럼 명령어가 짧게 여러개로 구성되는 것이다.

CISC : Collections.sort(myList);
RISC : 
```java
for (int i = 0; i < list.size(); i++) {
    for (int j = i + 1; j < list.size(); j++) {
        if (list.get(i) > list.get(j)) {
            int temp = list.get(i);
            list.set(i, list.get(j));
            list.set(j, temp);
        }
    }
}
```

이렇게 보면 CISC는 굉장히 추상화 되어있다고 생각한다. 하드웨어 레벨에서 현대의 추세는 추상화로의 방향이 아닌 구체화로의 방향인 것이다.

CISC의 단점이 명확해보이고, RISC가 이에 대안이 되는 것 처럼 보이지만 CISC 진영 또한 이러한 자신들의 단점을 잘 인지하고 있기에 이를 해결하려고 마이크로 명령어와 같은 복잡한 CISC 명령어를 더 쪼개서 적용하는 등 자신들만의 극복방안을 잘 마련하고 있기도 하다.

# 현대의 서버 워크테이션에서의 상황
여전히 Intel/AMD(x86 기반)을 많이 사용한다.

수십 년간 누적된 호환성 및 안정성 측면에서, 대부분의 클라우드 기술이 x86기반을 두고 발전한 역사가 존재하기에 여전히 높은 x86은 시장에서 높은 점유율을 보인다.

하지만 기업용 서버 인프라와 달리 클라우드-모바일 중심으로 ARM의 영향력이 빠르게 커지고 있는 상황이기도 하다.

# 최신의 Mac OS 사용자들이 느낄 수 있는 지점
Mac M칩 시리즈들은 ARM 기반이기 때문에 메모리보다 레지스터 활용이 높다. 또한 Apple은 이에 맞게 자체 OS도 ARM에 최적화 시키는 상황이다. 

그렇기에 Intel/AMD 컴퓨터보다 메모리 용량을 덜 구축하면서도 이용자들은 실제로 메모리 부족 현상을 같은 용량 대비 덜 겪게 된다.



