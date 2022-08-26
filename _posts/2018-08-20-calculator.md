---
layout: post
title: 스크립트 계산기
---

# 스크립트 계산기



<style type="text/css">

table tr td { text-align: center; border-width: 0px; }
input { width: 40px; height: 40px;  font-size: x-large; background-color: infobackground; font-family:AR DESTINE; }
#aaa { width: 200px; height: 30px; }

</style>
<script type="text/javascript">
var txt1="";
	function gjr(val){
		var abc=document.forms[0].aaa.value;
		txt1=abc+val.value;
		//var abc2=eval(txt1);
		document.forms[0].aaa.value=txt1;
		txt1=document.forms[0].aaa.value;
	}
	function gjf2(){
		txt1=document.forms[0].aaa.value;
		var qqq=txt1.substring(0, txt1.length-1);
		document.forms[0].aaa.value=qqq;
		alert("??");
	}
	function gjf3(){
		txt1=document.forms[0].aaa.value;
		var abc=eval(txt1);
		document.forms[0].aaa.value=txt1+"="+abc;
		txt1="";
	}
	function cencle(){
		document.forms[0].aaa.value="";
		txt1="";
	}
	
</script>


<form onkeydown="enter1()">
  <table border="1" width="250" height="300" align="center" background="image/angular_study2.JPG" bordercolor="pink">
    <tr>
      <td colspan="4">
        <input type="text" id="aaa" size="30" onclick="cencle()">
      </td>
    </tr>
		<tr>
			<td><input type="button" value="7"  onclick="gjr(this)"></td>
			<td><input type="button" value="8"  onclick="gjr(this)"></td>
			<td><input type="button" value="9"  onclick="gjr(this)"></td>
			<td><input type="button" value="+"  onclick="gjr(this)"></td>
		</tr>
		<tr>
			<td><input type="button" value="4" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="5" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="6" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="-" size="3" onclick="gjr(this)"></td>
		</tr>
		<tr>
			<td><input type="button" value="1" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="2" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="3" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="*" size="3" onclick="gjr(this)"></td>
		</tr>
		<tr>
			<td><input type="button" value="=" size="3" onclick="gjf3(this)"></td>
			<td><input type="button" value="0" size="3" onclick="gjr(this)"></td>
			<td><input type="button" value="&larr;" size="3" onclick="gjf2()" ></td>
			<td><input type="button" value="/" size="3" onclick="gjr(this)"></td>
		</tr>
	</table>
</form>