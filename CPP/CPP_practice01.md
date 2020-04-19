## 워밍업 0

```cpp
//0xff = 255 = 2^8 = 1byte
```





## 워밍업 1

```cpp
class Test
{
public:
   int a() { return 0; }
   int b() { return 1; }
   int c() { return 2; }
   char ch{};
};

int main()
{
   Test alpha;
   Test beta;
   Test* ptr_alpha{ &alpha };
   Test* ptr_beta{ &beta };
   int sizeTest{ sizeof(Test) };
   int(Test::*ptr_a)(){ &Test::a };
   int(Test::*ptr_b)(){ &Test::b };
   int(Test::*ptr_c)(){ &Test::c };
   return 0;
}
```



## 워밍업 2

```cpp
#include <iostream>

char _a;
short _b;
int _c;
float _d;
double _e;

class Guess
{
public:
	Guess(const char* id) { printf(id); }
};
Guess _guess{ "global" };

int main()
{
	char a;
	short b;
	int c;
	float d;
	double e;
	Guess guess{ "local" };
	return 0;
}
```





## 반복횟수, 이유

```cpp
int main()
{
	unsigned int limit = 256;

	for (unsigned char i = 0; i < limit; ++i)
	{
		//
	}

	return 0;
}
```



## 어디가 문제? 어떻게 고침?

```cpp
void foo(const char* src)
{
	int length = strlen(src);
	char* copy = new char[length];
	strncpy(copy, src, length);
	printf(copy);
	delete copy;
}
```



## 결과는?

```cpp
#include <iostream>

class A
{
};

class B
{
	char a;
};

int main()
{
	if (sizeof(A) == sizeof(B))
	{
		printf("same");
	}
	
	printf("different");

	return 0;
}

//class가 아무 내용이 없다고 해도 1byte는 차지한다.
//즉, char랑 '사이즈'는 같기 때문에 same이 나온다.
```



## 실행 결과는?

```cpp
#include <iostream>

class Base
{
	int a;
};

class Derived : Base
{

};

void foo(Derived a)
{
	printf("%d", a);
}

int main()
{
	Base a;
	Derived b;
	
	foo(a);
	foo(b);
}

//result 실행이되지 않는다. foo에서는 derived라는 자식 클레스를 받기 때문에 부모 클레스인 base는 받지 않는다.
//
```



## ※실행 결과는?

```cpp
#include <iostream>

void foo(int* a, int* b)
{
	a = b;
	*a = 3;
}

int main()
{
	int x = 1, y = 2;
	foo(&x, &y);
	printf("%d, %d", x, y);
	return 0;
}

//

//
```



## 실행 결과는? 이유는? ★

```cpp
#include <iostream>

class Base
{
public:
	int* a;
	Base()
	{
		a = new int;
		*a = 5;
		printf("%d\t", *a);
	}
	virtual ~Base()
	{
		printf("%d\t", *a * 2);
		delete a;
	}
};

class Derived : public Base
{
public:
	Derived()
	{
		a = new int;
		*a = 5;
		printf("%d\t", *a);
	}
	virtual ~Derived()
	{
		printf("%d\t", *a * 2);
		delete a;
	}
};

int main()
{
	Derived a;
	Base b = a;
	return 0;
    
    // 1) 5 5 10 10
    // 2) 5 10 5 10
    // 3) 5 5 10 에러
    // 4) 5 10 5 에러
}
```



## ＠실행 결과?

```cpp
#include <iostream>

class Base
{
public:
	virtual void print()
	{
		printf("Base class\n");
	}
};

class Derived : public Base
{
public:
	virtual void print()
	{
		printf("Derived class\n");
	}
};

int main()
{
	Base* a = new Base;
	Base* b = new Derived;

	a->print();
	b->print();

	delete a;
	delete b;

	return 0;
}
```



## 어떻게 될까?

```cpp
#include <iostream>

class Base
{
public:
	char x = 3;
};

class Derived : public Base
{
public:
	int y = 5;
};

int main()
{
	Derived a;
	Base b;

	Derived* pa{ &a };
	Base* pb{ &b };

    a = b;
	b = a;
	memcpy(&b, &a, sizeof(Derived));

	return 0;
}
```



## 정상? 컴파일 에러? 런타임 에러?

```cpp
#include <iostream>

class Test
{
public:
	void destroy()
	{
		delete this;
	}
};

int main()
{
	Test a;
	a.destroy();

	return 0;
}
```



## 실행 결과는?

```cpp
#include <iostream>

int main()
{
	float arr[5]{ 1.2f, 3.5f, 10.7f, -2.0f, 22.1f };
	float* ptr1 = &arr[0];
	float* ptr2 = ptr1 + 3;
	
	printf("%f\n", *ptr2);
	printf("%d\n", ptr2 - ptr1);

	return 0;
}
```



## 문제점은?

```cpp
#include <iostream>

class HeavyClass
{
public:
	// 멤버변수가 아주 많다
};

void Test(HeavyClass x)
{
	// 내용
}

int main()
{
	HeavyClass a;
	Test(a);

	return 0;
}
```



## 문제가 되는 부분?

```cpp
void foo()
{
    char test[]{ "def" };
	printf("abc");
    printf(test);
    printf("%s", test);
    
    int a = 7;
    printf(a);
    printf("%d", a);
    printf("%f", a);
}
```



## 실행 결과는? 이유는?

```cpp
int main()
{
	int a = 5 * 3 / 2;
	int b = 5 / 3 * 2;
	return 0;
}
```



## 포인터의 용도

1. 함수의 매개변수를 받을 때 복사 비용을 최소화하기 위해
2. 함수 내 지역변수를 다른 함수에 공유하기 위해
3. 리스트와 같은 특별한 자료형을 구현하기 위해
4. 전부 다



## 알고리즘 - Tree pre-order 순회

​            [11]

​    [2]               [3]

[9]   [4]       [7]

pre-order로 순회하며 출력하는 프로그램 작성. left나 right는 nullptr일 수 있음.

```cpp
struct node
{
	int value;
	node* left;
	node* right;
}
```



## 알고리즘 - 문자열 치환 (STL, 문자열 함수 사용 없이)

%20을 띄어쓰기로 치환

```cpp
char src[]{ "Test%20Guess%20This%20Please" }; // "Test Guess This Please"
void change(char* inout)
{
    
}
```



## 알고리즘 - n차 정방형

n차 정방형. 내용은 0또는 1만 있음. 특정 위치 (x, y)가 주어질 때 연속된 0을 전부 1로 만드는 코드를 작성하시오.



## 알고리즘 - 정렬된 배열 둘

두 배열에는 반복되지 않는 숫자들이 오름차순으로 정렬되어 있다. 두 배열의 크기는 항상 같을 때, 같은 숫자가 몇 번 나오는지 리턴하는 함수를 작성하라. 다만, 최적화에 신경을 써라.

```cpp
int arr1[5]{ 1, 2, 3, 4, 5 };
int arr2[5]{ 1, 3, 5, 7, 9 };

int compare(int a[], int b[])
{
	
}
```



## 알고리즘 - 문자열 비교

순서만 다른 문자열 "apple", "pplae", "aelpp" 등을 비교하여 같은지 확인하는 코드를 작성하라.



## 알고리즘 - 출석률

참가자 수 a, 총 학생 수 t가 주어질 때 출석률을 정수로 리턴하는 함수를 작성하라. 단, 함수 내에서 float이나 double 등을 사용하지 않고, 출석률의 소수부분은 버린다.

예) a = 0, t = 20 : 출석률 0

​      a = 4, t = 20 : 출석률 20

​      a = 10, t = 20: 출석률 50

```cpp
int count(int a, int t)
{

}
```

