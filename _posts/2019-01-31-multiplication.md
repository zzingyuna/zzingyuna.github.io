---
title: 구구단 게임
description: 자바스크립트로 만든 - 구구단 게임
author: yuna
date: 2019-01-31 00:00:00 +0800
categories: [Test, Javascript]
tags: [Game]
pin: false
math: true
mermaid: true
---


<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

<button id="btnStart" onclick="Start()">Start</button>
<br />
<br />
<p>남은 시간 : <span id="time"></span></p>
<p>점수 : <span id="score"></span></p>
<br />
<p id="computer">구구단질문</p>
<input type="text" id="txtUserInput" name="" value=""/>
<button class="butt1" onclick="EventClick();">정답제출</button>

<script src="https://code.jquery.com/jquery-1.10.2.js"></script>
<script>
	var score = 0; //점수
	var time = 30; //남은시간
	var isContinue = true;
	var finish; //남은시간 체크하는 setInterval
	var num1;
	var num2;
	// 게임시작
	function Start(){
		$("#score").text(score);
		$("#time").text(time);
		GetQuestion();
		finish = setInterval(StartTimeCheck, 1000);
		$("#btnStart").hide();
	}
	// 시간줄이기
	function StartTimeCheck(){
		if(time > 0){
			time = time-1; 
			$("#time").text(time);
		}else{
			clearInterval(finish);
			isContinue = false;
			if(confirm("시간이 종료되었습니다.\n새 게임을 시작하시겠습니까?")){
				score = 0;
				time = 30;
				isContinue = true;
				Start();
			}else{
				$("#btnStart").show();
				$("#computer").text("게임종료");
			}
		}
	}
	// 구구단 결과 입력시
	function EventClick(){
		if (!isContinue) {alert("시간이 종료되었습니다."); return;}
		var userVal = $("#txtUserInput").val();
		if((num1 * num2) == userVal){
			score = score+1;
			$("#score").text(score);
			GetQuestion();
		}
	}
	function GetQuestion(){
		if(isContinue){
			num1 = Math.round(Math.random()*8)+1;
			num2 = Math.round(Math.random()*8)+1;
			$("#computer").text(num1+" * "+num2+"=");
			$("#txtUserInput").val("");
			$("#txtUserInput").focus();
		}
	}
	// 엔터키 누르면 결과 반영
	$("#txtUserInput").bind('keyup', function(e){
		e.preventDefault();
		if(e.key=="Enter"){
			EventClick();
		}
	});
</script>

<!-- markdownlint-restore -->