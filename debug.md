# CPP 디버그 팁

## 중단점 이용

1. 화살표 끌어오기 (중단점을 이동시킨다.)

2. ALT + **num 패드의 '*'**

3. 중단점 -> 마우스 왼쪽 클릭 - > 조건 입력
    변수가 해당 조건이 될 때 멈춘다. 

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
4. __noop; 
연산을 하지 않겠다고 명시적으로 알려줌

```cpp
//보류
#include <string>

void foo(int a, int b)
{
    __noop;
}

int main()
{
    int a{}, b{};


    while (true)
    {
        ++a;

        foo(a, 0);
    }
    return 0;
}

```