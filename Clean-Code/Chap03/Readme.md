## 3. 함수
-----

```java
// ex 3-2
public static String renderPageWithSetupsAndTeardowns(
  PageData pageData, boolean isSuite) throws Exception {
  boolean isTestPage = pageData.hasAttribute("Test");
  if(isTestPage) {
    WikiPage testPage = pageData.getWikiPage();
    StringBuffer newPageContent = new StringBuffer();
    includeSetupPages(testPage, newPageContent, isSuite);
    newPageContent.append(pageData.getContent());
    includeTeardownPages(testPage, newPageContent, isSuite);
    pageData.setContent(newPageContent.toString());
  }
  return pageData.getHtml();
}
```
함수가 설정(setUp)페이지와 해제(teardown)페이지를 테스트 페이지에 넣은 후 해당 테스트 페이지를 HTML로 렌더링한다.
예제 3-1에서는 파악하기 어려웠떤 정보가 3-2에서는 쉽게 드러난다.


### 작게 만들어라!
함수를 만드는 첫째 규칙은 '작게'다.
함수를 만드는 둘째 규칙은 '더 작게'다.

이 규칙은 근거를 대기가 곤란하다. 함수가 작을수록 더 좋다는 증거나 자료를 제시하기도 어렵다.<br>
하지만 지금까지 경험을 바탕과 시행착오를 바탕으로 작은 함수가 좋다고 확신한다.<br>
한 화면에 가로 150자 세로 100줄을 넘어서는 안된다. 아니 20줄도 길다.<br>

그렇다면 얼마나 짧아야 좋을까?<br>
일반적으로 예제 3-2보다 짧아야 한다.<br>
예제 3-2는 3-3으로 줄여야 마땅하다.<br>

```java
// ex 3-3
public static String renderPageWithSetupsAndTeardowns(
  PageData pageData, boolean isSuite) throws Exception {
  if(isTestPage(pageData))
    includeSetupAndTeardownPages(pageData, isSuite);
  return pageData.getHtml();
}
```

다시 말해, if문/else문/while문 등에 들어가는 블록은 한 줄이어야 한다는 의미다.<br>
대개 거기서 함수를 호출한다. 그러면 바깥을 감싸는 함수(enclosing function)가 작아질 뿐 아니라,<br>
블록 안에서 호출하는 함수 이름을 적절히 짓는다면, 코드를 이해하기도 쉬워진다.<br>
이 말은 중첩 구조가 생길만큼 함수가 커져서는 안 된다는 뜻이다. <br>
그러므로 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다.<br>

### 한가지만 해라
함수는 한가지를 해야 한다. 그 한가지를 잘 해야 한다. 그 한 가지만을 해야 한다.

그렇다면 3-3은 한 가지만 하는가? 세 가지를 한다고 주장할 수도 있다.
1. 페이지가 테스트페이지인지 판단한다.
2. 그렇다면 설정 페이지와 해제 페이지를 넣는다.
3. 페이지를 HTML로 렌더링 한다.

위에서 언급하는 세 단계는 지정된 함수 이름 아래에서 추상화 수준이 하나다.
함수는 간단한 TO문단으로 기술할 수 있다.

TO RenderPageWithSetupsAndTeardowns, 페이지가 테스트 페이지인지 확인 한 후 테스트 페이지라면 설정 페이지와 해제 페이즈를 넣는다.
테스트 페이지든 아니든 페이지를 HTML로 렌더링한다.

지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다.

의미를 유지하면서 예제 3-3을 더 이상 줄이기란 불가능하다.
if문을 includeSetupsAndTeardownsIfTestPate라는 함수로 만든다면 똑같은 내용을 다르게 표현할 뿐 추상화 수준은 바뀌지 않는다.
따라서, 함수가 '한 가지만'하는지 판단하는 방법이 하나 더 있다.
단순히 다른 표현이 아니라 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈이다.

### 함수 당 추상화 수준은 하나로
함수가 확실히 '한 가지' 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.

예제 3-1은 이 규칙을 확실히 위반한다.
getHtml()은 추상화 수준이 아주 높다.
String pagePathName= PathParser.render(pagepath);는 추상화 수준이 중간이다.
.append("\n")와 같은 코드는 추상화 수준이 아주 낮다.

한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.

#### 위에서 아래로 코드 읽기: 내려가기 규칙

코드는 위에서 아래로 이야기처럼 읽혀야 좋다. 한 함수 다음에는 추상화 수준이 한단계 낮은 함수가 온다.
즉, 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다. 이것을 내려가기 규칙이라 부른다.

ex) TO 설정 페이지와 해제 페이지를 포함하려면, 설정 페이지를 포함하고, 테스트 페이지 내용을 포함하고, 해제 페이지를 포함한다.
TO 설정 페이지를 포함하려면, 슈트이면 슈트 설정 페이지를 포함한 후 일반 설정 페이지를 포함한다.
TO 슈트 설정 페이지를 포함하려면, 부모 계층에서 "SuiteSetUp" 페이지를 찾아 include 문과 페이지 경로를 추가한다.
TO 부모 계층을 검색하려면, ...

하지만 추상화 수준이 하나인 함수를 구현하기란 쉽지 않다.
많은 프로그래머가 곤란을 겪는다. 그렇지만 매우 중요한 규칙이다.
핵심은 짧으면서도 한 가지만 하는 함수다.
위에서 아래로 TO문단을 읽어내려 가듯이 코드를 구현하면 추상화 수준을 일관되게 유지하기가 쉬워진다.

