# Thread vs Runnable

자바에서 쓰레드를 구현하는 방법은 2가지가 있다.
- Runnable
- Thread

## Runnable

- implements Runnable 을 통해서 Runnable 인터페이스를 구현할 수 있다.
- Runnable 인터페이스를 구현하게되면 재사용성이 높고, 코드의 일관성을 유지할 수 있어서 Thread보다 더 효율적인 방법이라 할 수 있다.
- Runnable 인터페이스는 함수형 인터페이스이다. 
  - 즉 추상 메서드 ``run``을 반드시 구현해야 한다.
  - 함수형 인터페이스여서 간단한 람다식으로 구현이 가능하다. 
  - [함수형 인터페이스 자료](https://github.com/jeus1998/About-Java/blob/main/record/%ED%95%A8%EC%88%98%ED%98%95%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4.md)

### Runnable 인터페이스 구현 

```java
public class MyRunnable implements Runnable{
    private String name;
    public MyRunnable(String name) {
        this.name = name;
    }
    @Override
    public void run() {
        for (int i = 0; i < 50; i++) {
            System.out.println(name + " " + i);
        }
    }
}
```

### MyRunnable Test

```java
@Test
void runnableTest1(){
    MyRunnable myRunnable1 = new MyRunnable("myRunnable1");
    MyRunnable myRunnable2 = new MyRunnable("myRunnable2");

    Thread thread1 = new Thread(myRunnable1);
    Thread thread2 = new Thread(myRunnable2);
    thread1.start();
    thread2.start();
}
```

실행 결과
```text
myRunnable2 0
myRunnable1 0
myRunnable2 1
myRunnable1 1
myRunnable1 2
myRunnable1 3
myRunnable2 2
myRunnable2 3
myRunnable1 4
myRunnable1 5
myRunnable1 6
myRunnable1 7
myRunnable1 8
myRunnable1 9
myRunnable1 10
myRunnable1 11
myRunnable1 12
myRunnable1 13
myRunnable1 14
myRunnable1 15
myRunnable2 4
myRunnable2 5
myRunnable2 6
myRunnable2 7
myRunnable2 8
myRunnable2 9
myRunnable2 10
myRunnable2 11
myRunnable2 12
myRunnable2 13
myRunnable2 14
myRunnable2 15
myRunnable2 16
myRunnable2 17
myRunnable2 18
myRunnable2 19
myRunnable2 20
myRunnable2 21
myRunnable2 22
myRunnable2 23
myRunnable1 16
myRunnable1 17
myRunnable2 24
myRunnable2 25
myRunnable1 18
myRunnable1 19
myRunnable1 20
myRunnable2 26
myRunnable2 27
myRunnable2 28
myRunnable1 21
myRunnable1 22
myRunnable1 23
myRunnable1 24
myRunnable1 25
myRunnable1 26
myRunnable1 27
myRunnable1 28
myRunnable1 29
myRunnable1 30
myRunnable1 31
myRunnable1 32
myRunnable1 33
myRunnable1 34
myRunnable1 35
myRunnable1 36
myRunnable1 37
myRunnable1 38
myRunnable2 29
myRunnable2 30
myRunnable2 31
myRunnable2 32
myRunnable2 33
myRunnable2 34
myRunnable2 35
myRunnable2 36
myRunnable2 37
myRunnable2 38
myRunnable2 39
myRunnable2 40
myRunnable2 41
myRunnable2 42
myRunnable2 43
myRunnable1 39
myRunnable1 40
myRunnable1 41
myRunnable1 42
myRunnable1 43
myRunnable2 44
myRunnable2 45
myRunnable2 46
myRunnable2 47
myRunnable2 48
myRunnable2 49
myRunnable1 44
myRunnable1 45
myRunnable1 46
myRunnable1 47
myRunnable1 48
myRunnable1 49
```
- Runnable 인터페이스를 구현해서 생성한 객체를 Thread 객체를 만들 때 재사용 할 수 있다. 

### 람다식을 통한 Runnable 객체 만들기 

```java
@Test
void runnableTest2(){

    Runnable myRunnable1 = ()-> {
        for (int i = 0; i < 50; i++) {
            System.out.println("myRunnable1 " + i);
        }
    };
    
    MyRunnable myRunnable2 = new MyRunnable("myRunnable2");

    Thread thread1 = new Thread(myRunnable1);
    Thread thread2 = new Thread(myRunnable2);
    thread1.start();
    thread2.start();
}
```
- 람다식을 활용한 Runnable 객체 만들기 
- 결과 생략 

## Thread

- 상속을 받아서 사용해야 하기 때문에 다른 클래스를 상속받아 사용할 수 없다는 단점이 있다.
  - 자바는 다중 상속 ❌
- 일반적으로는 Runnable 인터페이스를 구현해서 스레드를 사용


### Thread 상속을 이용한 스레드 구현 

```java
/**
 * MyThread extends Thread
 */
public class MyThread extends Thread{
    String name;
    public MyThread(String name) {
        this.name = name;
    }
    public void run(){
        for (int i = 0; i < 50; i++) {
            System.out.println(name + " " + i);
        }
    }
}
```
- 문자를 인자로 받아서 50번 반복해서 출력하는 스레드 

### run() vs start()

```java
@Slf4j
class ThreadTest {
    @Test
    void threadBasicRunTest(){
        MyThread myThread1 = new MyThread("myThread1");
        MyThread myThread2 = new MyThread("myThread2");

        myThread1.run();
        myThread2.run();
    }
    @Test
    void threadBasicStartTest(){
        MyThread myThread1 = new MyThread("myThread1");
        MyThread myThread2 = new MyThread("myThread2");

        myThread1.start();
        myThread2.start();
    }
}
```

run() 실행 결과
```text
myThread1 0
myThread1 1
myThread1 2
myThread1 3
myThread1 4
myThread1 5
myThread1 6
myThread1 7
myThread1 8
myThread1 9
myThread1 10
myThread1 11
myThread1 12
myThread1 13
myThread1 14
myThread1 15
myThread1 16
myThread1 17
myThread1 18
myThread1 19
myThread1 20
myThread1 21
myThread1 22
myThread1 23
myThread1 24
myThread1 25
myThread1 26
myThread1 27
myThread1 28
myThread1 29
myThread1 30
myThread1 31
myThread1 32
myThread1 33
myThread1 34
myThread1 35
myThread1 36
myThread1 37
myThread1 38
myThread1 39
myThread1 40
myThread1 41
myThread1 42
myThread1 43
myThread1 44
myThread1 45
myThread1 46
myThread1 47
myThread1 48
myThread1 49
myThread2 0
myThread2 1
myThread2 2
myThread2 3
myThread2 4
myThread2 5
myThread2 6
myThread2 7
myThread2 8
myThread2 9
myThread2 10
myThread2 11
myThread2 12
myThread2 13
myThread2 14
myThread2 15
myThread2 16
myThread2 17
myThread2 18
myThread2 19
myThread2 20
myThread2 21
myThread2 22
myThread2 23
myThread2 24
myThread2 25
myThread2 26
myThread2 27
myThread2 28
myThread2 29
myThread2 30
myThread2 31
myThread2 32
myThread2 33
myThread2 34
myThread2 35
myThread2 36
myThread2 37
myThread2 38
myThread2 39
myThread2 40
myThread2 41
myThread2 42
myThread2 43
myThread2 44
myThread2 45
myThread2 46
myThread2 47
myThread2 48
myThread2 49
```
- 새로운 스레드 생성을 하지 않고 메인 스레드의 호출 스택에서 동작
- 병렬 처리 ❌ 

start() 실행 결과
```text
myThread1 0
myThread2 0
myThread1 1
myThread2 1
myThread2 2
myThread1 2
myThread2 3
myThread1 3
myThread1 4
myThread2 4
myThread2 5
myThread1 5
myThread1 6
myThread2 6
myThread2 7
myThread2 8
myThread1 7
myThread2 9
myThread1 8
myThread1 9
myThread2 10
myThread2 11
myThread1 10
myThread2 12
myThread2 13
myThread2 14
myThread2 15
myThread2 16
myThread2 17
myThread2 18
myThread2 19
myThread2 20
myThread1 11
myThread1 12
myThread1 13
myThread2 21
myThread2 22
myThread2 23
myThread2 24
myThread2 25
myThread1 14
myThread2 26
myThread1 15
myThread2 27
myThread1 16
myThread1 17
myThread2 28
myThread2 29
myThread2 30
myThread1 18
myThread1 19
myThread1 20
myThread1 21
myThread1 22
myThread1 23
myThread1 24
myThread2 31
myThread2 32
myThread1 25
myThread2 33
myThread2 34
myThread2 35
myThread2 36
myThread2 37
myThread2 38
myThread2 39
myThread2 40
myThread2 41
myThread2 42
myThread2 43
myThread2 44
myThread2 45
myThread2 46
myThread2 47
myThread2 48
myThread2 49
myThread1 26
myThread1 27
myThread1 28
myThread1 29
myThread1 30
myThread1 31
myThread1 32
myThread1 33
myThread1 34
myThread1 35
myThread1 36
myThread1 37
myThread1 38
myThread1 39
myThread1 40
myThread1 41
myThread1 42
myThread1 43
myThread1 44
myThread1 45
myThread1 46
myThread1 47
myThread1 48
myThread1 49
```
- 새로운 스레드 생성하여 해당 스레드의 호출 스택에서 run()메서드 동작
- 병렬 처리 ⭕️ 

- run()을 호출하는 것은 생성된 스레드 객체를 실행하는 것이 아니라, 단순히 스레드 클래스 내부의 run 메서드를 실행시키는 것이다.
- main 함수의 스레드를 그대로 사용해서 run 메서드를 실행하기 때문에 새로운 스레드가 생기지 않고 병렬처리를 할 수 없다.
- 반면에 start()는 새로운 스레드를 실행하는데 필요한 호출 스택(call stack)을 생성한 다음에 run을 호출해서, 
  생성된 호출 스택에 run()이 첫 번째로 저장되게 한다.
- start()를 호출하면 스레드를 새롭게 생성해서 해당 스레드를 runnable 한 상태로 만든 후 run() 메서드를 실행하게 된다.
- 따라서 start()를 호출해야만 멀티스레드로 병렬 처리가 가능해진다.

그렇다면 추상 메서드로 run() 밖에 존재하지 않는 Runnable은 왜 사용하는 것일까?
- Thread를 바로 사용하려면 상속을 받아야 한다. 
- 자바는 다중 상속을 허용하지 않기 때문에 Thread 클래스를 바로 상속받는 경우 다른 클래스를 상속받지 못한다.
- 하지만 Runnable 인터페이스를 구현한 경우에는 다른 인터페이스를 추가로 구현할 수 있을 뿐만 아니라, 다른 클래스도 상속받을 수 있다.
- 따라서 클래스의 확장성이 중요하다면 Runnable 인터페이스를 구현해 Thread에 주입해 주는것이 더 좋아 보인다.

### 참고자료
- [개발냥발](https://hyeo-noo.tistory.com/293)
