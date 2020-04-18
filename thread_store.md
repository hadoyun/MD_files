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

```cpp

#include <thread>
#include <atomic>
#include <iostream>

//k - 상수 g - 전역 _ - 맴버 e - 이넘
std::atomic_int gValue{};

// void plus(int& gValue) --> 가리는 변수 (사용 X)
// 지역 변수는 쓰레드 동기화를 하지 않아도 된다.
// 동기화 (synchrolization) 시간을 일치 시키는 것 / 둘이 일치시키는 것 
// 쓰레드의 동기화 - 쓰레드가 보는 모든 메모리의 값들을 최신으로 맞춰준다...! 
// (최신이 될 때까지 기다린다...!)
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