
# Optional get() vs orElse()

### get()

- Optional 객체가 값을 가지고 있을 때 그 값을 반환
- Optional 객체가 비어 있을 경우 (Optional.empty()).get() ➡️ NoSuchElementException 예외를 던진다.

```java
Optional<String> optional = Optional.of("Hello, World!");
String value = optional.get(); // "Hello, World!" 반환

Optional<String> emptyOptional = Optional.empty();
String value = emptyOptional.get(); // NoSuchElementException 발생
```
- Optional 비어 있을 때 호출하면 NoSuchElementException
- 따라서 .get()을 사용하기 전에 isPresent()로 값이 존재하는지 확인하는 것이 안전한 방법이다.

### orElse()

- Optional 객체가 비어 있는 경우 제공된 기본 값을 반환
- Optional 객체가 값을 가지고 있을 때는 그 값을 반환하고, 비어 있을 때는 기본 값을 반환
```java
public String getGreeting(Optional<String> name) {
    return "Hello, " + name.orElse("Guest");
}

// 사용 예시:
Optional<String> name = Optional.of("Alice");
System.out.println(getGreeting(name)); // "Hello, Alice"

Optional<String> emptyName = Optional.empty();
System.out.println(getGreeting(emptyName)); // "Hello, Guest"
```

### 예시  

```java
@Service
@RequiredArgsConstructor
public class LoginService {
    private final MemberRepository memberRepository;
    /**
     * @return null 로그인 실패
     */
    public Member login(String loginId, String password){
        return memberRepository.findByLoginId(loginId)
                .filter(m -> m.getPassword().equals(password))
                .get();
    }
}
```

- memberRepository에서 findByLoginId를 통해 사용자가 없으면 Optional.empty()를 반환 
  - get() ➡️ NoSuchElementException
- filter(m -> m.getPassword().equals(password))에서 비밀번호가 일치하지 않으면 Optional.empty()를 반환 
  - get() ➡️ NoSuchElementException

리팩토링

```java
@Service
@RequiredArgsConstructor
public class LoginService {
    private final MemberRepository memberRepository;
    /**
     * @return null 로그인 실패
     */
    public Member login(String loginId, String password){
        return memberRepository.findByLoginId(loginId)
                .filter(m -> m.getPassword().equals(password))
                .orElse(null);
    }
}
```
- default value 활용 
- Optional 비어 있지 않으면 Member 객체를 반환
- Optional 비어 있으면 null 반환
- NoSuchElementException 발생 ❌


