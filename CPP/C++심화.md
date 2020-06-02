# 메모리

## segment

data - 전역/static변수

code

stack - 지역변수(local variable == automatic variable), 함수 매개변수(parameter)

heap - 동적 할당된 변수

# 변수

## 전역/static 변수

### 메모리 공간

메모리 data 영역에 저장됨. 컴파일될 때 초기값이 같이 저장됨

### 초기화

```cpp
int a; // a의 값은 0으로 초기화된다
int* p; // p의 값은 nullptr로 초기화된다.

int main()
{
	static int sa; // 0
    static int* sp; // nullptr
	return 0;
}
```

## 포인터

### 포인터 변수의 ++ / -- 연산 (자료형의 크기만큼 주소가 이동된다★)

```cpp
int a{};
int* pa{ &a }; 	// pa가 저장하고 있는 주소는 0x00AABBC0
++pa; 			// pa가 저장하고 있는 주소는 0x00AABBC4

void* pany{ pa };
++pany; 		// 컴파일 안 됨!!
```

```cpp
void copy_post(char* dest, char* src)
{
	while (*src != '\0')
	{
		*dest++ = *src++; // 후위 증가 연산자(post-increment operator)
        
        // 위 코드는 아래와 같음
        //*dest = *src;
		//++dest;
		//++src;
	}
}
```

```cpp
void copy_pre(char* dest, char* src)
{
	while (*src != '\0')
	{
		*++dest = *++src; // 전위 증가 연산자(pre-increment operator)
        
        // 위 코드는 아래와 같음
        // ++dest;
		//++src;
		//*dest = *src;
	}
}
```

```cpp
#include <iostream>

using namespace std;

int main()
{
	int a{ 13 };
	int b{ 17 };
	const int* x{ &a };
	int* const y{ &a };
	const int* const z{ &a };

	x = &b; // x는 const int에 대한 그냥 포인터니까, 참조는 바꿀 수 있다.
	y = &b; // y는 int에 대한 const 포인터니까, 참조를 바꿀 수 없다.

	(*x)++; // x는 const int에 대한 포인터니까, 역참조 값을 바꿀 수 없다.
	(*y)++; // y는 int에 대한 포인터니까, 역참조 값을 바꿀 수 있다.

	(*z)++; // z는 const int에 대한 const 포인터니까, 참조도 역참조도 바꿀 수 없다.
	z = &b;

	return 0;
}
```



# 함수

## 호출 순서

```cpp
bool ReturnTrue()
{
	cout << "true\n";
	return true;
}

bool ReturnFalse()
{
	cout << "false\n";
	return false;
}

int main()
{
	if (ReturnTrue() ^ ReturnFalse())
		cout << "Case0\n";

	if (ReturnFalse() ^ ReturnTrue())
		cout << "Case1\n";

	if (ReturnTrue() ^ ReturnTrue())
		cout << "Case2\n";

	return 0;
}
```



# struct

```cpp
struct SAny
{
	char a{};
	char b{};
	int c{};
	bool d{};
};
```



# union

```cpp
union U
{
	unsigned char a;
	unsigned short b;
	unsigned int c;
};

int main()
{
	U x;
	memset(&x, 256, sizeof(x));
	return 0;
}
```

# class

```cpp
// F11 누르면서 따라가보기!!!

class A
{
public:
	A() 
	{
		cout << "A constructor\n"; 
	}
	virtual ~A()
	{
		cout << "A destructor\n"; 
	}
};

class B : public A
{
public:
	B()
	{ 
		cout << "B constructor\n"; 
	}
	virtual ~B() 
	{
		cout << "B destructor\n"; 
	}
};

int main()
{
	A x{};

	A* y{ new B() }; // B의 생성자가 호출되면 거기서 A의 생성자가 호출되어서 먼저 A의 생성자가 실행되고, 그 후 B의 생성자로 돌아와서 B의 생성자가 실행된다!
	delete y;

	return 0;
}
```



# 연산자

비트 연산자 (bitwise operator: & | ^ ★~)

```cpp
int main()
{
	unsigned char a{ 0b11011011 };
	unsigned char b{ 0b10101010 };
	unsigned char r{};

	r = a & b;

	r = a | b;

	r = a ^ b;

	r = ~a;
	
	r = ~b;

	return 0;
}
```

비트 시프트 연산자 (<< , >>)

```cpp
int main()
{
	unsigned char a{ 0b11011011 }; // 앞의 1101
	unsigned char b{ 0b01101010 }; // 뒤의 1010
	unsigned char r{};

	a = a >> 4;
	b = b << 4; // b = (b << 4) >> 4; => 주석처럼 하면 왼쪽 비트들이 손실되지 않음! 즉, 그냥 원래 b랑 같음.
	b = b >> 4;

	return 0;
}
```

논리 연산자 (&& || ^ !)




```cpp
#include <iostream>
#include <vector>

//TODO 동적 메모리의 심화적인 활용

struct book
{
	int number{};
	const char* title{};
};

void showBook(book* book, int n)
{
	printf(" %d의 항목에 %s 책이 있습니다.\n", book->number, book->title);
	
	if (n > 1)
	{
		showBook((book + 1), n - 1);
	}
}

// //혹은
// void showBook(book* book, int n)
// {
// 	for(int i = 0; i < n; ++i>)
// 	{
// 		printf("번호 %d  : %s \n", (book+i)->number, (book+i)->title);
// 	}
// }

int main()
{
	book *bk{};

	bk = (book*)malloc(2 * sizeof(book));

	if (bk == NULL)
	{
		printf("동적 메모리 할당에 실패했습니다. \n");
		exit(1);
	}
	
	bk->number = 1;
	bk->title = "그 많던 싱아를 누가 다 먹었나?";

	(bk + 1)->number = 2;
	(bk + 1)->title = "자료 구조";

	showBook(bk, 2);

	free(bk);

	book* ptr = (bk + 1);

	return 0;
}
```


```cpp
#include <iostream>
#include <vector>

//TODO 동적 메모리의 심화적인 활용

struct book
{
	int number{};
	char title[100]{};
	//const char* 형이 훨씬 더 편리하게 사용할 수 있다.
};

void showBook(book* book, int n)
{
	printf(" %d의 항목에 %s 책이 있습니다.\n", book->number, book->title);
	
	if (n > 1)
	{
		showBook((book + 1), n - 1);
	}
}

int main()
{
	book *bk{};

	bk = (book*)malloc(2 * sizeof(book));

	if (bk == NULL)
	{
		printf("동적 메모리 할당에 실패했습니다. \n");
		exit(1);
	}
	
	bk->number = 1;
	strcpy_s(bk->title, "노인과 바다");

	(bk + 1)->number = 2;
	strcpy_s((bk + 1)->title, "리버 보이");

	showBook(bk, 2);

	free(bk);

	return 0;
}
```

```cpp
#include <iostream>
#include <vector>

//TODO 동적 메모리의 심화적인 활용

int main()
{

	int** pptr = (int**)malloc(sizeof(int*) * 8);

	// 6개의 공간을 다시 8번 생성시켜준다.
	for (int i = 0; i < 8; ++i)
	{
		*(pptr + i) = (int*)malloc(sizeof(int) * 6);
	}

	int z{};

	//pptr의 각 항목 초기화
	for (int y = 0; y < 8; ++y)
	{
		for (int x = 0; x < 6; ++x)
		{
			*(*(pptr + y) + x) = 6 * y + x;
		}
	}

	for (int y = 0; y < 8; ++y)
	{
		for (int x = 0; x < 6; ++x)
		{
			printf("%3d", *(*(pptr + y) + x));
		}
		printf("\n");
	}
	
	for (int y = 0; y < 8; ++y)
	{
		free(*(pptr + y));	
	}
	//// 반면 cpp vector 혹은 배열을 사용할경우

	return 0;
}
```