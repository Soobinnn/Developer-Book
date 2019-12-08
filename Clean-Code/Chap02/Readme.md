## 2. 의미 있는 이름

### 의도를 분명히 밝혀라
의도가 분명하게 이름을 지으라.

좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 더 많다.

그러므로 이름을 주의 깊게 살펴 더 나은 이름이 떠오르면 개선하기 바란다.
그러면 코드를 읽는 사람이 좀 더 행복해진다.

변수(함수,클래스)의 존재 이유는?
수행 기능은?
사용 방법은?
따로 주석이 필요하다면 의도를 분명히 드러내지 못했다는 말이다.
```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for(int[] x : theList)
    if(x[0]==4)
      list1.add(x);
  return list1;
}
```
코드가 하는 일을 짐작하기 어렵다. 
why? 복잡한 문장은 없다.

문제는 코드의 단순성이 아니라 코드의 함축성이다.
다시 말해, 코드 맥락이 코드 자체에 명시적으로 드러나지 않는다.
위 코드는 암암리에 독자가 다음과 같은 정보를 안다고 가정한다.
1. theList에 무엇이 들었는가?
2. theList에서 0번째 값이 어째서 중요한가?
3. 값4는 무슨 의미인가?
4. 함수가 반환하는 리스트 list1을 어떻게 사용하는가?

위 코드 샘플엔 이와 같은 정보가 드러나지 않는다. 하지만 정보 제공은 충분히 가능했었다.

각 개념에 이름만 붙여도 코드가 상당히 나아진다.
```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>();
  for(int[] cell : gameBoard)
    if(cell[STATUS_VALUE] == FLAGGED)
      flaggedCells.add(cell);
  return flaggedCells;
}
```
단순성은 변하지 않았다. 연산자 수와 상수 수는 앞의 예와 같다.
그런데도 코드는 더욱 명확해졌다.
한 걸음 더 나아가, int 배열을 사용하는 대신, 칸을 간단한 클래스로 만들어도되겠다.
isFlagged라는 좀 더 명시적인 함수를 사용해 FLAGGED라는 상수를 감춰도 좋겠다.
새롭게 개선한 결과는 다음과 같다.
```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<Cell>();
  for(Cell cell : gameBoard)
    if(cell.isFlagged())
      flaggedCells.add(cell);
  return flaggedCells;
}
```
단순히 이름만 고쳤는데도 함수가 하는 일이 이해하기 쉬워졌다. 바로 이것이 좋은 이름이 주는 위력이다.

### 그릇된 정보를 피해라
