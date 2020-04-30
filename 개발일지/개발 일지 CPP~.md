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





## 200425



## 200430


```cpp
class TextFile
{
private:
	// const int _KbufferSize{ 300 };
	// Good !!
	static const int _KbufferSize{ 300 };
	char _fileName[_KbufferSize]{};
	std::string _data{};
};
```

***비정적 맴버는 특정 개체에 상대적이야 합니다. ***

클래스는 생성자가 호출됬을때 맴버 변수 / 맴버 함수 순으로 생성되기 때문에 
char _fileName[_KbufferSize]{}; 와 같이 정적인 배열을 생성 하거나, 혹은 전역 변수가 필요할 경우
**static** 키워드를 사용해서 전역변수처럼 만들어 준다.





```cpp
//파일 이름을 받아 텍스트를 읽어오는 함수 
void TextFile::openText(const char* _fileName)
{
	std::ifstream ifs{};

	ifs.open(_fileName, std::ios_base::in);
	

	//RAII - 범위를 벗어나면 자동종료 but 한번더 open 해주면 열어줘야함.
	if (ifs.is_open())
	{
		_data.clear();

		//한글자씩 읽어옴
		//_ifs.get();
		
		char buffer[_KbufferSize]{};

		while (!ifs.eof())
		{	
			//배열이 포인터가 되는순간 퇴화(decay)한다.. ㅠ; (배열의 길이에 대한 정보가 사라진다...!)
			//따라서 포인터로 받을때는 사이즈를 알려줘야한다.
			ifs.getline(buffer, _KbufferSize);
			
			//" " buffer "\n" ~
			_data += " ";
			_data += buffer;
			_data += "\n";
		}

	}
}


void TextFile::display()
{
	std::cout << _data << "\n";
}
```

1. std::ifstream 의 이해, ifstraam 함수는 소멸자를 호출 했을때, std::ifstream::close() 함수를 호출함.
따라서 따로 std::ifstream의 close()를 호출할 필요가 없다.

2. 배열이 포인터가 되는순간 퇴화(decay)한다.. ㅠ; (배열의 길이에 대한 정보가 사라진다...!) 
따라서 포인터로 받을때는 사이즈를 알려줘야한다.

3. while 문 사용 하기, while문은 ()안이 트루가 될때 반복한다. (보충 설명 듣기 - 까먹음 )

4. 함수를 너무 자세히 쪼개지는 말자... ㅜㅜ; 




## VS bebugind tips

1. 초록색 화살표 이용하기, (생략할 수 있다.)

2. 디버깅 시 변수 위에 마우스를 올리면 뜨는 옆에 고정를 누르면 변수 변화를 실시간으로 볼 수 있다.
(특히 배열 일경우에는 각 항목에 들어있는 변수들을 확인 가능하다.)
![](Screenshot 2020-04-21 at 23.17.12.png)
![](Screenshot 2020-04-21 at 23.22.03)

3. 



