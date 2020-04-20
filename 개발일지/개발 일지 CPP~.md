# 개발일지

## 200414 multy thread 만들기

CPP에서 람다 함수를 처음 사용...!
! 주의: JScript와 용법이 많이 달라 헷갈린다.

쓰레드를 만든 후에는 반드시 쓰레드의 메소드인 join() 함수를 호출한다.

```cpp
#include <thread> 
#include <iostream>
// #include <atomic>
#include <mutex>

//메인도 하나의 thread이다.
int main()
{
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
                // a에 접근
				++a;	
				mut.unlock();
			}
		}
	};

	std::thread thr_subtract
	{
		[&]()
		{
			for (int i = 0; i < 10000; ++i)
			{
				mut.lock();
                // 다른 쓰레드에서 사용된 a에 '쓰기' 연산으로 접근 
				--a;
				mut.unlock();
			}
		}
	};

    //join은 
	thr_add.join(); 
	thr_subtract.join();

	return 0;
}
```


## 200417 network - UDP 



```cpp



```