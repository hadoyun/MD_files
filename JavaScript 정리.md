# JavaScript



## 애니메이션

###  requestAnimationFrame() / cancelAnimationFrame()

```cpp
    function drawRect(positionX, positionY, witdh, height, color)
    {
        var canvas = document.getElementById("canvas");

        if(canvas.getContext)
        {
        var ctx = canvas.getContext('2d');
        ctx.fillStyle = color;
        ctx.fillRect(positionX, positionY, witdh, height);
        }
        else
        {
            alert("캔버스를 지원하지 않는 텍스쳐 입니다.")
        }
    }

    //흐른시간 
    let elapsed = 0;
    let start_time = 0;
    let x_pos = 0;
    let y_pos = 0;

    let rafId;
    
    
    function render() {
        var now = new Date().getTime();

        if(start_time == 0)
        {
            start_time = now;
        }
        elapsed = now - start_time;

        ++x_pos;
        ++y_pos;

        clearCanvas();

        drawRect(20 + x_pos,20 + y_pos,40,40,'rgb(255,100,100)');

        //render 무한 루프 시작
        rafId = requestAnimationFrame(render);

        if(elapsed > 5000)
        {
            //조건이 만족시 무한 루프 끝
            cancelAnimationFrame(rafId);
        }
    }
```

### requestAnimationFrame()에 매개변수가 있는 함수 추가하기

```cpp

	rafId = requestAnimationFrame(function() {
        _swapArray(a,b);
    });
```



### 그래픽 프로그래밍 주의 사항!

```js
//var text_color = "rgb(0, 160, 123)"; 그래픽 프로그래밍은 매번 지웟다가 그리는 것을 반복하기 때문에 전역으로 선언하면 색이 한번만 바뀌어용...

drawArray();

function drawArray()
{
    for(var i = 0; i < array.length; ++i)
    {   
        var bar_color = 'rgb(' + 255 + ',' + (100 + (array[i] * 15)) + ',' + 100 + ')';

        var text_color = "rgb(0, 160, 123)";
        
        if(i < sorted_count)
        {
            bar_color = 'rgb(100,100,100)';
            text_color = 'rgb(255,255,255)';
        }
        //pos_x, pos_y, width, height
        drawRect(60 + x_pos_array[i], 280 - (array[i] * 20), 40, 40 + (array[i] * 20), bar_color);

        writeText(array[i], 75 + x_pos_array[i], 300, textcolor);
    }
}
```



