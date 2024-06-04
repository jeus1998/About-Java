
# instanceof vs isAssignableFrom 

### 클래스 객체란?

클래스 객체는 자바에서 클래스의 메타데이터를 담고 있는 객체를 의미한다.
자바에서는 모든 클래스가 런타임에 Class 타입의 객체로 표현된다.
이 객체를 통해 클래스에 대한 정보(예: 클래스 이름, 메소드, 필드, 슈퍼 클래스)를 얻거나, 
리플랙션(reflection)을 사용해 동적으로 객체를 생성하고 메서드를 호출하는 것이 가능하다.

클래스 객체 예시 
```java
Class<String> stringClass = String.class;
```
위 코드에서 String.class는 String 클래스의 클래스 객체를 참조한다.
이 객체(stringClass)는 String 클래스에 대한 메타데이터를 포함하고 있다.

클래스 객체 사용 예시
```java
// 클래스 이름 얻기
String className = stringClass.getName();

// 동적 객체 생성
String newStringInstance = stringClass.newInstance();

// 메소드 호출
Method method = stringClass.getMethod("length");
Integer length = (Integer) method.invoke(newStringInstance);
```

### instanceof vs isAssignableFrom 분석 

isAssignableFrom과 instanceof는 비슷한 기능을 하지만, 사용되는 상황과 목적이 다르다.

isAssignableFrom
- 클래스 레벨에서 사용
- 두 클래스 간의 상속 관계 검사
- ex) Item.class.isAssignableFrom(Book.class)는 Book이 Item의 하위 클래스인지 검사한다. 

instanceof
- 객체 레벨에서 사용
- 특정 객체가 특정 클래스의 인스턴스인지 확인
- ex) bookInstance instanceof Item는 bookInstance가 Item 또는 그 하위 클래스의 인스턴스인지 검사한다.






