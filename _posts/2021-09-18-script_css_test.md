---
title: script + css 테스트
description: script + css 테스트
author: yuna
date: 2021-09-18 00:00:00 +0800
categories: [Test, Javascript]
tags: []
pin: false
math: true
mermaid: true
---


<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

<script src="https://code.jquery.com/jquery-1.10.2.js"></script>
<script type="text/javascript">
	function load(){
		$("div[itemprop=articleBody] #a").css("color","red");  //아이디로 탐색, 
		$("div[itemprop=articleBody] .b").css("color","blue");  //클래스로 탐색, 
		$("div[itemprop=articleBody] #c>p").css("color","yellow"); //자식 엘리먼트 탐색 - <div>태그,아이디,클래스, 모두 적용가능
		//$("div[itemprop=articleBody] #d>p>span").css("backgroundColor","pink"); 아래와 같은 결과
		$("div[itemprop=articleBody] #d span").css("backgroundColor","pink"); //한칸 띄움=아이디가 d인 div태그의 자식은 p태그이고 그 자식이 span으로 한단계 더 아래를 공백으로 처리
		                                                                           // 포함되어 있으면 선택됨.
		$("div[itemprop=articleBody] div:eq(1) span").css("backgroundColor","green"); //아이디 지정하지 않고 배열개념으로 적용
		//div태그배열(0부터시작)에서 1번 방내에 포함된 span태그의 색을 변경해줌
		//:eq(배열방번호) 엘리먼트를 인덱스로 탐색
		$("div[itemprop=articleBody] div:eq(1) span").click(function(){
			$(this).css("backgroundColor","red");
		});
		$("div[itemprop=articleBody] p:contains('김윤아')").css("color","green"); //컨테인즈 내용으로 검색.. p태그 내에 김윤아라는 내용이 있는 곳에 적용됨
		$("div[itemprop=articleBody] p[st='aa']").css("color","blue");  // 속성으로 검색.. p태그 내에 st속성의 명이 aa인 내용에 적용
		$("div[itemprop=articleBody] div:eq(4) p:nth-child(even)").css("color","yellow");
		$("div[itemprop=articleBody] div:eq(4) p:nth-child(1)").css("backgroundColor","blue"); //n(숫자)의 인덱스번호가 1부터 시작
		$("div[itemprop=articleBody] div:eq(4) p:nth-child(4n+1)").css("backgroundColor","pink"); // 계산식 적용시 인덱스번호 0부터 시작
		
		$("#startToggle").bind("click",function(){
			$(".toggletest").toggle(2000);
		});
		$("#smalldiv").click(function(){
			$("#smalldiv").animate({ width:"50%", opacity:0.4, fontSize:"26pt", marginLeft:"1in", borderWidth:"15px" }, 2000);
		});
		
		setInterval(function(){
			var div = $("#m");
			$(".a").first().appendTo(div);
		}, 100);
		
		$(".spin_img button").click(function(){
			$(".spin_img img").toggleClass("a");
			if($(".spin_imgbutton").text()=="start"){
				$(".spin_imgbutton").text("stop");
			}else{
				$(".spin_imgbutton").text("start");
			}
		});
	}

	function showa(){ $("#img1").show(500); }
	function hidea(){ $("#img1").hide(300); }
	function fadeina(){ $("#img1").fadeIn(2000, function(){ alert("fadeIn"); }); }
	function fadeouta(){ $("#img1").fadeOut(2000); }
	function fadetoa(){ $("#img1").fadeTo(1000, 0.5); }
	function fadeToggles() { $("#img1").fadeToggle(); }
	load();
</script>
<style type="text/css">
.spd {
font: bold;
background-color: olive;
font-size: x-large;
}

.toggledivs div { background-color:cyan; width:100px; border : 1px dashed blue; }


.spin_img img{ width: 150px; height: 150px; margin:0; padding:0; }
.spin_img #m { display: flex; }
.spin_img .sel { width: 140px; height: 170px; margin-left: 300px; border:6px dashed blue; text-align:center; position:absolute; }
.spin_img button { width: 100px; height: 60px; margin-left: 330px; top:200px; }
</style>

<div class="div1" onLoad="load">
	<p id="a"> 1. id seletor</p>
	<p id="a"> 1. id seletor 아이디는 하나만 인정됨</p> <!-- 아이디와 클래스의 차이점 -->
	<p class="b"> 2. class seletor</p>
	<p class="b"> 2. class seletor 클래스는 같은 이름이면 다같이 적용됨</p>
</div>
<div id="c">
	<p>3. 자식 엘리먼트 선택</p>
	<span>3. 자식 엘리먼트 선택-클릭하면 색 변경됨</span>
</div>
<div id="d">
	<p>4. 자손 엘리먼트 선택<span>자손 엘리먼트</span></p>
</div>
<div class="div4">
	<p>5. 엘리먼트의 내용으로 탐색</p>
	<p>붕어빵</p>
	<p st="aa">홍길동</p>
</div>
<div class="div5">
	<p>6. nth-child(n(숫자)/odd(홀수)/even(짝수)) 숫자를 입력하면 해당 숫자에 대한 내용에만 적용됨.</p>
	<p>6. nth-child(n/odd(홀수)/even(짝수))</p>
	<p>6. nth-child(n/odd(홀수)/even(짝수))</p>
	<p>6. nth-child(n/odd(홀수)/even(짝수))는 인덱스가 기본적으로 1부터 시작함</p>
	<p>6. nth-child(n/odd(홀수)/even(짝수))</p>
</div>


<div>
	<input type="button" value="show" onclick="showa()">
	<input type="button" value="hide" onclick="hidea()">
	<input type="button" value="fade in" onclick="fadeina()">
	<input type="button" value="fade out" onclick="fadeouta()">
	<input type="button" value="fade to" onclick="fadetoa()">
	<input type="button" value="fade toggle" onclick="fadeToggles()">
</div>

<div class="toggledivs">
	<div class="toggletest">
		이건 사라지는거..
	</div>
	<div class="toggletest" style="display: none;">
		이건 나타나는거..
	</div>
	<button id="startToggle">button</button>
	<div id="smalldiv">커지는 DIV 태그</div>
</div>


<div class="spin_img">
	<div class="sel"></div>
	<div id="m">
		<img src="https://media.istockphoto.com/id/1489790715/ko/%EC%82%AC%EC%A7%84/%EC%95%84%EB%A6%84%EB%8B%A4%EC%9A%B4-%EB%85%B9%EC%83%89-%EA%B3%84%EA%B3%A1%EC%9D%98-%EC%B4%88%EC%9B%90%EA%B3%BC-%EC%86%8C%EB%B0%95%ED%95%9C-%EB%8F%8C-%EC%98%A4%EB%91%90%EB%A7%89.jpg?s=1024x1024&w=is&k=20&c=_qUCQgoHPy0ax6BqnI2BxoOeWV2H0wYESX_oasPOkFY=" alt="0">
		<img src="https://media.istockphoto.com/id/1598689212/ko/%EC%82%AC%EC%A7%84/%EC%82%AC%EB%A7%89%EC%9D%98-%EB%8B%AC-%EC%A7%80%EA%B5%AC-%EC%98%A8%EB%82%9C%ED%99%94-%EA%B0%9C%EB%85%90.jpg?s=1024x1024&w=is&k=20&c=n9civPmi4qep9FM-YEBRppV6J4MIeVYInRspJEc3C6M=" alt="1">
		<img src="https://cdn.pixabay.com/photo/2015/06/19/21/24/avenue-815297_1280.jpg" alt="2">
		<img src="https://cdn.pixabay.com/photo/2017/12/15/13/51/polynesia-3021072_1280.jpg" alt="3">
		<img src="https://cdn.pixabay.com/photo/2016/12/11/12/02/mountains-1899264_1280.jpg" alt="4">
	</div>
	<br>
	<button>start</button>
</div>

<!-- markdownlint-restore -->