---
layout: post
---

# classic ASP



```
response.charset="utf-8"
'-------------------------------------------------
'	날짜 확인 테스트
'-------------------------------------------------
response.write "Now() ==> " & Now() & "<br>"
response.write "Date() ==> " & Date() & "<br>"
response.write "Time() ==> " & Time() & "<br>"

response.write "<hr>"

response.write mid(FormatDateTime(Now(), 2), 6,2)
response.write "<br />"
response.write right(FormatDateTime(Now(), 2), 2)
response.write "<br />"
response.write left("201809150923", 8)
response.write "<br />"


response.write CDate("2018-10-04 00:00:000")
response.write Month(FormatDateTime("2018-11-22", 2))&"-"&Day(FormatDateTime("2018-11-22", 2))
response.write "<br />"
response.write "FormatDateTime('2018-12-06 12:00:00', 0) ==> " & FormatDateTime("2018-12-06 12:00:00", 0) & "<br>"

response.write "<hr>"

response.write "Now() ==> " & Now() & "<br>"
response.write FormatDateTime("2018-12-06 13:57:00") & "<br>"
response.write (cdate("2018-12-06 13:57:00") < Now()) & "<br>"
 
response.write (cdate("2018-12-06 14:30:00") < Now()) & "<br>"

response.write "<hr>"

response.write "FormatDateTime(Now(), 0) ==> " & FormatDateTime(Now(), 0) & "<br>"
response.write "FormatDateTime(Now(), 1) ==> " & FormatDateTime(Now(), 1) & "<br>"
response.write "FormatDateTime(Now(), 2) ==> " & FormatDateTime(Now(), 2) & "<br>"
response.write "FormatDateTime(Now(), 3) ==> " & FormatDateTime(Now(), 3) & "<br>"
response.write "FormatDateTime(Now(), 4) ==> " & FormatDateTime(Now(), 4) & "<br>"
response.write "-----------------------" & "<br>"

response.write "FormatDateTime(Now(), 4) ==> " & FormatDateTime(Now(), 4) & "<br>"
response.write "Right(Now(), 3) ==> " & Right(Now(), 3) & "<br>"
 
response.write "====> " & FormatDateTime(Now(), 2) & " " & FormatDateTime(Now(), 4) & Right(Now(), 3) & "<br>"


'-------------------------------------------------
'	시간계산 테스트
'-------------------------------------------------
Dim mm 
'mm = 59-CInt(Minute(now()))
mm = 59-51

Dim timeNum
timeNum = 23 - CInt(hour(now())) & mm

if len(Cstr(mm)) < 2 then
  timeNum = 23 - CInt(hour(now())) & "0" & mm
end if 

response.write timeNum
```


결과  
```
Now() ==> 2019-01-08 오후 5:26:16
Date() ==> 2019-01-08
Time() ==> 오후 5:26:16
_______________________________________________

01
08
20180915
2018-10-0411-22
FormatDateTime('2018-12-16 12:00:00', 0) ==> 2018-12-06 오후 12:00:00
_______________________________________________

Now() ==> 2019-01-08 오후 5:26:16
2018-12-06 오후 1:57:00
True
True
_______________________________________________

FormatDateTime(Now(), 0) ==> 2019-01-08 오후 5:26:16
FormatDateTime(Now(), 1) ==> 2019년 1월 8일 화요일
FormatDateTime(Now(), 2) ==> 2019-01-08
FormatDateTime(Now(), 3) ==> 오후 5:26:16
FormatDateTime(Now(), 4) ==> 17:26
------------------
FormatDateTime(Now(), 4) ==> 17:26
Right(Now(), 3) ==> :16
====> 2019-01-08 17:26:16
608
```
