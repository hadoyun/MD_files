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







## 주 기억장치

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



## undefinded hehavior

정의되지 않은 행동 - 

### 임계구역 - critical seccetion - 

*** race condition : 여러 스레드가 하나의 공간을 두고 접근할 때 발생할 수 있는 상황***

***(최소한 한번이라도 "임계구역"에 ''쓰기'' 연산을 한다면  undefinded hehavior 가 발생할 수 있다.) ***


#### Atomic - 

```cpp
//아토믹을 선언하는 방법
#include <atomic>

// a는 2개 이상의 쓰레드가 동시에 쓰기연산으로 접근하는 변수이다.
std::stomic<int> a{};
```

#### mutural exclusion- 상호 배제

```cpp
//mutex! 의 구현
#pragma once
#include <thread>
#include <chrono>

class CheapMutex
{
public:
	CheapMutex() {};
	~CheapMutex() {};

public:
	void lock()
	{
		// 두개 이상의, 쓰레드에서 하나의, 변수에 쓰기, 연산을 실행하려고 할때, 
		// race condition을 방지하기 위해서 사용한다.

		// 하나의 스레드가 lock 함수를 호출 했을때, 그 경우 lock을 true로 바꿔준다.
		// 동기화 == 동시에 실행되는 여러 쓰레드를 하나의 시간대로 동기화 시킨다.

		// bool a = false;
		// "a가 false라고 치면"(X) "a는 false 이다"....!
		while (_bLocked == true)
		{
			//std chrono의 생성자 함수에 접근
			std::this_thread::sleep_for(std::chrono::microseconds(10));

			if (_bLocked == false)
			{
				break;
			}
		}

		_bLocked = true;
	}

	void unlock()
	{
		if (_bLocked == true)
		{
			_bLocked = false;
		}
	}

private:
	// 부울 변수를 말할때는 그 용도를 말해야한다.
	bool _bLocked{ false };
};
```


```cpp

#include <thread>
#include <atomic>
#include <iostream>

//k - 상수 g - 전역 _ - 맴버 e - 이넘
std::atomic_int gValue{}; // (== std::atomic<int> gValue{};)

// void plus(int& gValue) --> 가리는 변수 (사용 X)

// 지역 변수는 쓰레드 동기화를 하지 않아도 된다. 지역 변수는 해당 지역을 벗어나면 사라짐으로, 동시에 두개의 쓰레드가 접근이 불가능하다.


// 동기화 (synchrolization) 시간을 일치 시키는 것 / 둘이 일치시키는 것 
// 쓰레드의 동기화 - 쓰레드가 보는 모든 메모리의 값들을 최신으로 맞춰준다...! (최신이 될 때까지 기다린다...!)

// atominc / mutex(lock) 모두 동기화에 속한다.

// 복수 코어일 때, CASHE COHERERENCY - 코어의 캐쉬마다 각각 다른 값을 가지고 있을 수 있기 때문에 캐쉬의 값을 동기화 해준다.
void plus()
{
	int value{};

	for (int i = 0; i < 10000; ++i)
	{
		value += i;
	}

	gValue += value;
}

int main()
{
	//쓰레드는 참조형을 못받을지도 모른다...?
	std::thread threadA{ plus };

	plus();

	threadA.join();

	printf("%d\n", gValue.load());


	return 0;
}
```


```cpp
#include <thread>
#include <iostream>
#include <atomic>
#include <mutex>
//mutual exclusion - 상호 배제 // 잠그고 해제 하는 것을 해줘야함...!

int main()
{
	// 임계 구역 (critical section - 중요한 부분)
	// std::atomic<int> a{}; //한번 연산을 끝날 때까지는 다른 쓰레드가 접근하지 못하게 함.
	// 다른 함수가 못건듦 (보호된 임계구역)
	int a{}; 

	int b{ 77 };

	std::mutex mut{};

	std::thread thr_add
	{
		[&]()
		{
			for (int i = 0; i < 10000; ++i)
			{
				mut.lock();
				++a;	
				mut.unlock();
			}
		}
		//이것 자체가 하나의 명령어! 
		//thread를 초기화
	};

	std::thread thr_subtract
	{
		[&]()
		{
			for (int i = 0; i < 10000; ++i)
			{
				mut.lock();
				--a;
				mut.unlock();
			}
		}
	};

	// 이 스레드가 끝날 때 까지 기다리겠다.
	// 메인 스레드로 다시 와라 기다려라
	thr_add.join(); 
	thr_subtract.join();

	std::cout << a;
	//undefinded behavior 


	//이름 대신에 []를 사용한다.
	/*[]()
	{

	};*/

	return 0;
}
```

5. shift + F12




### Dead Lock

![Synchronization 5: Deadlock and Server Programming – CS 61](https://cs61.seas.harvard.edu/site/img/deadlock-01.png)

Thread 1 은 Thread 2 가 가지고 있는 Resource 1 에 접근하려고함 - > 대기 상태 발생

Thread 2 는 Thread 1 이 가지고 있는 Resource 2 에 접근하려고함 - > 대기 상태 발생



이러한 상황을 Dead Lock 상태라고 한다.