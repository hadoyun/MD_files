# windows programming 

## 프로젝트 설정

### 링커 -> 시스템 -> 하위시스템 (콘솔에서 창으로 변경) ★★

```cpp
#include <Windows.h>

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)

int APIENTRY WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPTSTR lpCmdLine, _In_ int nCmdShow)
```

모든 윈도우즈 함수의 기본 호출 규약은 __stdcall이다.

## 헝가리언 표기법 + alpha

lp = long pointer

h = handle

str = string 문자열

cmd = command 명령(어)

CmdLine = command line 명령줄

dw = double word

Wnd = window

param = parameter 매개변수

cb = count of bytes 바이트 수

Ex = Extra

hbr = handle to **brush**

DC = device context

fn = function

sz = zero-terminated string (마지막에 NULL로 끝나는 문자열)

proc = procedure 윈도우의 가장 기본적인 함수 두개중 하나 

## 윈도우 생성

```
RegisterClassEx();
CreateWindow();
ShowWindow();
```

```cpp
#include <Windows.h>

//WndProc 함수를 선언
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam);

int APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow)
{
	WNDCLASSEX wnd_class{};
	//wnd_class.cbClsExtra = 0;
	//wnd_class.cbWndExtra = 0;
	// 포인터의 정보손실 때문에 wnd_class{}의 사이즈 요구 == sizeof(WNDCLASSEX)로 사이즈 계산
	wnd_class.cbSize = sizeof(WNDCLASSEX);
	// 배경을 어떤 브러쉬로 칠할 것인가?  -원래 저장되어있는 오브젝트를 쓰기위해서 GetStockObject 사용
	// GetStockObject는 HGDIOBJ로 리턴한다.
	// GDIOBJ는 브러시, 팔레트, 비트맵, 폰트 등...이 있다.
	// 그 중에서 우리는 브러시가 필요해서 (HBRUSH)를 사용 - > 강제 형변환 (자료형을 강제로 변환
	//wnd_class.hbrBackground = (HBRUSH)GetStockObject(DKGRAY_BRUSH);
	// CreateSolidBrush는 원래 리턴형이 HBRUSH이다.
	wnd_class.hbrBackground = CreateSolidBrush(RGB(10, 220, 160));
	// 커서 모양 지정
	wnd_class.hCursor = LoadCursor(nullptr, IDC_PERSON); // 그냥 nullptr하면 기본 마우스 커서(화살표)
	// 큰 아이콘 지정
	wnd_class.hIcon = LoadIcon(nullptr, IDI_QUESTION);
	// 작은 아이콘 지정
	wnd_class.hIconSm = LoadIcon(nullptr, IDI_SHIELD);
	// instance 핸들 지정 - 자기 자신
	wnd_class.hInstance = hInstance;
	//WndProc 함수 포인터를 지정했음.
	wnd_class.lpfnWndProc = WndProc;
	// 클래스의 이름
	wnd_class.lpszClassName = "myclass";
	// 메뉴 리소스의 이름
	wnd_class.lpszMenuName = nullptr;
	//클래스 스타일 - CS_HREDRAW - 윈도우 가로 크기가 변했을때 다시 그리기 CS_VREDREDRAW  - 윈도우 세로 크기가 변했을때 다시 그리기
	wnd_class.style = CS_HREDRAW | CS_VREDRAW;

	//확장 클래스 등록 입력값을 포인터로 받기 때문에 &를 써준다.
	RegisterClassEx(&wnd_class);

	// CreateWindow라는 함수는 윈도우를 생성하고, 생성된 윈도우의 핸들을 리턴해준다. 그 값을 HWND hWnd에다가 저장한다.
	HWND hWnd = CreateWindow(
		wnd_class.lpszClassName, "내 첫 윈도우", WS_OVERLAPPEDWINDOW, 0, 0, 600, 200, nullptr, nullptr, hInstance, nullptr);

	// 생성된 윈도우(hWnd)를 보여주는 방법을 지정해서 보여줌
	ShowWindow(hWnd, nCmdShow);
	// 윈도우를 업데이트 해준다. 
	// 생성된 윈도우(hWnd)를 다시 그린다.
	UpdateWindow(hWnd);

	//메세지 무한루프
	// MSG 메세지 정보가 담길 구조체 선언
	MSG message{};
	// GetMessage == 구조체 message 에다가 메세지를 가져옴,
	while (GetMessage(&message, nullptr, 0, 0))
	{
		// message의 메세지를 번역하고 message에다가 그 값을 다시 저장한다.
		TranslateMessage(&message);
		//메세지 보내주기 (wndproc 로...)
		DispatchMessage(&message);
	}

	return 0;
}

// 윈도우 메세지를 어떻게 처리할 것인가? 
// message - 무슨일이 일어났는 지를 알려주는 변수
// wParam, lParam 구체적인 정보를 담고 있는 변수
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
	switch (message)
	{
	case WM_CHAR:
		break;
	case WM_DESTROY:
		PostQuitMessage(0);
		break;
	default:
		break;
	}

	return DefWindowProc(hWnd, message, wParam, lParam);
}
```

