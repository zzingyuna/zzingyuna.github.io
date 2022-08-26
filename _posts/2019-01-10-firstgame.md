---
layout: post
title: 두더지 게임
---

# 두더지 게임

자바스크립트로 만든 두더지 게임

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="description" content="Free Web tutorials">
  <meta name="keywords" content="HTML,CSS,XML,JavaScript">
  <meta name="author" content="John Doe">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Page Title</title>
    <style>
body {
  background-color: lightblue;
}

h1 {
  color: white;
  text-align: center;
}

p {
  font-family: verdana;
  font-size: 20px;
}

button.on{
background-image: url('https://data.ac-illust.com/data/thumbnails/38/3801adeee015000abdea7faec50e9446_t.jpeg');
background-size: 120px;
width:100px;
height:100px;
}
button.off{
background-color: blue;
width:100px;
height:100px;
}
</style>
    <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>

<h1>두더지 잡기!!</h1>
<p>남은 시간 : <span id="time"></span></p>
<p>점수 : <span id="score"></span></p>

<button onclick="Start()">Start</button>
<br />
<br />
<button onclick="myFunction(this)" id="btn1" name="monster" class="off"></button>
<button onclick="myFunction(this)" id="btn2" name="monster" class="off"></button>
<button onclick="myFunction(this)" id="btn3" name="monster" class="off"></button><br/>
<button onclick="myFunction(this)" id="btn4" name="monster" class="off"></button>
<button onclick="myFunction(this)" id="btn5" name="monster" class="off"></button>
<button onclick="myFunction(this)" id="btn6" name="monster" class="off"></button><br/>
<button onclick="myFunction(this)" id="btn7" name="monster" class="off"></button>
<button onclick="myFunction(this)" id="btn8" name="monster" class="off"></button>
<button onclick="myFunction(this)" id="btn9" name="monster" class="off"></button>

<script>
    var score = 0;
    var game;
    var time = 30;
    var finish;

    function myFunction(item) {
        //var x = document.getElementById("demo");
        //x.style.fontSize = "25px"; 
        //x.style.color = "red"; 
    
        //alert(item.className);
        console.log(item.className);
        if(item.className=="on"){
            score = parseInt($("#score").text());
            $("#score").text(score+1);
        }
    }

    function ChangeColor(){
        var num = Math.round(Math.random()*10);
        if(num > 9 || num < 1){ num = 1;}
        $("button.on").addClass("off");
        $("button.on").removeClass("on");
        $("#btn"+num).removeClass("off");   
        $("#btn"+num).addClass("on");
    }

    function Start(){
        game = setInterval(function() { ChangeColor(); }, 500);
        time = 30;
        finish = setInterval(function() { 
                    if(time < 1){ 
                        clearInterval(game); 
                        clearInterval(finish); 
                        $("button[name=monster]").removeClass("on");
                        $("button[name=monster]").removeClass("off");
                        $("button[name=monster]").addClass("off");
                        alert("축하축하~!!\n"+$("#score").text()+"점 획득하셨습니다!");
                    } 
                    $("#time").text(time);
                    time--;
                }, 1000);
    }
    
    $(document).ready(function(){   
        $("#score").text(score);  
        $("#time").text(time);
    });
</script>
</body>
</html>

```


  
수정 보완 v2  
(앱에서 동작할때.. touchstart로 변경)  
```

<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="utf-8">
<title>ttt</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" />
<script src="https://code.jquery.com/jquery-1.10.2.js"></script>  
<style>
body {
  background-color: lightblue;
}

h1 {
  color: white;
  text-align: center;
}

p {
  font-family: verdana;
  font-size: 20px;
}

button{
background-color:blue;
width:90px;
height:90px;
}

button:active{
background-color:black;
width:85px;
height:85px;
}

td{
width:92px;
height:92px;
}

.on{
background-image: url('https://data.ac-illust.com/data/thumbnails/38/3801adeee015000abdea7faec50e9446_t.jpeg');
background-size: 120px;
width:90px;
height:90px;
}

.startbtn{
display: block;
width: 50%;
border: none;
background-color: #4caf50;
padding: 14px 28px;
font-size: 16px;
cursor:pointer;
text-align: center;
}
</style>
</head>
<body id="Evt_506">
<br />
<br />
<h1>두더지 잡기!!</h1>
<p>남은 시간 : <span id="time"></span></p>
<p>점수 : <span id="score"></span></p>

<a href="#" onclick="Start(); return false;" class="startbtn">Start</a>
<br />
<br />
<table border="0">
<tr>
<td><button onclick="" id="btn1" name="monster" class=""></button></td>
<td><button onclick="" id="btn2" name="monster" class=""></button></td>
<td><button onclick="" id="btn3" name="monster" class=""></button></td>
</tr>
<tr>
<td><button onclick="" id="btn4" name="monster" class=""></button></td>
<td><button onclick="" id="btn5" name="monster" class=""></button></td>
<td><button onclick="" id="btn6" name="monster" class=""></button></td>
</tr>
<tr>
<td><button onclick="" id="btn7" name="monster" class=""></button></td>
<td><button onclick="" id="btn8" name="monster" class=""></button></td>
<td><button onclick="" id="btn9" name="monster" class=""></button></td>
</tr>
</table>
<br />
<script>
    var score = 0;
    var game;
    var time = 30;
    var finish;
	var num = 0;

	// 두더지 클릭
	$("button[name=monster]").on('touchstart', function (e){
		e.preventDefault();
        if($(this).attr("class")=="on"){
            score = score + 1;
            $("#score").text(score);
        }
		return false;
	});

	// 두더지 보이기 숨기기
    function ChangeColor(){
        var num = Math.round(Math.random()*10);
        if(num > 9 || num < 1){ num = 1;}
        $("button.on").removeClass("on");
        $("#btn"+num).addClass("on");
    }

	// 게임 시작
    function Start(){
		score = 0;
		time = 30;
        $("#score").text(score);
        $("#time").text(time);
        game = setInterval(ChangeColor, 500);
        finish = setInterval(CountTime, 1000);
    }

	function CountTime() {
		time = time -1;
		$("#time").text(time);
		if(time < 1){
			clearInterval(game);
			clearInterval(finish);
			$("button[name=monster]").removeClass("on");
			alert("축하축하~!!\n"+$("#score").text()+"점 획득하셨습니다!");
		}
	}

</script>
</body>
</html>

```



