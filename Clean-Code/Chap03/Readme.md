## 3. 함수
-----

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


