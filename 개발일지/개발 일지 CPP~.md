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


## 200420 CPP

	int 자료형 5개로 만들어진 구조체를 다뤄보는 연습

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

//int 자료형 5개를 포함한 자료형
struct s_test
{
	int a;
	int b;
	int c;
	int d;
	int e;
};

//s_test* 라는 구조체를 참조형으로 받는다. (== s_test* 를 참조한다.)
void test_sort(s_test*& my_struct)
{
	// s_test의 맴버 변수들을 백터로 만들어보자.
	// 포인터 자료형으로 만들어진 구조체에 접근 할시에는 -> 를 해준다.
	std::vector<int> v_index{ my_struct->a, my_struct->b, my_struct->c, my_struct->d, my_struct->e };

	//설계실패 vector에 int를 5개를 넣었음에도 불구하고 sort를 사용하지 않고 반복문으로 sort를 시도했다.;

	//int temp{};

	//a 와 b 를 sort 해보기
	//if (form == false)
	//{
	//	for (int i = 0; i < (int)v_index.size() - 1; ++i)
	//	{
	//		if (i > i + 1)
	//		{
	//			temp = v_index[i];
	//			v_index[i] = v_index[(size_t)(i + 1)];
	//			v_index[(size_t)(i + 1)] = temp;
	//		}
	//	}
	//	//대입
	//	my_struct.a = v_index[0];
	//	my_struct.b = v_index[1];
	//	my_struct.c = v_index[2];
	//	my_struct.d = v_index[3];
	//	my_struct.e = v_index[4];
	//}
	//else
	//{
	//	for (int i = 0; i < (int)v_index.size() - 1; ++i)
	//	{
	//		if (i < i + 1)
	//		{
	//			temp = v_index[i];
	//			v_index[i] = v_index[(size_t)(i + 1)];
	//			v_index[(size_t)(i + 1)] = temp;
	//		}
	//	}
	//	//대입
	//	my_struct.a = v_index[0];
	//	my_struct.b = v_index[1];
	//	my_struct.c = v_index[2];
	//	my_struct.d = v_index[3];
	//	my_struct.e = v_index[4];
	//}

	sort(v_index.begin(), v_index.end());

	my_struct->a = v_index[0];
	my_struct->b = v_index[1];
	my_struct->c = v_index[2];
	my_struct->d = v_index[3];
	my_struct->e = v_index[4];
}

int main()
{
	//구조체의 주소를 담는 v_test 벡터 생성
	std::vector<s_test*> v_test{};

	//v_test에 새로운 s_test를 동적할당
	v_test.push_back(new s_test{ 5, 2, 1, 3, 4 });
	
	std::cout << v_test[0]->a << ' ';
	std::cout << v_test[0]->b << ' ';
	std::cout << v_test[0]->c << ' ';
	std::cout << v_test[0]->d << ' ';
	std::cout << v_test[0]->e << ' ';

	
	test_sort(*&v_test[0]);

	printf("\n \n after sort: \n \n");

	std::cout << v_test[0]->a << ' ';
	std::cout << v_test[0]->b << ' ';
	std::cout << v_test[0]->c << ' ';
	std::cout << v_test[0]->d << ' ';
	std::cout << v_test[0]->e << ' ';
	
	delete v_test[0];

	return 0;
}

```