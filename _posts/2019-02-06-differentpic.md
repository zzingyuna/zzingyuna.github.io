---
layout: post
title: 틀린그림 찾기
---

# 틀린그림찾기 게임

자바스크립트로 만든 게임 - 틀린그림찾기 게임

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
body {  background-color: gainsboro;}
h1 {  color: darkcyan;  text-align: center;}
p {  font-family: verdana;  font-size: 20px;}
img {cursor: pointer;}
.ok {background:url("https://en.pimg.jp/009/321/808/1/9321808.jpg");width: 30px;height:30px;background-size: 30px;}
.ans1{position: absolute;top: 465px;left: 103px;}
.ans1_1{position: absolute;top: 465px;left: 493px;}
.ans2{position: absolute;top: 555px;left: 199px;}
.ans2_1{position: absolute;top: 555px;left: 583px;}
.ans3{position: absolute;top: 573px;left: 63px;}
.ans3_1{position: absolute;top: 573px;left: 447px;}
.ans4{position: absolute;top: 483px;left: 238px;}
.ans4_1{position: absolute;top: 483px;left: 636px;}
.ans5{position: absolute;top: 630px;left: 80px;}
.ans5_1{position: absolute;top: 584px;left: 473px;}
  </style>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>

<h1>틀린그림찾기</h1>
<p>사진두개 비교</p>
<br />

<button id="btnStart" onclick="Start()">Start</button>

<br />
<br />
<p>남은 시간 : <span id="time"></span></p>
<p>남은 목숨 : <span id="live"></span></p>
<p style='color:gainsboro;'>해, 귀, 잔디, 새, 꼬리</p>
<img src="http://itcm.co.kr/files/attach/images/813/303/896/9560e06ad62db403df4053f066ee9b6e.jpg"/>

<div class="ans1"></div>
<div class="ans1_1"></div>

<div class="ans2"></div>
<div class="ans2_1"></div>

<div class="ans3"></div>
<div class="ans3_1"></div>

<div class="ans4"></div>
<div class="ans4_1"></div>

<div class="ans5"></div>
<div class="ans5_1"></div>

<script>
	var time = 30; //남은시간
	var live = 2;  //남은목숨
	var finish; //남은시간 체크하는 setInterval

	// 게임시작
	function Start(){
		time = 30;
		live = 2;
		$("#time").text(time);
		$("#live").text(live);
		finish = setInterval(StartTimeCheck, 1000);
		$("#btnStart").hide();
		$("div.ok").removeClass("ok");
	}

	// 시간줄이기
	function StartTimeCheck(){
		if(time > 0){
			time = time-1; 
			$("#time").text(time);
		}else{
			clearInterval(finish);
			if(confirm("시간이 종료되었습니다.\n새 게임을 시작하시겠습니까?")){
				time = 30;
				Start();
			}else{
				$("#btnStart").show();
			}
		}
	}

	$("img").bind("click", function(event) {
		if(live < 1){
			alert("Game OVER!\n목숨이 없습니다. 새로 게임을 시작해주세요."); 
			clearInterval(finish);
			$("#btnStart").show();
			return false;
		}
		
        var x = event.pageX - this.offsetLeft;
        var y = event.pageY - this.offsetTop;
		
		//해
		// 98,110  120,110  98,132  120,132
		// 489,105  510,105  489,125  510,125
		if(x >= 98 && x <= 120 && y >= 110 && y <= 132){
			alert("해 선택 정답!!");
			$("div.ans1").addClass("ok");
			$("div.ans1_1").addClass("ok");
		}
		else if(x >= 489 && x <= 510 && y >= 105 && y <= 125){
			alert("해 선택 정답!!");
			$("div.ans1").addClass("ok");
			$("div.ans1_1").addClass("ok");
		}
		//귀
		// 189,190  220,190  189,222  220,222
		// 577,188  611,188  577,215  611,215		
		else if(x >= 189 && x <= 220 && y >= 190 && y <= 222){
			alert("귀 선택 정답!!");
			$("div.ans2").addClass("ok");
			$("div.ans2_1").addClass("ok");
		}	
		else if(x >= 577 && x <= 611 && y >= 188 && y <= 215){
			alert("귀 선택 정답!!");
			$("div.ans2").addClass("ok");
			$("div.ans2_1").addClass("ok");
		}
		//잔디
		// 60,216  78,216  60,261  78,261
		// 444,200  464,200 444,258  464,258
		else if(x >= 60 && x <= 78 && y >= 216 && y <= 261){
			alert("잔디 선택 정답!!");
			$("div.ans3").addClass("ok");
			$("div.ans3_1").addClass("ok");
		}	
		else if(x >= 444 && x <= 464 && y >= 200 && y <= 258){
			alert("잔디 선택 정답!!");
			$("div.ans3").addClass("ok");
			$("div.ans3_1").addClass("ok");
		}
		//새
		// 225,125  276,125  225,159  276,159
		// 610,112  672, 112  610,151  672, 151
		else if(x >= 225 && x <= 276 && y >= 125 && y <= 159){
			alert("새 선택 정답!!");
			$("div.ans4").addClass("ok");
			$("div.ans4_1").addClass("ok");
		}	
		else if(x >= 610 && x <= 672 && y >= 112 && y <= 151){
			alert("새 선택 정답!!");
			$("div.ans4").addClass("ok");
			$("div.ans4_1").addClass("ok");
		}
		//꼬리
		// 63,261  107,261  63,299  107,299
		// 467,206  497,206  467,255  497,255
		else if(x >= 63 && x <= 107 && y >= 261 && y <= 299){
			alert("꼬리 선택 정답!!");
			$("div.ans5").addClass("ok");
			$("div.ans5_1").addClass("ok");
		}	
		else if(x >= 467 && x <= 497 && y >= 206 && y <= 255){
			alert("꼬리 선택 정답!!");
			$("div.ans5").addClass("ok");
			$("div.ans5_1").addClass("ok");
		}
        else{
			//alert("X Coordinate: " + x + " Y Coordinate: " + y);
			live = live-1;
			$("#live").text(live);
			alert("틀렸습니다");
		}
		
		if($("div.ok").length == 10){
			alert("축하합니다!!\n모두 맞췄습니다!!");
			clearInterval(finish);
			$("#btnStart").show();
			return false;
		}
    });
</script>
</body>
</html>
```
