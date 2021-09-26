---
layout: post
---

# ASP.NET


* Eval 메서드 사용  
Eval 메서드는 GridView, DetailsView 및 FormView와 같은 데이터 바인딩된 컨트롤의 템플릿에서 런타임에 바인딩된 데이터 식을 계산합니다.  
런타임에 Eval 메서드는 DataBinder 개체의 Eval 메서드를 호출하여 명명 컨테이너의 현재 데이터 항목을 참조합니다.  
일반적으로 명명 컨테이너는 GridView 컨트롤의 행처럼 전체 레코드가 포함된 데이터 바인딩된 컨트롤의 최소 부분입니다.  
따라서 데이터 바인딩된 컨트롤의 템플릿 안에서 바인딩할 경우에만 Eval 메서드를 사용할 수 있습니다.  
Eval 메서드는 데이터 필드의 이름을 사용하고 데이터 소스의 현재 레코드에서 해당 필드의 값을 포함하는 문자열을 반환합니다.  
선택적 요소인 두 번째 매개 변수를 제공하여 반환 문자열의 형식을 지정할 수 있습니다.  
문자열 형식 매개 변수는 String 클래스의 Format 메서드에 정의된 구문을 사용합니다.  

* Bind 메서드 사용  
Bind 메서드는 Eval 메서드와 비슷하지만 몇 가지 중요한 차이점이 있습니다.  
Eval 메서드와 마찬가지로 Bind 메서드를 사용하여 데이터 바인딩 필드의 값을 검색할 수 있지만 Bind 메서드는 데이터를 수정할 수 있는 경우에도 사용됩니다.  
ASP.NET의 경우 GridView, DetailsView, FormView와 같은 데이터 바인딩된 컨트롤에서 데이터 소스 컨트롤의 업데이트, 삭제 및 삽입 작업을 자동으로 사용할 수 있습니다.  
예를 들어 데이터 소스 컨트롤에 대해 SQL Select, Insert, Delete 및 Update 문을 정의한 경우 GridView, DetailsView   
또는 FormView 컨트롤 템플릿에서 Bind 메서드를 사용하면 템플릿의 자식 컨트롤에서 값을 추출하여 데이터 소스 컨트롤에 전달할 수 있습니다.  
그러면 데이터 소스 컨트롤에서 데이터베이스에 대해 적절한 명령을 수행합니다.이러한 이유 때문에 Bind 함수는 데이터 바인딩된 컨트롤의 EditItemTemplate 또는 InsertItemTemplate 안에서 사용됩니다.  
일반적으로 Bind 메서드는 편집 모드에서 GridView 행에 의해 렌더링되는 TextBox 컨트롤 같은 입력 컨트롤과 함께 사용됩니다.  
데이터 바인딩된 컨트롤에서 렌더링하여 이러한 입력 컨트롤이 만들어지면 입력 값을 추출할 수 있습니다.  
다음 예제와 같이 Bind 메서드는 바인딩된 속성에 연결할 데이터 필드의 이름을 사용합니다.  

행의 Update 단추를 클릭하면 Bind 구문을 사용하여 바인딩된 각 컨트롤 속성의 값이 추출되어 업데이트 작업을 위해 데이터 소스 컨트롤에 전달됩니다.  


#### 배열, 리스트 차이점  
- 배열의 장점 : 검색속도가 빠르다.(인덱스 번호를 이용하여 곧바로 해당 위치에 접근가능), 크기 고정으로 인한 메모리 낭비 감소
- 리스트의 장점 : 크기를 자유자재로 변경할 수 있다.(유연성) 제공하는 메서드와 함수가 다양하다
- 데이터 처리에 따른 배열과 리스트의 사용
 1) 새로운 데이터의 삽입 시  
    -삽입할 위치에 있던 기존의 데이터를 그냥 덮어 쓰는 경우   - 배열이 좋다  
    -삽입 위치의 데이터와 그 이후의 데이터를 한 칸씩 뒤로 밀어서 삽입할 공간을 만들어야 하는 경우   -리스트가 좋다  
 2) 기존 데이터의 삭제 시  
    -삭제되고 남아 있는 공간을 그대로 두는 경우   -배열이 좋다  
    -삽입 위치의 데이터와 그 이후의 데이터를 한 칸씩 뒤로 밀어서 삽입할 공간을 만들어야 하는 경우   -리스트가 좋다  


#### IEnumerable을 상속받는 개체는 무엇이 있는지와 어떻게 쓰는지?
- 제네릭이 아닌 컬렉션에서 단순하게 반복할 수 있도록 지원하는 열거자를 노출합니다.(내부의 데이터를 순방향으로 접근할 수 있도록 해줌)
- 컬렉션을 반복하는 열거자를 반환합니다. 반환 값: 컬렉션을 반복하는 데 사용할 수 있는 System.Collections.IEnumerator 개체입니다.
- foreach문에 쓸 수 있는 배열이나 리스트같은 컬렉션에 상속받아 사용
- GetEnumerator()메서드로 상속받는 해당 객체를 반환받으면 IEnumerator인터페이스의 메소드(MoveNext, Reset, Current)로 해당 객체를 사용할 수 있다

** IEnumerator : 
IEnumerable이 내부의 데이터를 순방향으로 접근할 수 있도록 세팅해 놓고 GetEnumerator()메소드로 IEnumerator를 불러 아래와 같은 요소를 사용하여
원하는 데이터로 접근하는 방식으로 사용된다.  
MoveNext : 다음 요소로 이동 / Reset : 컬렉션의 첫 위치(앞)으로 이동) / Current : 컬렉션의 현재요소  


#### 구조체
- 클래스는 힙에 생성되는 참조 타입(Reference Type)이고, 구조체는 스택에 생성되는 값 타입(Value Type)이라는 것이죠.
- 값 형식이고 힙 할당을 필요로 하지 않는다. 구조체 데이터를 직접 저장하는 방식. 재사용이 불가능.
  ref와 out 매개변수를 제외하고, 구조체를 복사하는 것은 값을 복사하는 것과 같음(객체 참조를 복사하는 것보다 덜 효율적)
  구조체에 대한 참조를 생성하지 못하므로 구조체의 활용을 배제하는 결과.
  생명주기가 곧 해당 사용 페이지와 같음.


#### 제네릭이란?
- 타입을 미리 정하고 정한 타입만 대입되도록 만들어 다른 타입이 저장되지 못하게 하거나, 언박싱이나 캐스팅이 필요하지 않게 하는 것.
- 클래스에 사용할 타입을 디자인할 때 지정하는 것이 아니라, 클래스를 사용할 때 지정한 후 사용하는 기술을 말한다.
- 제네릭을 쓰는 이유 
  (형식 안정성 때문이다./캐스팅으로 인한 낭비를 하지 않아도 된다.(만약 object 타입으로 초기에 프로그래밍 해 놓았다면 일일히 string, int형 캐스팅 해야한다는 불편함이 존재한다,/
   박싱과 언박싱으로 인한 부하가 없다. )

#### ViewState란?
- ASP.NET 페이지 프레임워크에서 페이지를 렌더링하기 직전에 페이지와 각 컨트롤 값을 자동으로 저장할 때 사용되는 방법 중 하나로
  한 페이지에 대한 여러개의 요청을 서버 컨트롤의 뷰 상태를 복원하고 저장할 수 있습니다  상태 정보 사전에 가져옵니다.
  각각의 페이지마다 viewstate 내용이 다르며 ViewStateGUID의 키 값에 내용을 입력하여 소스에 보이지않게 할 수 있다(보안성)

- 사용법
ViewState["상태유지"] = "상태값"; //상태값 저장하기 Boxing  
string str상태유지 = (string)ViewState["상태유지"]; //상태값 가져오기 UnBoxing  

- 장점
server에 저장해서 읽어오는것 보다 유지 활용성이 좋으나 내용이 많을경우 웹페이지 다운로드가 더딤

** 뷰 상태 복원 : 페이지가 게시될 때 페이지 처리에서 처음 수행되는 작업

#### 시리얼라이즈(Serializable)

- 객체를 저장할 수 있도록 변경시킴(직렬화?)
- 메모리 구조가 바이트로 나열되있는데 메모리나 저장공간에 담을 수 있도록 일렬로 만드는 뜻,
  저장하거나 상대방에게 전송하기 위해 일렬로 만드는 의미
- 개체의 상태를 저장소에 보존 했다가 나중에 똑같은 복사본을 다시 만들거나 한 응용프로그램 에서 다른 응용프로그램으로 개체를 전송하기 위해서 사용한다.

- 장점
DOM을 이용하지 않고 XML  문서를 수정할 수 있다.   
어떤 어플리케이션으로부터 다른 어플리케이션으로 객체를 전달할 수 있다.   
어떤 도메인으로부터 다른 도메인으로 객체를 전달할 수 있다.   
객체를 XML 문자열의 형태로 방화벽을 거쳐(통과해) 전달할 수 있다(방화벽에 제약을 받지 않고 객체를 전달할 수 있다).  
 
- 단점
 직렬화와 역직렬화 과정에서 자원(CPU와 IO 장치)의 소모가 많다.  
 네트워크를 통해 객체를 전달할 경우 지연 문제가 발생할 수 있다(직렬화 과정은 상당히 느리다).  
 XML로 직렬화하는 것은 안전하지 않으며(XML 문서의 형태로 저장할 경우 다른 프로그램이나 사용자가 실수나 고의로 문서의 내용을 변경할 수도 있다.)  
 많은 저장 공간을 차지하고, public 클래스에 대해서만 직렬화가 이루어질 수 있으므로 private 클래스 또는 internal 클래스는 직렬화할 수 없다
(프로그래머로 하여금 직렬화를 지원하는 클래스를 만들기 위해 불필요한 public 클래스를 양산할 수도 있다).

#### ViewState, file, Application Cache
- 웹에서는 상태를 유지하기위한 여러가지 방법들을 사용합니다. 그 예로는 Application, Session, Cookie, ViewState 등등.. 이외에도 여러가지 방법들이 있습니다.  
  VisualStudio(이하 VS)에서 사용하는 서버 컨트롤 들은 기본적으로 상태유지를 위해 ViewState를 사용합니다.  
  사용자가 따로 설정하지 않아도 자체적으로 ViewState에 상태값을 저장해두었다가 불러 사용합니다.  

- ViewState : 각각의 특정 페이지에만 적용되며 ViewState["키"]="값" 형태로 저장된다(지역변수 개념과 비슷-생명주기는 폼이 로드될때)
- file : 유지하고자 하는 상태를 파일로 저장하여 사용하며 임의로 삭제하지 않을때까지 살아있다
- Application Cache : Application["키"]="값" 형태로 저장되며 첫번째 사용자가 접근하면 서버측의 메모리에 Application메모리가 생성되고 다음 사용자가 접근하면 생성된 메모리를 공유해서 사용함.(전역변수의 개념과 비슷-생명주기는 웹브라우저가 닫히기 전까지)

- 생명주기 순 (긴것>짧은것)  
  file > Application Cache > ViewState   
  특별한 내용(중요내용)이 담기지 않은 전자도서?,백과사전?,뉴스?,일반웹페이지화면>접속된 사용자가 공유해도 괜찮은 내용을 담는 인트라넷,그룹웨어?>로그인,개인정보를 submit할때?  


#### 어플리케이션
- 응용프로그램상태(Aoolication State) : 웹 응용 프로그램 전역(모든 페이지)에서 접근할 수 있으며, 모든 세션(논리적 연결)이 공유하는 정보 저장공간  
웹 서버의 메모리를 사용 : 데이터베이스에서 정보를 가져오는것보다 빠름  
정적(static)이며, 용량이 적고 자주 사용되는 데이터 집합을 저장해 두고 사용하면 사이트의 성능 향상  
스레드 안정성(Thread Safety) 비보장 : 다수의 세션이 동시에 응용프로그램 상태에 저장된 정보를 저장하거나 변경시 HttpApplicationState  클래스의 Lock(), UnLock()메서드를 사용해서 충돌을 피해야 함
사용예) 접속한 사용자 수 / 투표자 수 ...  


#### state 유지
{{< bootstrap-table table_class="table table-dark table-striped table-bordered" >}}
| 유지방법     | 자료요구     | 유지기간        | 유지 데이터 용량   |
| :---------: | :----------- | :-------------- | :---------------- |
| Application | 모든 사용자             | 다음 에플리케이션이 재시작할 때까지 | 임의의 크기, 한번만 설정 가능 |
| Cookie      | 사용자                  | 원하는 만큼 짧게\n또는 사용자가 쿠키를 지우지 않는 동안 | 최소량, 간단한 데이터 |
| Form Post   | 사용자                  | 다음 요청까지만\n(많은 요청들을 지난 후 재사용 가능) | 사실상 임의의 크기, 데이터가 모든 페이지 어디든 보내짐 |
| QueryString | 사용자\n사용자 그룹     | 다음 요청까지만\n(많은 요청들을 지난 후 재사용 가능) | 최소량, 간단한 데이터 |
| Session     | 사용자                  | 사용자가 액티브 상태인 동안,\n timeout 기간을 추가함 | 임의의 크기, 사용자마다 개별 세션 스토어를 가지므로 최대한 최소화 필요 |
| Cache       | 모든 사용자\n사용자 일부 | 필요한 만큼 길거나 짧게 | 크거나 작게 사용가능, 단순하거나 복잡한 데이터 |
| Context     | 사용자                  | 요청시 | 큰 개체 수용가능, 모든 요청에 사용되므로 일반적으로 사용 자제 |
| ViewState   | 사용자                  | 하나의 웹폼 페이지 | 최소량, 모든 페이지 어디든 보내질 수 있음 |
| Config file | 모든 사용자             | configuration 파일이 수정될 때까지 | 많은 데이터 수용가능, string 혹은 xml로 구성 |
{{< /bootstrap-table >}}



#### string.Compare(비교문자열1,비교문자열2,true)  
문자열을 대소문자 구분없이 비교하여 int형으로 값 반환 (일치:0, 크다:1, 작다:-1)  


#### checked/unchecked
```
int i = int.MaxValue;
checked
{
   Response.Write(i + 1);	// Exception
}
unchecked
{
   Response.Write(i + 1);	// Overflow
}
checked 구문에서 오버플로우 에러발생, unchecked 구문에서는 오버플로우 산술 결과를 반환
checked/unchecked 문은 정수 계열 형식 산순 연산과 변환에 대한 오버플로우 검사환경을 제어하는데 사용
```

#### 클래스와 구조체
```
class Point  //클래스
{
    public int x, y;
     public Point(int x, int y)
     {
         this.x = x;
         this.y = y;
     }
}

struct Point2  //구조체
{
    public int x, y;
    public Point2(int x,int y)
    {
        this.x = x;
        this.y = y;
    }
}

Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Point2 aa = new Point2(10, 10);
Point2 bb = aa;
aa.x = 20;
Response.Write(b.x.ToString());   // 클래스 결과: 20
Response.Write(bb.x.ToString());  // 구조체 결과: 10
```

#### lock문은 주어진 객체에 대해 상호 배제 잠금을 얻어 구문을 실행하고 나서 그 잠금을 푸는데 사용
```
public void 메소드명()
{
	lock (객체명)
	{
	   ... 실행 구문들
	}
}

여러개의 스레드가 하나의 메소드를 실행할때 lock으로 해당 객체 우선실행 후 차례를 넘김
```

#### yield문은 호출자에게 데이터를 하나씩 리턴할때 사용.
```
static IEnumerable<int> Range(int from, int to)
{
   for (int i = from; i < to; i++)
   { 
       yield return i;
   }
   yield break;
}

foreach (int x in Range(-10,10))
{
    Response.Write(x+"<br>");
}
```


#### 네이밍 가이드라인
1. Private Fields : "_" Prefix와 헝가리안 표기법
2. 지역변수 : 헝가리안 표기법
3. Namespace : 회사명과 기술명을 이용
4. Class : Prefix를 사용하지 않는다 "_"를 사용하지 않는다
5. Interface : "I" Prefix로 인터페이스임을 명시 "_" 사용하지 않는다
6. Parameter : "p" Prefix로 지역변수와의 충돌 방지
7. Method : 동사를 이용한다
8. Property/Enumerations : 헝가리안 표기법을 사용하지 않는다
9. Event : EventHandler 클래스에 "EventHandler" Suffix, EventArgument 클래스에 "EventArgs" Suffix
10. Exception : 비주얼스튜디오 닷넷에서 기본제공되는 "e" parameter와의 충돌 방지를 위해 "ex" 사용
11. Constant : "_"로 구분하며 의미별로 그룹화한다


#### overload 패턴
```
public void TestOverLoad(string a)
{
    TestOverLoad(a, null, null);
}

public void TestOverLoad(string a, string b)
{
    TestOverLoad(a, b, null);
}

public void TestOverLoad(string a, string b, string c)
{
    a += "1";
    if ( b !=null )
    {
        
    }
}
```

#### 웹 게시(Web Deploy)

1. Web Deploy : 네트워크를 통해 IIS 서버에 직접 업로드  
 IIS 서버와 네트워크로 반드시 연결되어 있어야 하며 웹 게시(Web Deploy)에 대응한 설정을 해두어야 한다  
 외부 IP 혹은 도메인이 존재한다면 인터넷을 통한 업로드도 가능하다  
2. Web Deploy 패키지 : Web Deploy와 비슷하나 IIS 서버와 네트워크로 연결되어 있을 필요가 없으며  
 Zip 파일로 압축되어 튀어나온 파일을 IIS 서버에 가져다가 IIS 서버의 웹 게시 마법사를 이용해 받아들이면 된다  
3. FTP : FTP를 통해 업로드. IIS에 FTP 서버 역할이 추가되어 있어야 하며 추가적인 설정이 필요하다  
4. 파일 시스템 : Web Deploy 패키지와 비슷하나   
 Web Deploy 패키지를 이용한 배포는 여러가지 ADO.NET 혹은 Entiti Framework에 대응할 수 있지만 파일 시스템을 그렇지 않다  
 일일이 수작업으로 수정해줘야 한다.  


#### visual studio 단축키
```
F9      현재 라인에 Breakpoint를 지정/해제 
Ctrl + Shift + F9    현재 Edit하고 있는 소스파일에 지정된 모든 Breakpoint 해제 
Ctrl + ]     '{'괄호의 짝을 찾아줌 ('{'에 커서를 놓고 눌러야 함} 
Ctrl + J, K     #ifdef 와 #endif의 짝을 찾아줌 
Ctrl + L     한 라인을 클립보드로 잘라내기 (Cut)  
Ctrl + Shift + L    한 라인을 삭제 
Alt + Mouse     블록 설정 세로로 블록 설정하기 (마우스로) 
Ctrl + Shift + F8    세로로 블록 설정하기 (키보드로), 취소할 때는 Esc키를 눌러야 함 
블록 설정 -> Tab    선택된 블록의 문자열을 일괄적으로 들여쓰기(Tab) 적용 
블록 설정 -> Shift + Tab   선택된 블록의 문자열을 일괄적으로 내어쓰기 적용 
Alt + F8    인덴트 정리. 범위 선택 후 사용하면 해당 범위를 표준 인덴트로 바꾸어줌.
Shift + F9    디버그 모드에서 해당 변수를 바로 Watch Window에 등록.
Ctrl + U    선택된 영역을 소문자로 바꿈
Ctrl + Shift + U    선택된 영역을 대문자로 바꿈 
Ctrl + Shift + 8    문단기호 표시/감추기 : Tab은 ^, Space는 .으로 표시 
Ctrl + D     툴바의 찾기 Editbox로 이동 
Ctrl + Up/Down Arrow    커서는 고정시키고 화면만 스크롤 시키기 
Shift + Alt + 커서 이동
Alt + 마우스 드래그 세로로 영역 선택
Shift + F12    선언으로 이동<?XML:NAMESPACE PREFIX = O /><?XML:NAMESPACE PREFIX = O />

=== 찾 기 ===
Ctrl +F3 현재커서의 단어 찾기
Ctrl +D 툴바의 찾기 Editbox로 이동
Ctrl + I 문자열 입력 점진적으로 문자열 찾기 (Incremental Search)
Ctrl + Shift + F3    현재 커서에 있는 문자열 찾기 backward 
SHIFT + ALT + O 프로젝트에 있는 파일 찾기 ( 비주얼 어시스트)
Alt + M 파일에서 method의 리스트를 보여준다.
Ctrl + ] '{}'괄호, #ifdef, #endif 의 짝을 찾아줌
F3      찾은 문자열에 대한 다음 문자열로 이동 (Next Search)
Ctrl + H     문자열 찾아 바꾸기 (Replace) 

=== 이동 관련 ===
CTRL + PGDOWN (or END) 문서 끝
CTRL + PGUP (or HOME) 문서 처음
F12 선언부로 가기
Ctrl + F2 현재 라인에 북마크 지정/해제
F2 지정된 다음 북마크로 이동
Ctrl + Shift + F2 지정된 모든 북마크를 해제
함수간 이동

=== 주석처리 ===
Ctrl+K, Ctrl+C 선택 영역 주석 처리 (.NET 2003, 2005)
Ctrl+K, Ctrl+U 선택 영역 주석 없앰 (.NET 2003, 2005)

=== 아웃라인 ===
Ctrl+M, Ctrl+L 모든 아웃라인 보이기/숨기기 (Edit.ToggleAllOutlining)
Ctrl+M, Ctrl+M 현재 아웃라인 보이기/숨기기 (Edit.ToggleOutliningExpansion)
Ctrl+M, Ctrl+H 선택영역 아웃라인 지정(Edit.HideSelection)
Ctrl+M, Ctrl+U 현재 아웃라인 삭제 (Edit.StopHidingCurre
Ctrl+M, Ctrl+P 모든 아웃라인 삭제(Edit.StopOutlining Text Editor)

=== 기 타 ===
ALT + F7 프로젝트 속성
Shift+Alt+Enter : 전체화면 토글
Ctrl + Shift + F9 현재 Edit하고 있는 소스파일에 지정된 모든 Breakpoint 해제

디버그 모드에서 Watch Window에서 추가하고픈 변수나 등등 앞에 커서를 위치 시킨후 Shift + F9

☆☆ Studio 단축키 ☆☆


일반 단축키
- 모두 저장 : Ctrl + Shift + S
- 문서창 닫기 : Ctrl + F4
- 다음 문서 : Ctrl + F6, 이전 문서 : Ctrl + Shift + F6
- 다음 도구 : Alt + F6
- 들여 쓰기 : Teb, 내어 쓰기 : Shift + Teb
- 주석 달기 : Ctrl + E + C, 주석 해제 : Ctrl + E + U
- 파일에서 찾기 : Ctrl + Shift + F, 중단 : Alt + F3, S
- 찾기 이전으로 : Ctrl + F3, 다음으로 : Shift + F3
- 증분검색 정방향 : Ctrl + I, 역방향 : Ctrl + Shift + I
- 문서 끝 : Ctrl + End, 시작 : Ctrl + Home, 행 이동 : Ctrl + G
- 자동 줄바꿈 : Ctrl + E, W
- 공백 보기 : Ctrl + E, S,   가로 공백 삭제 : Ctrl + E, \
- #region 펼치기 & 접기 : Ctrl + M, M
- 클립보드에 복사 : Ctrl + Shift + Num
- 클립보드링 순환 : Ctrl + Shift + Insert

디버그시 단축키
- 직접실행창 표시 : Ctrl + Alt + I
- 모든 중단점 지우기 : Ctrl + Shift + F9
- 중단점 추가 : Alt + F9
- 중단점 설정/해지 : F9
- 프로그램에서 사용하는 모든 모듈 보기 : Ctrl + Alt + U
- 간략한 조사식 : Ctrl + Alt + Q, Shift + F9
- 디버그 다시 시작 : Ctrl + Shift + F5
- 디버그 커서까지 실행 : Ctrl + F10
- 프로시져 단위 디버그 : F10
- 한단계씩 디버그 : F11
- 프로시져 나가기 : Shift + F11
- 디버깅 중지 : Shift + F5
- 디버그 커서까지 실행 : Ctrl + F10
- 디스어셈 플리 설정/해제 : Ctrl + F11
- 디버그 조사식 1,2,3,4 : Ctrl + Alt + W, 1, 2, 3, 4
- 디버그 하지 않고 시작 : Ctrl + F5

DataBase 단축키
- 데이터베이스 선택영역 실행 : Ctrl + R, Ctrl + D
- 데이터 베이스 한 단계씩 실행 : Ctrl + D, Ctrl + S
- SQL 선택영역 실행 : Ctrl + R

도구창
- 책갈피 창 : Ctrl + W, B
- 책갈피 지정 : Ctrl + B, T
- 책갈피 이전 : Ctrl + B, P  -- 다음 : Ctrl + B, N
- 모든 책갈피 지우기 : Ctrl + B, C
- 매크로 창 : Alt + F8
- 매크로 기록 : Shift + Ctrl + R
```


* 컴파일과 인터프리터 : 컴파일러는 소스코드를 컴파일하여 실행파일로 만드는것, 인터프리터는 소스코드를 실시간으로 기계어로 해석해서 실행해줌

* 네임스페이스(namespace) : 성격이나 하는 일이 비슷한 클래스, 구조체, 인터페이스, 델리게이트, 열거형식 등을 하나의 이름아래 묶음
                            다른 네임스페이스에서 해당 네임스페이스를 참조하기 위해선 using 네임스페이스명; 을 해야합니다.

* Main 메소드 : 프로그램의 진입점(Entry Point)로 프로그램을 시작하면 실행되며, 모든 프로그램은 메인메소드를 가지고 있어야 한다

* CLR(Common Language Runtime): C# 뿐 아니라 CLS(Common Language Specification)의 규격을 따르는 모든 언어로 작성된 프로그램 지원하는 실행되는 환경  
IL(Intermediate Language)을 OS가 이해할 수 있는 네이티브코드로 컴파일 해줌  
(=JIT 컴파일 : Just In Time : 적시컴파일 : 실행할때마다 실시간으로 컴파일해서 실행)  

* C# 컴파일러 : C# 코드를 IL(Intermediate Language)라는 중간언어로 바꿔줌

* 값 형식과 관련있는 스택(stack) : 데이터가 저장되어있는 힙 메모리의 주소 저장, 코드블록 안에서 생성된 모든 변수들은 코드블록이 끝나는 동시에 메모리에서 제거됨

* 참조값과 관련있는 힙(heap) : 실제 값을 저장, 더이상 사용하지 않는 객체를 정리해주는 가비지 컬렉터(Garbage Collector) 존재

* 박싱(boxing)과 언박싱(nuboxing) : 박싱은 object 형식에 값 데이터를 힙에 할당(object a=20;), 힙에 있던 값 형식 데이터를 값 형식 객체에 다시 할당할땐 언박싱(int b=(int)a;)

* var : 데이터 형식을 var로 지정하면 들어오는 값에 따라 데이터 형식이 자동으로 지정됨. 해당 키워드는 지역변수에서만 사용되며 사용시 반드시 초기값을 넣어주어야 형식이 자동지정된다

* 메서드(Method)=함수(Function)=프로시져(Procedure)

* LINQ(Language Integrate Query) : 통합 질의 언어, 컬렉션 형태로 되어있는 도든 데이터에 대해 질의를 할 수 있는 마이크로스프트의 기술 (MS-SQL데이터 가져오기, 메모리상의 컬렉션, XML에 대해서도 사용가능)

* IIS(Internet Information Services) : 마이크로소프트 인터넷 정보 서비스(Internet Information Services, IIS)는마이크로소프트 윈도를 사용하는 서버들을 위한 인터넷 기반 서비스들의 모임  
전반적인 웹사이트 용어에서, 아파치 웹 서버에 이어 세계에서 두 번째로 가장 잘 알려진 웹 서버이다.  
2007년 10월 기준으로 전 세계 웹사이트의 37.13%와 전 세계 활성화 웹사이트들의 38.23%가 인터넷 정보 서비스를 사용  
서버는 현재 FTP, SMTP, NNTP,HTTP/HTTPS를 포함하고 있다  

* SSL (Secure Socket Layer) : 어플리케이션 계층과 TCP 사이에 놓인 계층으로 전송 데이터를 암호화하는 프로토콜. (웹브라우저와 웹서버 사이에 보안 접속 통신을 말하며 공개키, 개인키 방식으로 암호화)

* LazyLoading (지연로딩=Lazy) : 객체가 필요로하는 시점까지 초기화를 지연하고 있다가 최초 사용하게 되는 시점에 객체를 초기화하여 사용. 개체를 처음으로 참조할 때까지 개체 초기화 또는 인스턴스화를 미루는 것을 말합니다

* DeferredLoading (즉시로딩=Eager) : 객체를 사용하는 시점에 바로 초기화. 쿼리에서 명시적으로 요청된 개체와 함께 특정한 관련 개체 집합이 로드되는 로드 패턴입니다


* ?? 연산자(물음표두개) : ??를 기준으로 좌측값이 null이면 우측값을 null이 아니면 좌측값을 반환한다. "기준값 == null ? 좌측값 : 우측값" 과 같은 의미

* 큐잉(Quing) : 컴퓨터 내부의 처리를 교통정리하는 것  
병목현상(BottleNeck)이 일어났을때 프로세스가 처리 순서를 큐잉이라는 상태로 보관하고 있다가  
순서를 만들어 출력(대기열=큐잉) 프로세스의 처리 순서를 잡아주는 스케쥴러와 같은 개념  

* 어셈블리 캐시(Assembly Cache)
	최초 페이지 요청시 컴파일된 결과를 저장해두고 있다가 사용자가 요청할때 바로 응답할 수 있도록 해줌.

* MMC 스냅인 (Microsoft Management Console Snap_in)
	전체 프로그램의 일부분으로 작동하는 모듈 형태의 프로그램. 독립형 스냅인과 확정형 두가지가 있음.

* 미리 컴파일(Precompilation)
	웹페이지를 처음 요청했을때 응답속도 지연에 대하여 미리 컴파일 하여 좀 더 빠른 응답속도 기대. 웹사이트를 사용자들에게 배포할때 주로 사용.

* SQL 캐시 종속성(SQL Cache Dependency)
	DB에 접근하여 데이터를 가져와 캐시에 임시저장해두고 사용할때 DB에 변화가 생겼을때 캐시에 반영이 안되는 현상(동기화가 안되었을 경우)에 대비하여  
	DB에 대한 종속성을 캐시에 걸어두고 DB에 변화가 있을때만 DB에 접근하여 캐시를 업데이트하여 DB의 접근횟수 감소

* XML (eXtensible Markup Language) : 구조화된 문서와 데이터를 교환할 수 있는 범용적인 언어. 업계 표준으로 특정 시스템 및 기종 상관없이 데이터를 주고 받을 수 있음.

* CLS (Common Language Specification 공용 언어 명세) : 내부 규격이 같은 .NET 언어들을 의미, 문법은 다르지만 동일한 규격을 가지고 일관성 있게 .NET Framework에 접근 가능하도록 함.

* .NET > .NET Framework : 닷넷은 XML 웹 서비스를 통해 서로 다른 시스템을 통합하기 위한 제반 환경 및 기반을 의미(운영체제, 프레임워크 언어), 닷넷 프레임워크는 닷넷의 목적을 실현시키기 위한 실질적인 기반 또는 기초 환경을 의미(ASP.NET, 데이터 액세스 기술, 윈도우 응용프로그램 구현 기술)
	
* CLR 공용 언어 런타임(Common Language Runtime) : 메모리 관리, 보안 관리, 오류처리 등의 작업을 도와주어 프로그래밍을 단순화 해줌. 닷넷 프레임워크로 개발된 응용 프로그램의 실행 환경 제공.

* 인터페이스 : 클래스와 구조체로 구현할 수 있는 계약을 정의.  
		메소드와 속성, 이벤트, 인덱서를 포함할 수 있다.  
		인터페이스를 구현하는 클래스나 구조체가 제공해야 하는 멤버를 지정해야 한다.  
		인터페이스 내 다수의 인터페이스를 허용한다.  


* 정수 리터럴  
long a = 1000000*1000000;    //error  
long a = 1000000L*1000000L;  // 2000000  

* 전치리 지시문
소스 파일의 특정부분을 조건적으로 건너뛰고 에러나 경고 조건을 보고하며 소스코드의 다른 부분을 나타내는 기능 제공.  
#define, #undef : 조건부 컴파일 기호를 정의하고 정의를 해제할 때  
#if, #elif, #else, #endif : 조건에 따라 소스코드의 특정부분을 건너 뛸때  
#line : 에러나 경고에 나오는 줄 번호를 제어할 때  
#error, #warning : 소스코드의 특정 부분을 명시적으로 표시할 때  
#region, #endregion : 소스코드의 특정 부분을 명시적으로 표시할 때  
#pragma : 컴파일러에 선택적인 상황 정보를 지정할 때  


* OOP:객체지향 프로그래밍
- 캡슐화 : 객체의 내부 자료를 외부에서 임의로 수정할 수 없도록 보호하는 개념이다.
- 클래스/객체 : 클래스는 틀이고, 객체는 틀에서 만들어진 것이다
- 독립성 : 객체는 자체적으로 데이터와 메서드를 가진다. 즉, 객체는 서로 독립적으로 운영된다
- 상속성 : 특정 클래스의 자료나 메서드를 다른 클래스가 물려받아 사용하는 개념이다
- 메서드 : 객체의 동작 행위를 정의하는 것이다
- 다형성 : 상속성을 재정의하는 개념이다.

* 디자인 패턴
Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides가 출판한  
'Design Patterms Elements of Reusable Object Oriented Software'에서 유래  
생성에 대한 패턴, 구조에 대한 패턴, 행동에 대한 패턴이 있다.  
기존 객체지향 프로그래밍 원칙을 확대 및 보완  

* UML : Unified Modeling Language
세계적으로 공통된 프로그램 개발에 관여하는 사람들의 의사소통을 목적으로 모델링하는 개념.  
유스케이프 다이어그램, 클래스 다이어그램, 시퀀스 다이어그램, 액티비티 다이어그램, 컴포넌트 다이어그램,  
배치 다이어그램, 패키지 다이어그램 등이 있다.  
개발해야 할 시스템의 모델링을 위한 기법을 정리한 것으로 전 세계에서 공통적으로 사용하는 표준이다.  

* XML : Extensible Markup Language
자료 전달 및 관리하기 위한 방법 중 하나.  
표준 마크업 종류  
-SGML : 최초 마크업 언어 현재 사용안함.  
-HTML : 웹 환경을 위해 개발된 언어  
-XML : SGML 기준으로 개발되어 사용자가 태그를 설정하여 사용 가능  
-XHTML : HTML 확장형으로 태그를 사용자가 정의 가능  
공용 마크업 종류  
-MathML : 수학공식 표현  
-VoiceML : 음성처리  
-ebXML : B2B를 위해 개발된 언어  


* 네임 스페이스 : XML 문서에서 사용하는 엘리먼트의 유일성 보장

* Human-Oriented Web : 기술적으로 포털을 중심으로 하는 웹은 정보를 사람의 눈으로 모두 확인해야 하는 웹

* Machine-Oriented Web : XML을 기반으로 컴퓨터가 사람을 대신하여 자료를 찾고 조작할 수 있는 웹

* DTD (Data Type Definition) : XML 문서 구조 요약 기술

* XML Schema : XML 문서의 구조에 대한 정리를 위해 개발된 기술

* XSLT : XML 문서를 조작할 수 있도록 개발된 기술


