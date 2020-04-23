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

UDP를 만들어 보자!

### server 서버를 만드는 순서
1. WSA 시작 
2. 소켓 만들기 
3. 소켓을 묶기 (bind) (서버 이기 때문에 만든다.)
~ 소켓을 만들기 완료 
4. (서버 이기 때문에) 
5. 


```cpp
#include <WinSock2.h>
//윈소켓 소켓 시작하기
WSADATA wsa_data{};
int error{ WSAStartup(MAKEWORD(2, 2), &wsa_data) };

//윈소켓 끝내기
WSACleanup() 

// 윈소켓 만들기 socket() -> 오류가 일어나지 않을 시(일어난다면 오류코드 리턴), 
// AF_INET == The Internet Protocol version 4 (IPv4) address family.
// SOCK_DRRAM == date gramm
// UPPROTO_UDP == UDP 프로토콜 사용 
m_server_socket = socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP);
m_server_socket == INVALID_SOCKET

// no error return 0 , otherwise return SOCKET_ERROR
int socket_error = closesocket(m_server_socket);

// get window socket error of last thread.
WSAGetLastError();

// bindSocket only be used in server
// bind return error code
sockaddr_int addr;

// &addr 
bind(m_server_socket, (sockaddr*)&addr, sizeof(addr))

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

};

//s_test* 라는 구조체를 참조형으로 받는다. (== s_test* 를 참조한다.)
void test_sort(s_test*& my_struct)
{
	// s_test의 맴버 변수들을 백터로 만들어보자.
	// 포인터 자료형으로 만들어진 구조체에 접근 할시에는 -> 를 해준다.
	std::vector<int> v_index{ my_struct->a, my_struct->b, my_struct->c };

	sort(v_index.begin(), v_index.end());

	my_struct->a = v_index[0];
	my_struct->b = v_index[1];
	my_struct->c = v_index[2];
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

	test_sort(*&v_test[0]);

	printf("\n \n after sort: \n \n");

	std::cout << v_test[0]->a << ' ';
	std::cout << v_test[0]->b << ' ';
	std::cout << v_test[0]->c << ' ';
	
	delete v_test[0];

	return 0;
}

```

## 200423




## VS bebugind tips

1. 초록색 화살표 이용하기, (생략할 수 있다.)

2. 디버깅 시 변수 위에 마우스를 올리면 뜨는 옆에 고정를 누르면 변수 변화를 실시간으로 볼 수 있다.
(특히 배열 일경우에는 각 항목에 들어있는 변수들을 확인 가능하다.)
![](Screenshot 2020-04-21 at 23.17.12.png)
![](Screenshot 2020-04-21 at 23.22.03)

3. 



