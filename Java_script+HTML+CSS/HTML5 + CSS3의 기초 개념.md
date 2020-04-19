# HTML5 + CSS3의 기초 개념

### HTML5 (Hyper Text Markup Language)

HTLM5는 웹 페이지 구축을 위한 뼈대가 되는 '마크 업' 언어이다.

(마크업 언어란 문서나 데이터의 구조를 표시하는 언어의 한 종류 인데

typora에서 ctrl + 1은 문장의 크기를 늘리는데 이는 마크업에서 ### + 문장과 같다.



현재 HTML5는 2014년에 '공식' 표준으로 지정된 마크업 언어이다.

(2016년 HTML5.1 , 2017년 HTML5.2, HTML5.3은 현재 작업중)





### CSS3

CSS란? 'cascading Style Sheet'의 줄임말이다.

CSS로 문서의 스타일을 적용하는 방식이 마치 폭포수처럼 연속적으로 적용된다는 말에서 유래됨.

CSS는 HTML만으로 해내기 어려운 시각적 디자인을 효과적으로 적용하기 위한 코드이다.



 HTML이 웹 페이지 구축을 위한 뼈대라고 하면, CSS는 웹 디자인을 완성시켜주는 언어이다.

CSS는 단독으로 아무런 역활을 할 수 없으며 HTML문서가 있어야만 작동을 하게 된다.

- 자바스크립트를 이용해서 만들어야했던 애니메이션과 같은 효과를 CSS에서 제공함
- 최신 브라우저에서는 CSS3의 모든 속성들을 지원함 - > 디자인적으로 많은 효과를 줄 수 있음.







# 웹사이트의 구조

### <헤더(header)> 

회사 로고와 목차

### <메인 이미지(article)>

### <본문(article)>

### <본문2(article)>

### <푸터(footer)>

사이트에 관련된 주요 정보 - 구성 및 참고 자료



## 헤더 만들기

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <meta name = "viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        
    </head>
    <body>
        
        
    </body>
</html>
```

위 코드는 HTML5 문서의 가장 일반적인 구조를 볼 수 있다.

HTML은 태그라는 언어로 구성되어있는데, 태그의 특징은 <>기호로 나타냅니다.

<> 기호는 오직 HTML문서에서만 사용됩니다.

->헤드 태그를 없에서 CSS와 head를 지우고싶다면 아래의 코드를 사용!

```javascript
document.head.parentNode.removeChild(document.head);
```



### < !DOCTYPE html >

DOCTYPE은 어떤 버전의 HTML 언어로 기술 할 것인지를 묻는 문서의 타입 죽, 형식을 명시하기 위한 것으로 

HTML문서의 첫번째 줄에 나타나야하는 부분이다.

현재 표준 규격은 HTML5로써 특별한 이유가 없다면 모든 웹사이트 부분에서 < !DOCTYPE html >을 사용한다.

! HTML문서를 수정할 경우가 있는 경우, HTML5 문서가 아니면 CSS3 속성이 적용되지 않는다는 것만 알면 된다.



VS Code 등에서 쉽게 사용할 수 있는 방법 doc를 치면 자동적으로 <!DOCYPE html>을 포함한 html 5 기본 코드가 완성! 된다.



### ''< HTML >"  ~ " < /HTML >

예제를 보면 <HTML로 시작해서 </HTML로 끝나는데 HTML의 문서들은 고유의 태그들이 있으며, 시작이 있으면 닫힘이 반드시 존재한다.

(단 몇몇 태그들을 닫힘이 없을 수 있음)

<HTML이 의미하는 것은 "현재 이 문서는 HTML 문서이다"라고 '브라우저'에게 알려주는 역활을 한다.

모든 HTML 태그는 <HTML 내부에서 작성해야 한다.



# HTML5의 기본 태그

<html> -  문서정의 태그 최상위 태그로 웹문서의 시작과 끝을 지정한다.

<head> - 머리말 태그 : 부가적인 정보(스타일 시트와 스크립트 정보)를 지정한다.
    웹브라우저 창에서는 실제로 표시되진 않음
    <title> </title>, <meta>, <style> </style>, <link> 를 포함한다.

<meta> - 메타 태그 : 웹 문서의 정보(작성자, 형식, 인코딩 방식)을 지정한다.
빈 태그로 종료 태그가 없다.

<title > - 제목 태그 : 웹문서의 제목을 지정한다.
     브라우저 창 위쪽의 타이틀 바 영역에 표시한다.
<body> 내용 태그 웹문서로 표현될 실제 내용을 지정한다.

 대부분의 내용은 실제로 웹 브라우저에 표시된다.



### head와 body 태그 

HTML 페이지 안에서는 head와 body 태그를 입력한다.

body는 사용자에게 실제로 보이는 부분이며, head 태그는 body에서 필요한 스타일 시트와 자바스크립트를 제공하는데 사용된다.

```html
<head>
    HTML 문서에 대한 정보, CSS 속성,
    
</head>
<body>
    각종 HTML 태그 정보
    하단 부분에 자바스크립트
    
</body>
```

head 내부에는 현재 HTML 문서에 대한 정보와 CSS 파일 및 속성, 또는 DOM(문서객체 모델)에 영향을 미치지 않는 자바 스크립트 파일과 메타 정보와 같은 것들을 넣어둔다.



### div 태그

```html
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 			"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
    <title>Insert title here</title>
</head>
    
<body>
	<div style="background-color:red">첫번째 영역</div>
	<div style="width:100px; height:100px; background-color:#CF0">두번째 영역</div>
	<div style="width:50px; height:50px; border:1px solid black; background-		color:yellow">세번째 영역</div>
	<div style="width:100px; height:50px; border:3px solid black; float:right">네번째 영역</div>
	<div style="width:30; height:30px background-color:green; float:left; margin:30px;">네번째 영역</div>
</body>
</html>
```







div 속성입니다.



**1.** style은 <div>태그의 스타일을 지정해주는 것으로 다른 속성들을 사용할 수 있게끔 해줍니다. <div style="">

**2.** width는 <div>의 가로 크기를 정해줍니다. px(픽셀)단위로도 정할 수 있고 %(비율)단위로도 정할 수 있습니다.

**3.** height는 <div>의 세로 크기를 정해줍니다. px(픽셀)단위로도 정할 수 있고 %(비율)단위로도 정할 수 있습니다.

**4.** border은 <div>의 테두리의 굵기를 정해줍니다. 숫자가 클수록 굵기가 굵어집니다.

**5.** background-color은 <div>태그의 배경색상을 정하는 속성입니다.

**6.** float은 div의 정렬(좌,우)을 하는 속성입니다. 가운데정렬은 안됩니다.

**7.** margin은 div의 정렬기준 끝에서부터 여백을 주는 속성입니다. (margin-top,margin,-bottom,margin-left,margin-right)



### P 태그 

<p></p> 태그는 paragraph, 즉 문단의 약자로, 하나의 문단을 만들 때 쓰입니다.
```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>오디오 재생입니다!</title>
</head>
<body>
    <audio controls="controls">
        <source src="video/">
    </audio>
    <p>
        여기에 사용된 
        <a href="https://www.youtube.com/watch?v=md7dK5-qvHc">
        mps 파일
        </a>
    </p>
</body>
</html>
```

### br 태그



```html
<html>
	<body>
		the
		first
		line<br>
		the second line
	</body>
</html>

//출력 결과
//the first line
//the second line
```

### aside 태그

문서의 주요 내용과 간접적으로만 연관된 부분을 나타냅니다. 주로 사이드바 혹은 콜아웃 박스로 표현합니다.

```html
<article>
    <aside>
        <h2>사이트바 제목</h2>
        <section>
            <ul>
                <li>사이드 메뉴 1</li>
                <li>사이드 메뉴 2</li>
                <li>사이드 메뉴 3</li>
            </ul>
        </section>
    </aside>
</article>
```





## 





# Canvas 태그와 기본적인 JAVAscript를 사용해서 그림을 그려보자!

```html
<head>
    <meta ~~~~~~>
    <style>
        canvas {
            border: 1px solid blue;
        }
    </style>
    <title>canvas</title>
</head>

<body onload = "init()">
    <canvas id="canvas" width="500" height="300"></canvas> // 이미지를 그릴 캔버스 지정 
    
```

**step 1 ~ 캔버스 스타일 지정**

**style 태그를 사용해서 canvas의 속성을 지정해준다. **

```html
    <style>
        canvas {
            border: 1px solid blue;
        }
    </style>
```

**step 2 - body  태그에서 onload 이벤트 호출!**

```html
<body onload = "init()">
```

혹은 

```html
<script language="javascript">
  function init() {
    <!-- 함수의 내용! --!>
  }
</script>
```

와 같이 함수를 미리 작성하고 사용할 수도 있다.

**step3 - Canvas 태그를 사용하여 그림을 그릴 캔버스를 설정한다.**

```html
 <canvas id="canvas" width="500" height="300"></canvas>
<!--canvas라는 이름의 크기 500 / 높이 300의 캔버스를 사용할 것이다.--!>
```

**step4 - script 태그 안에 js 함수 작성!**

사용되는 js 함수

getElementById(); ~ Canvas 태그로 설정된 id를 사용 설정된 canvas 를 가져온다(get).

getContext(); - 

``` html
<script>
        function init() {
            var canvas = document.getElementById("canvas"); 
            var ctx = canvas.getContext("2d");
            draw(ctx);
          
            ctx.beginPath(); // 경로의 시작을 알림
            
            ctx.moveTo(0,0); //0,0에서 부터 그림 그리기
            ctx.lineTo(500,300); // X축 500 Y축 300까지
            
            ctx.lineWidth = 2; // 선두깨
            ctx.strokeStyle = "#0033ff"; // 선 색상 (파란색)

            ctx.stroke(); // 선을 그려줌
        }
    </script>
```

