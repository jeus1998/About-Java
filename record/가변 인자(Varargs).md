# 가변 인자(Varargs) 

### 예제 코드 

test()
```java
void test(String itemName, Integer maxPrice, Item... items) {
    List<Item> result = itemRepository.findAll(new ItemSearchCond(itemName, maxPrice));
    assertThat(result).containsExactly(items);
}
```
- ``Item... items``는 가변 인자를 의미하며, 호출 시 여러 개의 Item 객체를 전달할 수 있다.

test() 호출
```java
Item item1 = new Item("item1", 100);
Item item2 = new Item("item2", 200);
Item item3 = new Item("item3", 300);

test("item", 300, item1, item2, item3);
test("item", 100, item1, item2);
test("item", 100, item2);
```

### 가변 인자 내부 동작, 주의 사항 

내부 동작 
- 가변 인자를 사용하는 메서드는 내부적으로 배열로 처리
- ``Item... items``는 실제로는 ``Item[] items``와 같으며, 전달된 인수들은 배열로 변환

주의 사항 
```text
1. 가변 인자는 마지막 매개변수로만 사용 가능

// 올바른 예
void correctMethod(String name, Item... items) { }

// 잘못된 예 - 가변 인자가 마지막 매개변수가 아님
void incorrectMethod(Item... items, String name) { }

2. 가변 인자와 다른 매개변수를 함께 사용할 수 있음

void mixedParameters(String name, Integer maxPrice, Item... items) {
    //  생략 ..
}

3. 가변 인자를 전달하지 않을 수도 있음

test("item", 300); // items 배열은 빈 배열로 처리
```
