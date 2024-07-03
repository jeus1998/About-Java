# 제네릭 (Generics)

### 자바에서 제네릭(Generics)

- 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법 
- 객체별로 다른 타입의 자료가 저장될 수 있게 한다. 
- 제네릭은 클래스나 메서드를 정의할 때 타입을 일반화(generalize)하여 특정 타입에 종속되지 않게 하는 것 
- 실제 사용할 때 구체적인 타입으로 대체 

### 제네릭 문법 예시 (배열과 리스트 선언 비교)

```java
String [] array = new String [10];

ArrayList<String> list = new ArrayList<>();
```
- ``<>`` 꺽쇠 괄호가 제네릭이다. 
- 괄호 안에 타입명울 지정한다.
  - ``<String>``: 해당 자료형의 타입은 String 타입으로 지정되어 문자열 데이터만 리스트에 적재할 수 있게 한다. 
- 선언하는 키워드나 문법 순서가 다를뿐, 결국 자료형명을 선언하고 자료형의 타입을 지정하는 점은 같다.
- 제네릭은 배열의 타입을 지정하듯이 리스트 자료형 같은 컬렉션 클래스나 메소드에 사용할 내부 데이터 타입(type)을 파라미터(parameter)
  주듯이 외부에서 지정하는 타입을 변수화 한 기능이라고 이해하면 된다.

### 제네릭 사용 예시

```java
public class Main {
    public static void main(String[] args) {
        // 제네릭 타입에 정수 타입을 할당 
        FruitBox<Integer> intBox = new FruitBox<>(); 
      
        // 제네릭 타입에 문자열 타입을 할당 
        FruitBox<String> stringBox = new FruitBox<>();
    }
}

class Fruit<T>{
    List<T> fruits = new ArrayList<>();
    
    public void add(T fruit){
        fruits.add(fruit);    
    }
    
    public T get(int index){
        return fruits.get(index);
    }
}
```
- 타입 파라미터 생략 
  - 제네렉 객체를 사용하는 문법의 형태를 보면 양쪽 두 군데에 꺾쇠 괄호 제네릭 타입을 지정함을 볼 수 있다.
  - 하지만 맨 앞에서 클래스명과 함께 타입을 지정해 주었는데 굳이 생성자까지 제네릭을 지정해 줄 필요가 없다(중복)
  - jdk1.7 이후 new 생성자 부분의 제네릭 타입을 생략할 수 있게 되었다. 
- 타입 파라미터 할당 가능 타입 
  - 제네렉에서 할당 받을 수 있는 타입은 Reference 타입 뿐이다.
  - int, double 같은 원시 타입(Primitive Type)은 사용하지 못한다.
- 제네릭 타입 전파
  - ```<T>``` 부분에서 타입을 받아 내부에서 ```<T>```타입으로 지정한 멤버들에게 타입이 구체적으로 설정 되는 것이다.
  - 이를 구체화(Specialization)라고 한다. 

### 복수 타입 파라미터 

- 제네릭은 반드시 한개만 사용하라는 법은 없다. 만일 타입 지정이 여러개가 필요할 경우 2개,3개..n개 얼마든지 만들 수 있다.
- 제네릭 타입의 구분은 꺽쇠 괄호 안에서 쉼표(,)로 하며 ``<T, U>`` 와 같은 형식으로 복수 타입 파라미터를 지정할 수 있다.
- 당연히 클래스 초기화 시점에 제네릭 타입을 두개를 넘겨주어야 한다. 

```java
import java.util.ArrayList;
import java.util.List;

class Apple {}
class Banana {}

class FruitBox<T, U> {
    List<T> apples = new ArrayList<>();
    List<U> bananas = new ArrayList<>();

    public void add(T apple, U banana) {
        apples.add(apple);
        bananas.add(banana);
    }
}

public class Main {
    public static void main(String[] args) {
    	// 복수 제네릭 타입
        FruitBox<Apple, Banana> box = new FruitBox<>();
        box.add(new Apple(), new Banana());
        box.add(new Apple(), new Banana());
    }
}
```

### 타입 파라미터 기호 네이밍 

- 지금까지 제네릭 기호를 ``<T>``와 같이 써서 표현했지만 식별자 기호는 문법적으로 정해진 것이 없다. 
- for문을 이용할때 변수를 i ➡️ j ➡️ k 순서대로 지정해서 사용하듯이 제네릭의 표현 변수를 ``<T>``로 표현한다고 보면 된다.
- 명명하고 싶은대로 아무 단어나 넣어도 문제는 없지만, 대중적으로 통하는 통상적인 네이밍이 있으면 개발이 용이해 지기 때문에 
  아래 표와 같은 암묵적인 규칙(convention)이 존재한다.
  
![3.png](Image%2F3.png)


### 제네릭 메서드 

그냥 타입 파라미터로 타입을 지정한 메서드 
```java
class FruitBox<T> {

    public T addBox(T x, T y) {
        // ...
    }
}
```

제네릭 메서드 
```java
class FruitBox<T> {
	
    // 클래스의 타입 파라미터를 받아와 사용하는 일반 메서드
    public T addBox(T x, T y) {
        // ...
    }
    
    // 독립적으로 타입 할당 운영되는 제네릭 메서드
    public static <T> T addBoxStatic(T x, T y) {
        // ...
    }
}
```
- 제네릭 메서드란, 메서드의 선언부에 ``<T>``가 선언된 메서드를 말한다. 
- 제네릭 메서드는 직접 메서드에  ``<T>``를 설정함으로서 동적으로 타입을 받아와 사용할 수 있는 독립으로 운용 가능한 메서드이다. 


제네릭 메서드의 호출 원리 
```java
public class Main{
  public static void main(String[] args) {
     FruitBox.<Integer>addBoxStatic(1, 2);
     FruitBox.<String>addBoxStatic("안녕", "잘가");
     
    // 메서드의 제네릭 타입 생략
    // 매개변수의 데이터형을 보고 타입을 추론 
    FruitBox.addBoxStatic(1, 2); 
    FruitBox.addBoxStatic("안녕", "잘가");
  }
}

class FruitBox<T> {
	
    // 클래스의 타입 파라미터를 받아와 사용하는 일반 메서드
    public T addBox(T x, T y) {
        // ...
    }
    
    // 독립적으로 타입 할당 운영되는 제네릭 메서드
    public static <T> T addBoxStatic(T x, T y) {
        // ...
    }
}
```
- 제네릭 타입을 메서드명 옆에 지정해줬으니, 호출 역시 메서드 왼쪽애 제네릭 타입이 위치하게 된다.
- 이때 컴파일러가 제네릭 타입에 들어갈 데이터 타입을 메소드의 매개변수를 통해 추정할 수 있기 때문에
  대부분의 경우 제네릭 메서드의 타입 파라미터를 생략하고 호출할 수 있다. 

의문점 
- 클래스 옆에 붙어있는 제네릭과 제네릭 메소드는 똑같은 ```<T>```인데 어떻게 제네릭 메서드만이 독립적으로 운용되는지?
```text
처음 제네릭 클래스를 인스턴스화하면, 클래스 타입 매개변수에 전달한 타입에 따라 제네릭 메소드도 타입이 정해지게 된다.

그런데 만일 제네릭 메서드를 호출할때 직접 타입 파라미터를 다르게 지정해주거나(타입 지정), 다른 타입의 데이터(타입 생략)를 매개변수에 넘긴다면 
독립적인 타입을 가진 제네릭 메서드로 운용되게 된다.
```


### 제네릭 사용 이유와 이점

- 컴파일 타임에 타입 검사를 통해 예외 방지 
  - 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거하여 개발을 용이하게 해준다.
  - 제일 좋은 에러는 컴파일 에러이다.
- 불필요한 캐스팅을 없애 성능 향상
  - 제네릭은 미리 타입을 지정 & 제한해 놓기 때문에 형 변환(Type Casting)의 번거로움을 줄일 수 있다. 
  - 타입 검사에 들어가는 메모리를 줄일 수 있고 가독성 또한 좋아진다. 
  - down casting or up casting 과정은 추가적인 오버헤드가 발생하는 것과 같다.  


### 참고자료
-[Inpa Dev](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A0%9C%EB%84%A4%EB%A6%ADGenerics-%EA%B0%9C%EB%85%90-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)