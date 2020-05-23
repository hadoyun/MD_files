# 심플테트리스


## 랜더링

게임 상에서 모든 그리기는 랜더링의 시작(beginRendering())~ 랜더링의 끝(endRendering()) 안에서 들어있다.

```cpp
g_simpleTetris.beginRendering(clearColor);
{
	g_simpleTetris.drawBoard(boardPosition, Color(0, 60, 100), Color(200, 200, 200));
	
	g_simpleTetris.drawImageAlphaToScreen(1, position);

	g_simpleTetris.useFont(1);
	g_simpleTetris.drawTextToScreen(Position2(0, 0), Size2(SimpleTetris::kBoardSizePixel.x + 20, boardPosition.y - 10), L"TETRIS", Color(200, 100, 100),
		EHorzAlign::Center, EVertAlign::Center);

	g_simpleTetris.useFont(0);
	g_simpleTetris.drawTextToScreen(Position2(kWidth - 100, 0), L"FPS: " + g_simpleTetris.getFpsWstring(), fpsColor);
}
g_simpleTetris.endRendering();

```

랜더링 시작(클리어 컬러)

## 왜 frontDc() 만 있으면 뭐가 문제 인가?
화면에 보여주기 전에 그리는 과정을 보여주지 않기 위해서 ...!
(깜빡거림이 발생)



## 게임 입력 받기

게임의 모든 입력은 while문 안에서 이루어져야한다.

```cpp
while (g_simpleTetris.update() == true)
{	// 다음 블록 큐를 업데이트 하는 함수
	g_simpleTetris.updateNextblockQueue();
	// 게임 레벨을 업데이트 하는 함수
	g_simpleTetris.updateGameLevel();

	//gameover가 아닌 상황.
	if (g_simpleTetris.isGameOver() == false)
	{	// 
		if (g_simpleTetris.tickInput() == true)
		{	//퍼즈가 트루라면 입력을 받지 않고 움직이지 않기
			if (g_simpleTetris.getPause() == true)
			{
				__noop;
			}
			else
			{
				//0x8001: key가 처음 눌렸을 때를 의미함.
				if (GetAsyncKeyState(VK_LEFT) == SHORT(0x8001))
				{
					g_simpleTetris.move(EDirection::E);
				}
				if (GetAsyncKeyState(VK_RIGHT) == SHORT(0x8001))
				{
					g_simpleTetris.move(EDirection::W);
				}
				if (GetAsyncKeyState('Z') == SHORT(0x8001))
				{
					//g_simpleTetris.move(EDirection::N);
					g_simpleTetris.rotate(true);
				}
				if (GetAsyncKeyState('X') == SHORT(0x8001))
				{
					//g_simpleTetris.move(EDirection::N);
					g_simpleTetris.rotate(false);
				}
				if (GetAsyncKeyState(VK_DOWN) == SHORT(0x8001))
				{
					g_simpleTetris.move(EDirection::S);
				}
				if (GetAsyncKeyState(VK_UP) == SHORT(0x8001))
				{
					g_simpleTetris.playFastSound();
					while (g_simpleTetris.move(EDirection::S) == true)
					{

					}
				}
				if (g_simpleTetris.tickGameSpeedTimer() == true)
				{
					g_simpleTetris.move(EDirection::S);
				}
				if (GetAsyncKeyState('H') == SHORT(0x8001))
				{
					g_simpleTetris.holdBlock();
				}
			}
			if (GetAsyncKeyState('P') == SHORT(0x8001))
			{	//토클
				g_simpleTetris.togglePause();
			}

```



### 토글 (toggle)

```cpp
void hady::SimpleTetris::togglePause() const
{
	_pause = !_pause;
}
////////////////////////////////////////////
bool hady::SimpleTetris::togglePause() const
{
	return _pause = !_pause;
}
// in class
// mutalbe bool _pause{false};
```


## 다양한 알고리즘 사용하기 + 계산을 쉽게 해주는 구조체들

블록 계산을 용이하게 하고, 블록의 등록을 쉽게 하기 위해서 4*4 이중 배열을 만들고 그 안에 set이라는 함수를 만든다. 

```cpp
struct BlockContainer
{
	uint8 data[4][4]{};

	void set(uint8 _00, uint8 _10, uint8 _20, uint8 _30,
		uint8 _01, uint8 _11, uint8 _21, uint8 _31,
		uint8 _02, uint8 _12, uint8 _22, uint8 _32,
		uint8 _03, uint8 _13, uint8 _23, uint8 _33)
	{
		data[0][0] = _00;
		data[0][1] = _10;
		data[0][2] = _20;
		data[0][3] = _30;

		data[1][0] = _01;
		data[1][1] = _11;
		data[1][2] = _21;
		data[1][3] = _31;

		data[2][0] = _02;
		data[2][1] = _12;
		data[2][2] = _22;
		data[2][3] = _32;

		data[3][0] = _03;
		data[3][1] = _13;
		data[3][2] = _23;
		data[3][3] = _33;
	}
};
```

### 블록의 방향과 종류

```cpp
//블록의 방향
	enum class EDirection
	{
		N,
		W,
		S,
		E,
		MAX
	};
```

```cpp
//블록의 종류
	enum class EBlockType
	{
		None,
		Used, // Gray
		I, // Blue
		T, // Purple
		O, // Yellow
		L, // Orange
		InvL, // Skyblue
		Z, // green
		S, // Pink
		Bingo,
		MAX
	};
```

