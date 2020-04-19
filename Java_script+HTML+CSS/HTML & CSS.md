# HTML & CSS

### HTML???

-> 하이퍼텍스트 기술용 언어(Hypertext Mark-up Language)

웹의 구조 중 - 웹문서

## HTML 구조 (태그) 파악하기

웹 문서 만들기에 앞서... doc를 치고 tap ->

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```





### CSS???

->**Cascade Style Sheet**

웹의 구조 중 - 디자인



### Javascript

-> 예전에는 웹의 동작

-> ★★현재는 웹의 모든 것!!!!



## 개발자 도구

-> 웹프로그래밍을 할 때 죽을때까지 함께 하는 친구 ㅠㅜ

(ctrl + shift + i) or (F11)



### 배끼기 전 - > css 지우기!

콘솔 탭에서 

문서의 해드 부모 노드에서 자식 들을 제거하기 -> ★★document.head.parentNode.removeChild(document.head);

현재 웹 문서의 html 부분만 표시됨.

head 태그를 지우는 자바 스크립트 코드



CSS를 지웠음에도 디자인 되어있는 페이지가 표시됨 -> iframe(아이프레임)

### iframe??

웹페이지 안에 웹페이지를 삽입하는 방법

```html
<iframe src="불러올 웹페이지의 URL" scrolling="스크롤링 허용여부(yes|no|auto)">

</iframe>
```



- src : 불러올 페이지의 URL(인터넷 상에서의 파일 주소)

- scrolling : 아이프레임 안에서 스크롤링을 허용할 것인지를 결정
- auto : 스크롤이 필요한 경우에만 스크롤 바를 노출(기본값)
- yes : 스크롤링 허용, 필요없는 경우에도 스크롤 바를 노출
- no : 스크롤 하지 않음



(이 외에도 테두리 사용여부 등의 속성이 있지만 CSS를 통해 배워보자)



## NAVER 만들어 (무작정) 만들어 보기

```html
<!DOCTYPE html> //쌍이 없어영!
<html> //!Doctype 말고는 여는 태그 닫는 태그가 따로 있다.
    <head> /* head 태그는 html의 자식 태그 */
        
    </head> /*head 태그와 body 태그와는 서로 형제 */
    <body>

    </body>
</html>
```

head 태그는 웹페이지의 설정

body는 현재 우리가 보는 웹 페이지의 내용을 나타낸다.





### Favicon 설정

```html
<link rel="shortcut icon" type="image/x-icon" href="img/favicon.ico">
```

img 파일을 만든 후, 콘솔 탭에서 Favicon의 주소를 찾아 img 파일에 다운로드 해준 뒤,

해당 코드 추가!

++ (./ 는 현재 폴더를 의미한다.)

### contents 설정

```html
<body> 
        <h1>헤이버</h1>
        <h2>검색창</h2>
        <h2>실시간 검색어</h2>
        <h3>로그인</h3>
        <h3>뉴스</h3>
        <h3>법률</h3>
        <h3>쇼핑</h3>
    </body>
```

중요함에 따라 h ~ 뒤에 숫자가 달라진다.



```html
<a href='#'>헤이버</a>
```

href는 a에 대한 attribute이다.



### 필드셋을 만들어보자!

```html
<fieldset>
    <legend>검색</legend>
    <input />
    <input type="checkbox"/>
    <input type="radio"/>
    <input type="radio"/>
</fieldset>
```

input 타입! - > chechbox  말그대로 체크 박스

​					-> ridio 양자 택일이나 선택