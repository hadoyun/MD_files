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

