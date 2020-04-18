# C++ 공부

## 진법



| 10진법 | 2진법 |
| ------ | ----- |
| 0      | 0     |
| 1      | 1     |
| 2      | 10    |
| 3      | 11    |
| 4      | 100   |
| 5      | 101   |
| 6      | 110   |
| 7      | 111   |
| 8      | 1000  |
| 9      | 1001  |
| 10     | 1010  |



## 단위

bit = 0 또는 1만 저장할 수 있는 (기계에서) 가장 작은 단위

**★byte★ = 8 bit (자료형의 최소 단위!)**

-  bit가 8개 모여 총 2^8가지의 수를 저장 가능. 메모리에서는 실질적인 최소 단위

kilobyte (KB) = **2^10 byte =1024 byte** 

megabyte (MB) = 2^10 KB = 1024 KB

gigabyte (GB) = 2^10 MB = 1024 MB

terabyte (TB) = 2^10 GB = 1024 GB



### 변수(variable★★) - 값을 저장하는 친구

### 자료형 (data type) = 데이터의 유형

#### 빌트인 타입(builtin types)

```cpp
char letter = 'a'
char letter{'a'}; // -127~128(1byte == 8bit == 2^8)까지 저장 가능 

char my_char[]{ "가" };
wchar_t my_wchar[]{ L"가" }; // ★2byte 유니코드!

short short_integer{300}; // -32767~32768(2byte)
int longer_integer{60000}; // -2147483647 ~ 2147483648(4byte)
long long much_longer_integer{4000000000}; // 8 byte
float point{0.1f}; // 4 byte
double double_point{123456789.123456}; // 8 byte
unsigned int integer{100}; // 부호 없는 int (== 정수)
```

#### 내가 선언한 타입
```cpp

struct SInt
{
    int a{};
    int b{};
};

int main()
{
    SInt a;   
}

```


## 연산자

### 대입 연산자

```cpp
 a = b;

b의 값을 a에 대입한다.
```

### 부정 연산자

```cpp
bool a{ true };

// a의 반대값(false)를 a에 대입한다.
a = !a;
```



## 초기화

```cpp
int a{ 5 };
//초기화는 변수 선언시에만 가능하다.
//a{ 1 };

int b;
//초기화는 변수 선언시에만 가능하다.
//b{ 3 };

//초기화와 대입은 같이하지 못한다. {] 다음에 =를 쓰지 못한다.
//int c{ 4 } = 1;
```


## 지역변수와 전역변수

### life time(생성~소멸) 



```cpp
void foo()
{
    //다음 줄에서 a가 생성된다.
    int a{};
    a = 10;
    
}// foo가 끝나면 a가 소멸된다.
```

### 지역 변수를 전역 변수로 바꿔보자 (static)




```cpp
#include "stack.h"

int g_a{};

void foo()
{
	static int a{};

	++g_a;
	++a;

}

int main()
{
	foo();
	foo();
	foo();
    
    //a와 g_a의 life time은 같다.
    
	// 다음줄은 실행이 되지 않는다. 
    // a의 scope가 foo 이기 때문이다.
	int b = a;
    // 다음줄은 정상적으로 실행이 된다.
    // g_a가 전역변수이기 때문이다.
	int c = g_a;


	return 0;
}
```



## class / 구조체(struct)

```cpp
struct my_struct
{
    int     value1{}; // 4
    float   value2{}; // 4
    double  value3{}; // 8
}

void foo()
{
    //★모든 변수는 사용하기 위해서 '이름(identify)'을 붙여야한다.
    //구조체 이름에 .을 눌러봐야 아무일도 일어나지 않는다.
    //main 에서 사용하기 위해서는 구조체의 이름에 .을 찍어야한다.
    // -> my_struct.
    
    my_struct named{};
    named.value1 = 10;
    named.value2 = 10.0f;
    named.value3 = 0.123;
}
```



###  class의 상속

```cpp
#include <string>

enum class e_gender
{ 
    남자,
    여자, 
};

//상속을 하는 클래스를 부모 클래스(perant class라고 한다.)
class person
{
public:
	person(string name, int age, e_gender gender) 
        : m_name{ name }, m_age{ age }, m_gender{ gender } {};
	~person() {};
    
protected:
	std::string m_name{};
	int m_age{};
	e_gender m_gender{e_gender::남자}; // e_gender == 0;
};

//상속을 받는 클래스를 '자식 클래스'(child class)라고 부른다.
class student : public person
{
public:
	student(string name, int age, e_gender gender, string school, int grade) : 	
	//자식 클래스에서 입력을 받아 부모 클래스의 멤버 변수의 값을 변경하고 싶다면, 
	//'부모 클래스 이름(변수, 변수)'로 선언한다.
	person(name, age, gender), m_school{ school }, m_grade{ grade } {};

	~student() {};

private:
	std::string m_school{};
	int m_grade{};
};
```



```cpp
// class는 '양식', '틀'이다.
// 우리가 다뤄야할 것은 클래스의 '사본'이다.
class avatar_system
{
public:
	avatar_system() {};
	~avatar_system() {};



private:
	avatar_base m_avatar{};
	
    /*m_avatar. X -> ★ 변수의 변경은 함수의 바디(body)에서만 한다.*/

};

int main()
{
    // my_avatar가 '사본' 즉, instance이다.
    avatar_system my_avatar{};
    
    
    return 0;
}

```




## 배열(array)

```cpp
// 배열을 사용하지 않았을 경우
float score_korean{};
float score_math{};
float score_english{};

score_korean = 100.0f;
score_math = 97.1f;
score_english = 32.1f;

float CalculateAverage(float score_korean, float score_math, float score_english)
{
    return (score_korean + score_math + score_english) / 3.0f;
}
```

```cpp
// 배열을 사용했을 경우
float scores[3];// [3]은 배열의 크기(size)를 의미하고, 배열의 크기는 내가 사용할 수 있는 항목의 갯수를 의미한다.
// scores는 &scores[0](즉, 배열의 주소는 배열의 0번 항목이다.)과 의미가 같다.

scores[0] = 100.0f; // 배열의 0번 '항목(entry)'에 100.0f를 대입
socres[1] = 97.1f;  // 배열의 1번 '항목(entry)'에 97.1f를 대입
scores[2] = 32.1f;  // 배열의 2번 '항목(entry)'에 32.1f를 대입

float CalculateAverage(float* ptr_scores)
{
    return (ptr_scores[0] + ptr_scores[1] + ptr_scores[2]) / 3.0f;
}
```



### 오버 플로우(overflow), 언더 플로우(underflow)

```cpp
int a[5]{};

for(int i = 0; i < 10; ++i)
{
    // !! overflow 발생 (a[5]~a[9]는 접근하면 안됌)
    a[i] = 2;
}

int index{};

for(int i = 0; i < 10; --i)
{
 	// !! underflow 발생 (a[-1]~a[-5]는 접근하면 안됌)
    index = 4 - i;
    a[index] = 1;
    
}
```



### 배열 복사하기(memcpy)!

```cpp
int a[5]{ 1, 2, 3, 4, 5 };
int b[5]{};

void foo()
{
    // 배열을 복사하는 유일한 방법...
    // 각 항목을 복사한다. 마법처럼 모든 항목을 동시에 복사할 수 없다.
    b[0] = a[0];
    b[1] = a[1];
    b[2] = a[2];
    b[3] = a[3];
    b[4] = a[4];
  
    //or 반복문을 쓰니 조금 간결해졌다..
    for (int i = 0; i < 5; ++i)
    {
        b[i] = a[i];
    }
    
    // 반복문까지 한꺼번에 해주는 함수가 있다!
    // 마지막 매개변수는 복사할 바이트 크기!
    memcpy(b, a, sizeof(int) * 5);
    
}
```




## 반복문(loop) 용어 

### for 문 

정해진 횟수만큼 반복하는 반복문

```cpp 
//int i =[초기값, 비교값)

//== int i= [0,5)
for (int i=0; i <5; ++i)
{
    
}
```

### while 문 

조건이 참일 동안 계속 반복되는 반복문


    ```cpp
    bool should_loop{true};

    //esc키가 눌리면 true를 리턴해주는 함수
    bool is_esc_pressed();

    while (should_loop)
    {
        std::cout << "반복 중이에양!";
        
        if (is_esc_pressed())
        {
            should_loop = false;
        }
    }
    ```



## 형변환 (casting)

```cpp
float f = 2.4f;

// 옛날 방식 (강제 형변환) - 가끔가다 예전 함수의 매개변수들은 강제형변환을 해줘야한다.
int a = (int)f;

// 최근 방식(안전한 형변환)
int b = static_cast<int>(f);
```



## 상수(const, constexpr)

```cpp
//변수 a와 b는 값이 변하지 않는다.

//런 타임(run time)에 a에 7의 값이 대입된다.
const int a{ 7 };

//컴파일 타임(compile time)에 b에 8의 값이 대입된다. 
constexpr int b{ 8 };

// 컴파일 타임의 오류를 잡아내고 싶으면 constexpr을 사용하는게 더 좋다.
```



# 함수 - 일을 처리한다.

함수 사용법을 **(syntax)**라고 한다.

```cpp
//매개변수(parameter)라고 한다.
[return_자료형] 함수_이름(매개변수_자료형 매개변수_이름)
 
 
int Multiply(int a, int b)
{
    return a*b;
}

// ==

int main()
{
    int c = Multiply(2,3);
    
    return 0;
}
```

 

```cpp
// 수학에서
// 함수 f(x)를 f(x) = 2x라고 정의할 때,
// f(2)의 값은 4다 => f(2) == 4

int f(int x)
{
	return x * 2;
}

int main()
{
	int result = f(2) * 3;    
}


//-------------------------------
int g_Accululation{}; //누적용 변수

void Accumulate(int x)
{
    g_Accululation += x;
}

int main()
{
    Accumulate(1);
    Accumulate(2);
    Accumulate(3);
    Accumulate(4);
}
```



## 여러가지 함수

### to_string(); - 매개변수를 string 값으로 변환시킨다.

```cpp
using namespace std;

int a = "455";

//float이든 int든 string으로 변경가능
// b = "456" - float
string b = to_string( a );

string c = "4543";

// d는 integer값 4543
int d = soti(c)
```



### Getter 함수

##### **프로세스 상에서 무언가를 얻어오는 함수는 모두 Get~ 으로 시작한다.**

ex) getconsolecursorposition(), getline

```cpp
getconsolecursorposition(HANDLE_OUTPUT, SHORT X, SHORT Y );


//C++에서는 라인을 통째로 읽어오는 라인 입력 함수 getline 함수가 있다. 
getline(stream, string);
```



### Setter 함수 

##### 값을 지정하는 함수는 모두 Set~ 으로 시작한다.

```cpp
SetConsoleCursorPosition()
```

 

### Debug용 함수 - assert()

```cpp
#include <cassert.h> // assert 함수 수록 

assert(SUCCEEDED(Device->CreateVertexShader(m_Blob->GetBufferPointer(), m_Blob->GetBufferSize(), nullptr, &m_VS)));

//Device->CtreateVertexShader가 성공했을 경우에만 작동시킨다. 그 이외에는 오류를 띄운다.
```



## 매크로  







## 조건문

```cpp
int a{ 3 };

if (a == 3)
{
    
}

bool b{ true };

if (b == true)  //== ★★if (b) 
{
    
}
```



## 부등호 사용법

```cpp
<= 
// '작거나' 같다.
>= 
// '크거나' 같다.
```



## 연산자

### 산술 연산자?

덧셈 +

뺄셈 -

곱셈 *

나눗셈 /

+나머지(moduler) %

```cpp
int mod = 8 % 3; // mod의 값은 2
```



### 대입 연산자 

```cpp
a += b // a를 b만큼 증가
a -= b // a를 b만큼 줄인다.
```



### 비교 연산자 

```cpp
==
!= 
<= 
>=
< 
>
```



### 논리 연산자? && ||

```cpp
if ((a == 7) && (b == 7))
    //a가 7이면서 b가 7일때
{
	
}

if ((a == 7) || (b == 7))
    //a가 7이거나 b가 7일때
{
	
}
```



### 비트 연산자

```cpp
| // or
& // and
^ // xor
! // not
```

```cpp
char a = 0b00111111 & 0b11111100 // a = 0b00111100
char b = 0b00001111 | 0b11110000 // b = 0b11111111
char c = 0b00111111 ^ 0b11111100 // c = 0b11000011
char d = !0b00111100 // d = 0b11000011
```



### 삼항연산자

```cpp
//int Max int a, int b 중에서 큰 값을 리턴하라 - 그냥 나타낼 경우
int Max(int a, int b)
{
    if (a > b)
    {
        return a;
    }
    else 
    {
        return b;
    }
}

//int Max int a, int b 중에서 큰 값을 리턴하라 - 삼항 연산자 사용
int Max(int a, int b)
{
    int result = (a > b) ? a : b;
    return result;
}
```



### 논리연산자

```cpp
//x가 3이상 5이하 일때! && (== ~~ 이고)
if ((x >= 3) && (x <= 5))
    
//x가 3이거나 5 일때! || (== ~ 이거나)
if ((x == 3) || (x == 5))
```



### ★나머지 연산(Moduler - %) 

C언어에서는 기존 사칙 연산 이외에 **'나머지 연산'**을 제공한다.

? 나머지 연산이란? 

->  Moduler 연산자라고 부른다.  , 정수 자료형에서 정수 자료형을 나누면 정수값이 나옴으로 나머지 연산자가 필요하다.

-> 나머지 연산자는 특정수의 배수일때 0이 됨으로, 특정수가 특정수의 배수일 때를 확인하는데 쓰인다.

-> 나머지 연산자는 실수(R)에서는 지원하지 않으며, 분자가(Numerator)가 0일 때, 작동하지 않는다.

```cpp
#include <iostream>

int main()
{
	int a{ 5 };
	int b{ 2 };

	int c = a % b;

	std::cout << c;

	return 0;
}
// 1
```



```cpp
#include <iostream>

int foo()
{
	int a{ 1 };
	int b{ 0 };

    // Complie fail - Numerator is 0!
	int c = a % b;

	std::cout << c;
   

	return 0;
}
```



## 프로세스와 API



응용프로그렘 (application == app)

ex) 응용 프로그렘 "계산기"를 3개 키면 프로세스가 3개이다.

 다 같은 "계산기"인데 어떻게 구분 할까??? -> 그때 필요한 개념이 '핸들(handle == h- (접두사) )'이다.

```cpp
GetStdHandle() --> 콘솔 핸들을 얻어오는 함수 (windows.h에 들어있다.)
```

API란? application programming interface의 줄임말이다.

-> 말 그대로 어플리케이션을 쉽게 만들수 있게 해주는 인터페이스 



## ★포인터 변수★

### ★메모리 주소는 ★무조건 포인터변수(==자료형*)에 저장한다.

```cpp
int* ptr{};  // = int* ptr{ nullptr };
```



**Dangling pointer : 유효하지 않은 변수를 가리키는 포인터 -> 잡기 어려운 변수 함수**

**nullptr로 대입하는 이유**

```cpp
int* g_Ptr{};

void Foo()
{
    int a{ 3 };
	// ☆★변수의 주소를 얻어고 싶을 때는 변수 이름 앞에 &를 쓴다.
    // (Referencing 참조  <> 	Dereferencing 역참조 ex) *g_Ptr )
    g_Ptr = &a;
    
    // Foo() 함수가 종료되면 g_Ptr은 Dangling pointer가 된다.
    
    g_Ptr = nullptr;
    // 이 줄이 있어야 Dangling pointer가 안됀다.
}
```



### 포인터에 자료형이 있는 이유

```cpp
void foo()
{
    char str[4]{ 'a','\0','\0','\0' };
    //str[0]은 char str배열의 포인터 이기 때문에 (int*)로 강제 현변환을 하면 char str 배열 전체를 int로 변경할 수 있다.
	int* a = (int*)&str[0];
}
```



### 포인터(형) 변수의 참조와 역참조 + 참조형 변수

```cpp
int a{ 5 };

// 포인터 변수
// 참조: 변수의 주소값을 가져옴

int* ptr_a{ &a }; // 주소를 사용한 포인터 변수 초기화 
				  //== ptr_a를 'a의 주소'를 저장한다.

// 역참조: 주소값을 통해 변수의 값을 가져옴

int b{ *ptr_a };  // 포인터의 역참조를 사용한 초기화 
				  //== ptr_a의 주소가 있는 곳의 메모리 값을 가져와서 변수 b를 초기화

// 참조형 변수

int& ref_a{ a }; // ref_a 자체가 a이다.
				 //참조형 변수는 초기화를 안 할 수 없다! (즉 반드시 가리키는 값이 존재)
```



## 참조형 변수

```cpp
// 참조형 변수(즉, 쓰여진 인수가 곧 그 변수의 실제 값이다.)
int a{ 5 };

int nor_a{ a }; // nor_a 에는 변수 a에 복사된 rvalue가 들어간다.


int& ref_a{ a }; // ref_a 자체가 a이다. 참조형 변수는 초기화를 안 할 수 없다!
				 //(즉 반드시 가리키는 값이 존재) 
				 //이후로 ref_a가 가리키는 변수를 a가 아닌 다른 변수로 바꿀 수 없다!
```

```cpp
struct my_struct
{
    my_struct(int _a, int _b) : a{ _a }, b{ _b } {};
    
	int a{};
    int b{};
};

void foo(my_struct& input);
void bar(my_struct input);
void ink(const my_struct& input);

int main()
{
    my_struct obj{ my_struct(1, 2) };
    foo(obj); // obj가 변경됨.
    bar(obj); // obj가 변경되지 않음.
    
    return 0;
}

void foo(my_struct& input)
{
    // 변수 input은 참조형 변수 (즉, 메모리 상에서 input은 obj와 동일한 변수)0
    // => input을 위한 메모리 공간이 할당되지 않는다.
    // => 즉, input을 변경하면 main() 내 obj가 변경된다.
    // 결론: input을 위해 메모리가 할당되지도 복사가 일어나지도 않는다.
    // (메모리 공간 낭비 줄이고 CPU 연산 능력 낭비도 줄인다.)
    input.a = 3;
    input.b = 4;
}

void bar(my_struct input)
{
    // 변수 input은 obj의 복사본 (즉, 메모리 상에서 서로 다른 변수)
    // => input을 위한 메모리 공간이 할당된다.
    // => 즉, input을 변경하면 input만 변경되고, main() 내 obj는 그대로다.
    // 결론: input을 위해 메모리가 할당되고 복사가 일어난다.
    input.a = 5;
    input.b = 6;
}

void ink(const my_struct& input)
{
    // 나는 무슨 짓을 해도 input의 값을 변경할 수 없다...
    // 그리고 이 사실을 ink()함수의 선언부만 보고도 알 수 있다!
}
```



### 참조형 변수 vs. 포인터 변수

```cpp
struct my_struct
{
    my_struct(int _a, int _b) : a{ _a }, b{ _b } {};
    
	int a{};
    int b{};
};

void add_one_ref(my_struct& ref_inout)
{
    ++ref_inout.a;
    ++ref_inout.b;
}

void add_one_ptr(my_struct* ptr_inout)
{
    if (ptr_inout != nullptr)
    {
        ++ptr_inout->a;
        ++ptr_inout->b;
    }
}
```



## struct와 class

**struct 기본 접근한정자 = public**

**class 기본 접근한정자 = private**

### 생성자와 소멸자

```cpp
struct meeting
{
    // struct를 정의할 때, 생성자를 작성하지 않으면 컴파일러가 자동으로 기본 생성자를 작성해 준다.
    meeting() {};
    
    // 초기화를 덮어씌우는 방법
    meeting(int _start, int _end) : start{ _start }, end{ _end }{    };
    
    // 초기화를 덮어씌우지 않는 방법
    /*
    meeting(int _start, int _end)
    { 
    	start = _start;
        end = _end;
    };
    */
    // 위와 같이 값을 대입하면 meeting이 생성될 때 start와 end가 0으로 '초기화'되고
    // 생성자 안에서 start는 _start로, end는 _end로 값이 바뀌므로 총 두 번 값 변경이 일어난다.
    int start{};
    int end{};
};
```

```cpp
class my_class
{
public:
	my_class(int data) : m_data{ data } {};
	~my_class() {};
	
private:
	int m_data{};
};
```



## 윈도우<windows.h> 관련 함수를 모를때?

구글에서 함수 이름을 치고 MSDN사이트에 들어가면 된다.

<https://docs.microsoft.com/ko-kr/windows/console/getstdhandle>



## 열거형 (== enum)

```cpp
#define WHO_HUMAN 0
#define WHO_DOG 1
#define WHO_CAT 2

#define RSP_ROCK 0
#define RSP_SCISSORS 1
#define RSP_PAPER 2

enum class e_who
{
	human,
	dog,
	cat
};

enum class e_rsp
{
	rock,
	scissors,
	paper
};

struct s_palyer
{
	//who가 0이면 사람, 1이면 개, 2이면 고양이
	int who{};
	//0이면 바위, 1이면 가위, 2면 보
	int rsp{};
};

struct s_palyer_upgraded
{
	e_who who{};
	
	e_rsp rsp{};
};

int main()
{
	s_palyer player_one{};

	player_one.who = WHO_DOG;
	player_one.rsp = RSP_SCISSORS;

	s_palyer player_two{};

	player_two.who = RSP_SCISSORS;
	player_two.rsp = WHO_DOG;
	

	s_palyer_upgraded player_up_one{};
	player_up_one.who = e_who::dog;
	player_up_one.rsp = e_rsp::scissors;

	s_palyer_upgraded player_up_two{};
	player_up_two.who = e_rsp::scissors;
	player_up_two.rsp = e_who::dog;

	return 0;
}
```



```cpp
// 함수의 매개변수가 매크로이기 때문에 찾기가 쉽지 않아서 enum class 으로 미리 저장해놓음.

// enum class 는 값을 찾기가 훨씬 쉬움
enum class EGetStdHandleParamter
{
	input = STD_INPUT_HANDLE,
	output = STD_OUTPUT_HANDLE,
	error = STD_ERROR_HANDLE,
};

HANDLE WrapperGetStdHandle(EGetStdHandleParamter param)
{
	return GetStdHandle(static_cast<DWORD>(param));
}

ex)
    g_HandleConsole = GetStdHandle(STD_OUTPUT_HANDLE);
	
	g_HandleConsole = WrapperGetStdHandle(EGetStdHandleParamter::output);
```



## 오류 모음

### 재정의 - 같은 이름의 변수를 여러번 선언함

```cpp
void Foo()
{
    int a{3};
    int a{5}; //재정의 오류 발생
}
```



## 콘솔 API 

- `HANDLE GetStdHandle(DWORD nStdHandle)`

  - 설명: 콘솔 창의 핸들을 얻어오는 함수
  - 매개변수
    - `DWORD nStdHandle`
      - 매크로로 정의된 값.
      - 매크로 목록
        - **STD_OUTPUT_HANDLE** <- 우리가 주로 쓸 매크로 값
        - STD_INPUT_HANDLE
        - STD_ERROR_HANDLE

  ```cpp
  HANDLE h_ConsoleOutput = GetStdHandle(STD_OUTPUT_HANDLE);
  ```

- `BOOL SetConsoleCursorPosition(
      HANDLE hConsoleOutput,
      COORD dwCursorPosition);`

  - 설명 커서 포지션을 설정(지정)하는 함수
  - 매개변수
    - `HANDLE hConsoleOutput`
      - 콘솔창의 핸들
    - `COORD dwCursorPosition` 
      - 내가 커서를 놓을 위치

  ```cpp
  	COORD my_xy{};
  	my_xy.X = X;
  	my_xy.Y = Y;
  
  	SetConsoleCursorPosition(g_Handle, my_xy);
  ```

  

- `SetConsoleCursorInfo( HANDLE hConsoleOutput, CONST CONSOLE_CURSOR_INFO* lpConsoleCursorInfo);`

  - 콘솔 커서를 안보이기 위해 쓰는 함수

  - 관련 자료형

    - CONSOLE_CURSOR_INFO

      ```cpp
      // 콘솔 커서의 정보
      typedef struct _CONSOLE_CURSOR_INFO {
          DWORD  dwSize; // sizeof(CONSOLE_CURSOR_INFO)를 입력해야 함
          BOOL   bVisible; // 커서를 보이게 하려면 true 안 보이게 하려면 false
      } CONSOLE_CURSOR_INFO, *PCONSOLE_CURSOR_INFO;
      ```

      

## 메모리 누수

```cpp
// 본 코드를 입력하면 출력창에 메모리 누수와 누수가 된 위치를 표시해준다.
#include <crtdbg.h>

#define _CRTDBG_MAP_ALLOC
#define new new( _NORMAL_BLOCK, __FILE__, __LINE__ )

int main()
{
    _CrtSetDbgFlag(_CRTDBG_ALLOC_MEM_DF | _CRTDBG_LEAK_CHECK_DF);
    
    return 0;
}
```

