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


## file 입출력 (binary)

되도록이면 의미가 같은 함수를 묶어주자...!

***2진, 즉, binary로 파일을 저장해보자..!***

```cpp
#pragma once
#include <vector>

using byte = unsigned char;
using int32 = int32_t;
using uint32 = uint32_t;

class BinaryFile
{
public:
	BinaryFile();
	~BinaryFile();
public:
	void load(const char* fileName); //파일을 저장하고 로드한다.
	void save(const char* fileName);
public:
	void clear(); // _vData를 벡터의 clear 메소드를 통해 초기화 한다. { _vData.clear(); }
	void write(int32 value);
	void write(char value);

public:
	bool canRead(uint32 count) const; // 읽을 수 있는지 없는지 확인해주는 함수
	
	int32 readInt32() const; 
	char readChar() const;

private:
	//파일의 데이터를 byte 단위로 넣어서 보관할 벡터
	std::vector<byte> _vData{};
	//const 함수 여도 바꿀 수 있도록...! 의미상 const 는 맞는 함수를 const해주기 위해서 
	mutable uint32 _at{};
};

```


```cpp
void BinaryFile::load(const char* fileName)
{
	std::ifstream ifs{};

	//ifs.open 메소드의 2번째 인수 == std::ios_base는 in 나 binary등의 객체를 | 연산으로 같이 적용하는 것이 가능하다.
	ifs.open(fileName, std::ios_base::in | std::ios_base::binary);

	if (ifs.is_open() == true)
	{
		std::cout << "파일이 열렸어양! \n";

		while (ifs.eof() == false)
		{
			_vData.emplace_back(ifs.get());
		}
		// 디버그 확인용, vector<byte>_vData에 byte가 들어가는지 확인한다.
		// DebugBreak(); 
	}
	else
	{
		std::cout << "파일이 여는데 실패했어양! \n";
	}
}


```

***알고리즘!***


```cpp
// O자형 블록
	{
		_blocks[(int)EBlockType::O][(int)EDirection::N].set(
			1, 1, 0, 0,
			1, 1, o, 0,
			0, 0, 0, 0,
			0, 0, 0, 0
		);

		//모두 바꾸기 '단어 단위' alt + A == 모두 바꾸기 
		_blocks[(int)EBlockType::O][(int)EDirection::W].set();
		_blocks[(int)EBlockType::O][(int)EDirection::S].set();
		_blocks[(int)EBlockType::O][(int)EDirection::E].set();
	}

```

```cpp
	// grey scale 전부다 무채색..! 
	// RGB가 서로 다를 수록 채도가 높아진다...!

	createBlock(EBlockType::I,		Color(255, 100, 100));
	createBlock(EBlockType::T,		Color(100, 240, 130));
	createBlock(EBlockType::O,		Color(100, 100, 255));
	createBlock(EBlockType::L,		Color(250, 250, 100));
	createBlock(EBlockType::InvL,	Color(100, 250, 250));
	createBlock(EBlockType::Z,		Color(240, 160,  50));
	createBlock(EBlockType::InvZ,	Color(255, 127, 255));
```



**const 함수에서 값을 바꿀 수 있게 해주는 mutable!**

```cpp
bool fs::SimpleTetris::tickTimer() const
{
	using namespace std::chrono;

	auto elapsed = duration_cast<milliseconds>(steady_clock::now() - _prevTime);

	if (elapsed.count() >= _timerInterval)
	{
		_prevTime = steady_clock::now();

		return true;
	}
	return false;
}
/// in class 
public:
mutable std::chrono::steady_clock::time_point _prevTime{};
```


## 되도록 서순으로 조건문 작성하기 


```cpp
oid fs::SimpleTetris::checkBingo()
{
	int32 currentY{ (int32)kBoardSize.y - 1 };
	
	uint32 bingoCount{};

	while (currentY >= 0)
	{
		bool isBingo{ true }; // 기본 값이 false
		for (uint32 x = 0; x < (uint32)kBoardSize.x; ++x)
		{
			if (_aaBoard[currentY][x] == 0)
			{
				isBingo = false; // 서순
				break;
			}
		}
		if (isBingo == true)
		{
			for (int32 y = currentY - 1; y >= 0; --y) // 확신이 들지 않는다면 unsigned를 쓰면 안된다.
			{
				memcpy(_aaBoard[y + 1], _aaBoard[y], (size_t)kBoardSize.x);
			}
			++bingoCount;
		}
		else
		{
			--currentY;
		}
	}
	_score += (bingoCount * bingoCount) * 100;
}


```





## 별명을 모아 두는 header!

```cpp
#pragma once
#include <cstdint>

namespace hady 
{
	using int8 = int8_t;
	using int32 = int32_t;
	using uint8 = uint8_t;
	using uint32 = uint32_t;
	using uint64 = uint64_t;
}
```


# 타이머 클래스 설정하기

timer 란? 정해놓은 시각에 따라 신호(혹은 동작)(을)를 발생시키는 장치 

timer가 해야할일 

1. 타이머를 세팅(시작)한다.

2. 일정 시간 이후에 알림(bool)


```cpp
using uint64 = uint64_t;
using uint32 = uint32_t;
//tick 함수 , elapsed == 현재 시간 - 시작시간
bool CheapTimer::tick() const
{
	uint64 elapsed = std::chrono::steady_clock::now().time_since_epoch().count() - _startTime;

	switch (_unit)
	{
	case hady::CheapTimer::EUnit::second:
		elapsed /= 1'000'000'000;
		break;
	case hady::CheapTimer::EUnit::milli:
		elapsed /= 1'000'000;
		break;
	case hady::CheapTimer::EUnit::micro:
		elapsed /= 1'000;
		break;
	case hady::CheapTimer::EUnit::nano:
		//elapsed *= 1;
		__noop;
		break;
	default:
		break;
	}

	if (elapsed >= _interval) return true;
	return false;
}
```





```cpp
#pragma once
#include "Common.h"
#include <chrono>

//몇 초에 한번 지났는지 알려주는 아이
namespace hady 
{
	
	class CheapTimer
	{
	public:
	//구조체를 class 내부에서 사용하기 위해서는??
		enum class EUnit
		{
			second,
			milli,
			micro,
			nano,
		};

	public:
		CheapTimer();
		~CheapTimer();

	public:
		void set(uint32 interval, EUnit unit)
		{
			_unit = unit;

			_interval = interval;
		}
		void reset();
		
		void start()
		{
			_startTime = std::chrono::steady_clock::now().time_since_epoch().count();
		}

		bool tick() const;

	private:
		uint32			_interval{}; //
		uint64			_startTime{};
		EUnit			_unit{ EUnit::second };
	};
}
```

## toggle

```cpp
bool isPause() const
{
	if (_pauseFlag == 0)
	{
		_pause = true;
		++_pauseFlag;
	}
	else if (_pauseFlag == 1)
	{
		_pause = false;
		--_pauseFlag;
	}
};
```

```cpp
bool isPause() const
{
	int count{};

	if (_pauseCount%2 == 1)
	{
		_pause = true;
		++_pauseCount;
	}
	else if (_pauseCount%2 == 0)
	{
		_pause = false;
		++_pauseCount;
	}
};
```


**함수를 이해하면 코드가 짧아진다..!**

```cpp
bool isPause() const
{
	if (_pause == true)
	{
		_pause = false;
		return true;
	}
	else (_pause == false)
	{
		_pause = true;
		return false;
	}
};
```




**정적 할당**
실제 할당은 런타임에서 이루어지만, 얼마만큼의 메모리를 사용할지 미리 알고 있는 경우에 사용한다.

```cpp
int a {};


```

**동적 할당**

new 연산자는 '할당된 메모리의 주소'를 리턴하기 때문에 생성된 메모리의 주소를 받기 위해서는 
자료형을 포인터로 사용해야한다.

'동적으로 할당된 메모리'를 해제하기 위해서 delete를 사용해야한다.
```cpp
int* a = new int;

delete a;
a = nullptr;
```





```cpp
bool a = false;
bool b = true;

bool ab { a != a };

a = !b;
```









# 디버깅 !!

## VS bebugind tips

1. 초록색 화살표 이용하기, (생략할 수 있다.)

2. 디버깅 시 변수 위에 마우스를 올리면 뜨는 옆에 고정를 누르면 변수 변화를 실시간으로 볼 수 있다.
(특히 배열 일경우에는 각 항목에 들어있는 변수들을 확인 가능하다.)
![](Screenshot 2020-04-21 at 23.17.12.png)
![](Screenshot 2020-04-21 at 23.22.03)

3. 




### 솔루션 디버깅

한 프로젝트가 다른 프로젝트를 참조하거나 간접적으로 사용할때, 
shift + ctrl + B 를 눌러서 '솔루션' 빌드 해야한다. (아니면 오류떠용!)


디버깅 용으로 솔루션을 빌드 했을 때만, 특정 함수가 실행되게 하고 싶을 때

```cpp
#if defined DEBUG || _DEBUG
if (GetAsyncKeyState('W') == SHORT(0x8001))
{
	auto timerInterval{ g_simpleTetris.getTimerInterval() };

	g_simpleTetris.setTimerInterval(timerInterval - 50);
}
if (GetAsyncKeyState('Q') == SHORT(0x8001))
{
	auto currBlockType = g_simpleTetris.getCurrBlockType();
	uint32 iNextBlockType{ (uint32)currBlockType + 1 };
	if (iNextBlockType >= (uint32)EBlockType::MAX)
	{
		iNextBlockType = 2;
	}
	g_simpleTetris.setCurrBlockType((EBlockType)iNextBlockType);
}
#endif 

```

# class
class를 빠르게 만드는 방법 

1. 하는 일!로 분리 한다.

2. 맴버 변수는 급하게 만들지 않아도 좋다...!
