# Non-DaemonThread VS DaemonThread & Junit

### 기록, 공부 이유

```java
@Slf4j
public class FieldServiceTest {
    private final FieldService fieldService = new FieldService();
    @Test
    void field(){
        log.info("main start");

        Runnable userA = () ->{
            fieldService.logic("userA");
        };

        Runnable userB = () ->{
            fieldService.logic("userB");
        };

        Thread threadA = new Thread(userA);
        threadA.setName("thread-A");
        Thread threadB = new Thread(userB);
        threadB.setName("thread-B");

        threadA.start();
        sleep(100); // 동시성 문제 발생 X
        threadB.start();

        sleep(3000); // 메인 쓰레드 종료 대기
        log.info("main exit");
    }
    private void sleep(int mills) {
        try {
            Thread.sleep(mills);
        }
        catch (InterruptedException e){
            log.info("sleep fail{}", e);
        }
    }
}
```
```text
김영한님 스프링 강의를 듣던 도중 main 쓰레드 종료 대기를 시키시길래 엥 데몬 쓰레드가 아니면
메인 쓰레드가 종료되더라도 해당 일반 스레드들은 계속 돌아갈 텐데 라고 생각을 하면서 해당 코드를 주석 처리했더니
메인 쓰래드가 종료가 되고 내가 실행시킨 threadA, threadB가 바로 종료되는 현상을 발견했다. 
이참에 데몬스레드에 대해서 더 자세히 알아보고 테스트 환경에서 스레드 동작도 이해해 보자.
```

### 데몬 스레드 vs 일반 스레드

- 일반 스레드 
  - 애플리케이션의 주요 작업을 수행
  - 모든 일반 스레드가 종료되지 않는 한, 자바 애플리케이션(JVM)은 종료되지 않는다.
- 데몬 스레드
  - 주로 백그라운드 작업을 수행(가비지 컬렉션, 로그 작성 등)
  - 모든 일반 스레드가 종료되면 JVM은 데몬 스레드를 강제 종료하고 애플리케이션을 종료

Thread 클래스 일부 
```java
public class Thread implements Runnable {
    /* Make sure registerNatives is the first thing <clinit> does. */
    private static native void registerNatives();

    static {
        registerNatives();
    }

    private volatile String name;
    private int priority;

    /* Whether or not the thread is a daemon thread. */
    private boolean daemon = false;
    
    // ...
}    
```
- name: 스레드 이름 
- priority: 스레드 우선순위 
- daemon: daemon or not(normal)

### 데몬 스레드 메인 스레드 죵료 시 함께 종료 예제

```java
public class DaemonThreadExample {
    
    public static void main(String[] args) {
        
        Thread daemonThread = new Thread(() -> {
            try {
                while (true) {
                    System.out.println("Daemon thread is running...");
                    Thread.sleep(500);
                }
            } 
            catch (InterruptedException e) {
                System.out.println("Daemon thread interrupted");
            }
        });

        // 데몬 스레드로 설정
        daemonThread.setDaemon(true);
        daemonThread.start();

        // 메인 스레드 대기
        try {
            Thread.sleep(2000);
        } 
        catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread exiting...");
    }
}
```
- 데몬 스레드 생성: ``daemonThread``는 백그라운드에서 계속 실행되는 스레드로, 무한 루프에서 주기적으로 메시지를 출력
- 데몬 스레드 설정: ``daemonThread.setDaemon(true);``로 데몬 스레드로 설정
- 메인 스레드 종료: 메인 스레드는 2초 후 종료. 메인 스레드가 종료되면 데몬 스레드도 종료
- 이때 해당 데몬 스레드를 생성한 스레드는 메인 스레드이다. 그래서 자신을 생성한 일반 스레드가 종료되니까 같이 종료되는 것이다.

출력 결과
```text
Daemon thread is running...
Daemon thread is running...
Daemon thread is running...
Daemon thread is running...
Main thread exiting...
```

### JUnit 테스트와 스레드 종료

```java
import org.junit.jupiter.api.Test;

public class ThreadTest {

    @Test
    void testDaemonThread() {
        Thread daemonThread = new Thread(() -> {
            try {
                while (true) {
                    System.out.println("Daemon thread is running...");
                    Thread.sleep(500);
                }
            } catch (InterruptedException e) {
                System.out.println("Daemon thread interrupted");
            }
        });

        // 데몬 스레드로 설정
        daemonThread.setDaemon(true);
        daemonThread.start();

        // 메인 스레드 대기
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread exiting...");
    }

    @Test
    void testNormalThread() {
        Thread normalThread = new Thread(() -> {
            try {
                while (true) {
                    System.out.println("Normal thread is running...");
                    Thread.sleep(500);
                }
            } catch (InterruptedException e) {
                System.out.println("Normal thread interrupted");
            }
        });

        // 일반 스레드로 설정
        normalThread.start();

        // 메인 스레드 대기
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread exiting...");
    }
}
```
- testDaemonThread()
  - 데몬 스레드를 생성하고 시작
  - 메인 스레드가 2초 동안 대기한 후 종료
  - 데몬 스레드는 메인 스레드가 종료되면 함께 종료
- testNormalThread()
  - 일반 스레드를 생성하고 시작
  - 메인 스레드가 2초 동안 대기한 후 종료
  - 일반 스레드는 메인 스레드가 종료된 후에도 계속 실행되지만, JUnit 테스트 프레임워크가 테스트 메서드가 종료되면 이 스레드를 강제로 종료

JUnit의 동작 방식
- JUnit은 테스트 메서드가 종료되면 테스트와 관련된 모든 스레드를 정리하고 종료시키려고 한다.
  이는 데몬 스레드든 일반 스레드든 관계없이 적용
- 이는 테스트가 예측 가능한 시간 안에 종료되도록 보장하며, 다음 테스트가 깨끗한 상태에서 실행될 수 있도록 한다.

JUnit에서 스레드 테스트 팁
- join 사용: 스레드가 완료될 때까지 기다리기 위해 Thread.join()을 사용할 수 있다.
- sleep 사용: 스레드가 완료될 때까지 기다리기 위해 Thread.sleep()을 사용할 수 있다.
- 테스트 시간 제한: JUnit의 ```@Timeout`` 어노테이션을 사용하여 테스트 메서드의 실행 시간을 제한할 수 있다.
