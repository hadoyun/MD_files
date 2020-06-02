## 자료형

char = 1byte  = 8bit = 나타탤 수 있는 경우의 수 256가지 2^8 

따라서 unsinged char = [0, 255] (0부터 255포함)

unsinged char = 255 + 1 = 0 

unsinged char = 0- 1 = 255



## 반복문

```cpp
for (int i = 0; i < 10; ++i)
{
    
}

//선언식에서 선언 후 초기화, 조건식의 조건이 성립하면 본문을 실행 후, i를 증감 후, 다시 조건식에 들어가서 조건을 확인

for ( 1 ; 2 ; 3)
{
    4;
}
```



### 문자열



모든 문자열은 널문자로 끝난다.

```cpp
//"abcde"가 차지하는 메모리 공간은 a / b/ c / d / e / \0 로 총 6byte이다.

strlen(); // strlen은 널문자를 제외한 문자열의 길이를 리턴 해주는 함수이다.

char* dst = new char[length] // == 5byte 할당, 따라서 6byte인 문자열 "abcde"는 strncpy로 복사되지 않는다.

strncpy(dst, src, len); // strncpy는 문자열의 마지막에 널문자가 들어갔는지 까지 확인해준다. 
```

```cpp
int len = strlen();
char* dst = new char[len + 1];
memcpy(dst, src, len); // memcpy는 할당된 사이즈 만큼 복사하기 때문에 복사된 dst에 마지막에 널문자가 복사되지 않는다.
printf(dst); //dst는 널문자로 끝나지 않는 문자열이므로 print(dst)는 버퍼오버플로우가 발생한다.
//널문자가 나올때까지 계속 출력 된다.
```



```cpp
#include <iostream>
#include <vector>

// uint32_t 이지 uint32가 아니다...
// scant_f는 const char& (문자열만) 받을 수 있다.

uint32_t countTwo(double x, uint32_t count)
{
	uint32_t result{};

	if(x >= 2)
	{ 
		++count;
		countTwo((x/2), count);
	}
	else
	{
		result = count;

		return result;
	}
}

int main()
{
	uint32_t count{};
	double x{};

	std::cin >> x;

	int r = countTwo(x, count);

	printf("%d", r);


	return 0;
}
```