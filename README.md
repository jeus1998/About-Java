# About-Java
자바 공부 기록

## & VS && 

자바에서 &와 &&는 둘 다 논리 연산자이다. 하지만 차이점이 있다.

### & (비트 연산자 및 논리 연산자)

비트 연산자 

- &는  비트 연산자로 사용할 때, 두 피연산자의 비트 단위로 AND 연산을 수행합니다.

```java
int a = 5; // 0101 in binary
int b = 3; // 0011 in binary
int result = a & b; // 0001 in binary
```
논리 연산자

- &는 논리 연산자로 사용할 때, 두 피연산자가 모두 true 일 때만 결과가 true가 됩니다.
- 두  피연산자가 모두 평가된다. (단일 조건 체크라도 오른쪽 피연산자까지 평가)

```java
boolean a = false;
boolean b = true;
boolean result = a & b; // result is false
```

- a에서 이미 false여서 result가 false지만 오른쪽인 b까지 확인을 한다.

### && 조건부 논리 연산자

- &&는 두 피연산자가 모두 true일 때만 결과가 true가 됩니다.
- 조건부(단축 평가) 논리 연산자입니다.
- 왼쪽 피연산자가 false인 경우, 오른쪽 피연산자는 평가되지 않는다.

```java
boolean a = false;
boolean b = false;
boolean result = a && b; // result is false, b is not evaluated
```
- a에서 이미 false여서 오른쪽 b는 평가되지 않는다.

### 요약

두 연산자의 차이는 주로 성능 및 부작용(두 번째 피연산자의 평가 여부)에 영향을 미친다.

&&를 사용하면 불필요한 평가를 피할 수 있어 성능이 향상된다.
