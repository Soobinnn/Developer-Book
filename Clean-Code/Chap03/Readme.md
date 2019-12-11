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
(ex 3-7 참고)

#### Switch문
본질적으로 switch문은 N가지를 처리하는데, 
단 두개인 switch문도 길다. 하지만 switch문을 완전히 피할 방법은 없다.
그러나 각 switch문을 저차원 클래스에 숨기고 절대로 반복하지 않는 방법은 있다.

다형성을 이용한다.
```java
//ex04
public Money calculatePay(Employee e)
throws InvalidEmployeeType {
  switch (e.type) {
    case COMMISSIONED:
      return calculateCommissionedPay(e);
    case HOURLY:
      return calculateHourlyPay(e);
    case SALARIED:
      return calculateSalariedPay(e);
    default:
      throw new InvalidEmployeeType(e.type);
  }
}
```
위 함수의 문제점
1. 함수가 길다. (새 직원유형을 추가하면 더길어짐)
2. '한가지' 작업만 수행하지 않는다.
3. SRP(Single Responsibility Principle)을 위반한다.
4. OCP를 위반한다.

해결 코드
```java
public abstract class Employee {
  public abstract boolean isPayday();
  public abstract Money calculatePay();
  public abstract void deliverPay(Money pay);
}

public interface EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}

public class EmployeeFactoryImpl implements EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
    switch(r.type) {
      case COMMISSIONED:
        return new CommissionedEmployee(r);
      case HOURLY:
        return new HourlyEmployee(r);
      case SALARIED:
        return new SalariedEmployee(r);
      default:
        throw new InvalidEmployeeType(r.type);
    }
  }
}
```
일반적으로 나는 switch문은 단 한 번만 참아준다. 다형적 객체를 생성하는 코드 안에서다.
이렇게 상속 관계로 숨긴 후에는 절대로 다른 코드에 노출하지 않는다.
물론 불가피한 상황도 생긴다. 나 자신도 이 규칙을 위반한 경험이 여러 번 있다.

#### 서술적인 이름을 사용하라
ex3-7에서 예제 함수이름을 testableHtml -> SetupTeardownIncluder.render로 변경했다.
함수가 하는 일을 좀 더 잘 표현하므로 훨씬 좋은 이름이다.
좋은 이름이 주는 가치는 아무리 강조해도 지나치지 않다. 코드를 읽으면서 짐작했던 기능을 각 루틴이 그대로 수행한다면 깨끗한 코드라 불러도 된다.

이름이 길어도 괜찮다. 겁먹을 필요없다. 길고 서술적인 이름이 짧고 어려운 이름보다 좋다.
길고 서술적인 이름이 길고 서술적인 주석보다 좋다.
함수 이름을 정할 때는 여러 단어가 쉽게 읽히는 명명법을 사용한다.
그런 다음, 여러 단어를 사용해 함수 기능을 잘 표현하는 이름을 선택한다.
이름을 정하느라 시간을 들여도 괜찮다. 이런저런 이름을 넣어 코드를 일겅보면 더 좋다.

이름을 붙일 때는 일관성이 있어야 한다. 모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용한다.

#### 함수 인수
함수에서 이상적인 인수 개수는 0개다. 다음은 1개(단항)이고, 다음은 2개(이항)다. 3개(삼항)은 가능한 피하는 편이 좋다.
4개 이상(다항)은 특별한 이유가 필요하다. 특별한 이유가 있어도 사용하면 안 된다.

인수는 어렵다.
테스트 관점에서 보면 인수는 더어렵다.
갖가지 인수 조합으로 함수를 검증하는 테스틐 케이스를 작성한다고 상상해보라.

#### 많이 쓰는 단항 형식
함수에 인수 1개를 넘기는 이유로 가장 흔한 경우는 두 가지다.
1. 인수에 질문을 던지는 경우다.
ex) boolean fileExists("MyFile")이 좋은 예다.
2. 인수를 뭔가로 변환해 결과를 반환하는 경우다.
함수 이름을 지을 때는 두 경우를 분명히 구분한다.

#### 플래그 인수
플래그 인수는 추하다. 함수로 부울 값을 넘기는 관례는 정말로 찍하다.
