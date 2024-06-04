# 객체의 값 비교 vs 객체의 주솟값 비교 

```text
백준 11652 카드 실버4 풀면서 
로직은 맞는 것 같은데 계속 틀렸었다. 
문제는 객체의 값 비교를 해야 하는데 객체의 주솟값 비교를 했던 게 문제였다.
```

```java
ArrayList<Map.Entry<Long, Integer>> list = new ArrayList<>(map.entrySet());
    Collections.sort(list, new Comparator<Map.Entry<Long, Integer>>() {
        @Override
        public int compare(Map.Entry<Long, Integer> o1, Map.Entry<Long, Integer> o2) {
            if(o1.getValue() > o2.getValue()) return -1;
            else if(o1.getValue() == o2.getValue()){
                if(o1.getKey() > o2.getKey()) return 1;
                else if(o1.getKey() == o2.getKey()) return 0;
                else return -1;
            }
            return 1;
        }
    });
    System.out.println(list.get(0).getKey());
```

왜 문제일까?
- 기본 타입에서의 == 연산자는 값을 비교한다.
- 하지만 참조 타입에서의 == 연산자는 주솟값을 비교한다.
- 객체의 값을 비교하는 게 아니라 객체의 주소를 계속 비교했으니 틀렸던 것이다. 

그렇다면 객체의 값은 뭐로 비교해야 할까?
- compareTo 메서드를 사용
- 대소 비교 연산자 >, <
- +,- 연산 이후 ==



