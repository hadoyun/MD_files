# 스레드

## 스레드란?

###  프로세스는 스레드로 이루어져 있다!( == 스레드는 메모리에 저장된다.)

== 한 프로세스가 여러가지의 쓰레드를 가질 수 있다. (멀티 쓰레딩)

```cpp
#include <thread>

void plus(int in, int* out)
{
	for (int i = 0; i < 10000; ++i)
	{
		++in;
	}
	*out = in;
}

//메인도 쓰레드다!
int main()
{
	int a{}, b{}, c{};
	// 배열의 이름 == &변수 이름 == 주소!
	// 배열의 주소 == 배열의 이름 std::vector<int> a; a == vector a의 이름
	// 함수의 주소 == 함수의 이름 () 없이
	// plus(a, &b);

	// 쓰레드는 쓰레드가 생성되었을 때, 초기화 할때 실행된다.(RAII 패턴)
	// 아는 것부터 정확하게 설명해야함!
	// 서브 루틴 == 함수 내에서 호출되는 함수, 
	// 함수는 함수 마다, 쓰레드는 쓰레드 마다 ★"스택" 공간이 존재한다.
	std::thread thread_a{ plus, a, &b };

	plus(a, &c);
	// 본래의 프로세스에 join 한다는 의미
	// join은 쓰레드가 자신의 일을 끝낼 때까지 기다린다. 멀티 쓰레드를 했을 경우에는 join을 한꺼번에 몰아서 한다...!
	// 블로킹 함수 == 블로킹 함수가 리턴할 때까지, 이 함수를 호출한 쓰레드가 멈춘다..! ex) cin << (기본 입출력 함수)
	thread_a.join();

	return 0;
}
```




프로그램이 실행되면 프로세스가 된다.



즉, 기본으로 모든 프로그래밍은 싱글 스레딩 이상이다.



스레딩 == 일의 목록 !







#### 주 기억장치

 메모리(RAM = ramdon access memory) (지금 우리가 사용하는 대부분의 메모리) ( 메우 빠름)

(ex) gpu - v ram)

10만 번째 메모리를 한번에 접근 가능함.

```js
var array = { 1, 2, 3, 4, 5};

//array[4] == 5;
```



-실행된 프로그램 - > 프로세스! (메모리에서 적제된 프로그램) (메모리가 더 느리기 때문에)



#### 보조 기억장치

그 외의 기억장치들 (하드 디스크(물리적으로 새김), SSD(디지털로 새김), 플로피 디스크, CD... 포함) ( 엄청나게 느림)

-> 명령의 집함 - > 프로그램!





## 멀티 스레딩을 만드는 이유...?

''동시''의 다른 여러개의 일을 하고 싶기 때문에...!



### ''동시''의 두가지 개념

#### concurrency(concurrent의 명사형 : con(같이)+ current(물줄기) 동시성)

빠르게 교차(context swicting)하게하면서 하기 때문에 동시에 일어난 것 처럼 보인다.

context swicting도 명렁어이기 때문에 비용이 발생한다  == 꼭 빠른게 아님

### 시분할(time slicing) 프로그래밍





#### parallemlism(parallel 의 명사형 : 병행하다, (병행성))

**실제로는 병행되는 것이다.**



 ***cpu 하나 밖에 없었음  / 쿼드 코어 이상부터 ***







## 프로세스

### heap : 동적 할당된 변수가 있는 공간

먼저 선언된 것을 먼저 주소에 할당 시킨다.





### stack : 지역 변수가 있는 공간 

( 보통 지역성이 없는 프로세스가 저장된 메모리에서 가장 마지막 높은 공간( 보통 메모리는 숫자가 높을 수록 뒤로 가기 때문에)을 차지한다.)

자료 구조인 stack과 마찬가지로 먼저 들어온 것이 마지막에 해제 된다.



#### undefinded hehavior

정의되지 않은 행동 - 

####  race condition : 여러 스레드가 하나의 공간을 두고 접근할 때 발생할 수 있는 상황

#### (최소한 한번이라도 "임계구역"에 ''쓰기'' 연산을 한다면  undefinded hehavior 가 발생할 수 있다.) 



임계구역 - critical seccetion - 



Atomic - 

mutural exclusion- 상호 배제





### Dead Lock

![Synchronization 5: Deadlock and Server Programming – CS 61](https://cs61.seas.harvard.edu/site/img/deadlock-01.png)

Thread 1 은 Thread 2 가 가지고 있는 Resource 1 에 접근하려고함 - > 대기 상태 발생

Thread 2 는 Thread 1 이 가지고 있는 Resource 2 에 접근하려고함 - > 대기 상태 발생



이러한 상황을 Dead Lock 상태라고 한다.