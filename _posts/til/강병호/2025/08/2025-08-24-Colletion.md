 ---
 title: 2025-08-24
 author: 강병호
 date: 2025-08-24
 category: TIL/강병호/2025/08
 layout: post
 ---



 # 컬렉션 팩토리

자바 9로 업데이트가 되면서 작은 컬렉션 객체를 쉽게 만들 수 잇는 방법을 제공합니다.


    ```
    // 기존
    List<String> friends = new ArrayList<>();
	friends.add("Raphael");
	friends.add("Olivia");
	friends.add("Thibaut");
	
	// Arrays.asList() 팩토리 메서드 사용
	List<String> friends = Arrays.asList("Raphael", "Olivia", "Thibaut")
    ```


#### UnsupportedOperationException 예외 발생

위의 방식처럼 코드를 간단하게 줄일 수 있지만 이런 경우에는 고정길이로 리스트를 생성하기에 새 요소를 추가하거나 삭제할 수 없다.

## 리스트 팩토리

`List.of` 팩토리 메서드를 통해 다음과 같이 간단하게 리스트를 생성할 수 있다.


    ```
    List<String> friends = List.of("Raphael", "Olivia", "Thibaut");
    ```


해당하는 코드에 `friends.add("Kang")`을 수행하면 `java.lang.UnsupportedOperationException`이 발생하게 된다. 



## 집합 팩토리

다음의 방식으로 고정길이의 집합을 생성한다.


    ```
    Set<String> friends = Set.of("Raphael", "Olivia", "Thibaut");
	```

중복된 요소로 Set을 생성한다면 `IllegalArgumentException`이 발생한다.

## 맵 팩토리

다음 두 가지 방식을 통해 고정 길이의 맵을 초기화할 수 있다.


    ```
    Map<String, Integer> ageOfFriends = Map.of("Raphael", 30, "Olivia", 25, "Thibaut", 26);
    
    import static java.util.Map.enty;
    Map<String, Integer> ageOfFriends2 = Map.ofEnties(
	    entry("Raphael", 30),
	    entry("Olivia", 25),
	    entry("Thibaut", 26),
	)
    ```


# 리스트와 집합 처리

자바 8에서는 List, Set 인터페이스에 컬렉션 자체를 변경하는 다음의 메서드가 추가도니다.

- `removeIf` : 프레디케이트를 만족하는 요소를 제거한다. `List`나 `Set`을 구현하거나 그 구현을 상속받은 모든 클래스에서 이요가능하다.
- `replaceAll` : 리스트에서 사용하는 기능으로 `UnaryOperator` 함수를 이용해 요소를 변경한다.
- `sort` : `List`인터페이스에서 제공하는 기능으로 정렬처리를 수행한다.

### removeIf 메서드

다음의 트랙잭션을 삭제하는 코드는 `ConcurrentModificationException`을 일으킨다. 그 이유는 `for-each` 루프가 `Iterator`객체를 사용하기 때문이다.


    ```
    for (Transaction transaction : transactions) {
	    if (Character.isDigit(transaction.getReferenceCode().charAt(0))) {
		    transactions.remove(transaction);
	    }
    }
    // 실제 구현 코드
    for (Iterator<Tranasction> iterator = transactions.iterator(); iterator.hasNext();) {
    if (Chracter.isDigit(transaction.getReferenceCode().charAt(0))) {
	    transactions.remove(transction); // 반복하면서 별도의 두 객체를 통해 컬렉션을 변경
	    }
    }
    ```

위 방식은 다음의 방식을 컬렉션을 관리한다.
- `Iterator` 객체, `next()`, `hasNext()` 를 이용해 소스에 질의한다.
- `Collection` 객체 자체, `remove()`를 호출해 요소를 삭제한다.

이는 반복자의 상태가 컬렉션의 상태와 동기화되지 않음을 의미한다.
이러한 문제를 해결하기 위해 다음의 방식으로 해결할 수있으며 이는 자바 8의 `removeIf` 메서드와 호환이 되며 위에서 발생가능한 에러를 방지할 수 있다.


    ```
    for (Iterator<Tranasction> iterator = transactions.iterator(); iterator.hasNext();) {
    if (Chracter.isDigit(transaction.getReferenceCode().charAt(0))) {
		iterator.remove();
	    }
    }
    // removeIf 사용
    transactions.removeIf(transction -> Chracter.isDigit(transction.getReferenceCode().charAt(0)));
    ```


### replaceAll 메서드

`replaceAll`메서드를 이용해 리스트의 각 요소를 새로운 요소로 변경할 수 있다


기존 컬렉션을 바꾸는 방법으로 다음과 같이 `ListIterator`객체를 이용하여 처리할 수 있다.


    ```
    for (ListIterator<Stirng> iterator = referenceCodes.listIterator(); iterator.hasNext(); ) {
    String code = iterator.next();
    iterator.set(Character.toUpperCase(code.charAt(0)) + code.substring(1));
    }
    ```

이를 다음과 같이 `replaceAll`를 통해 다음과 같이 간단하게 구현할 수 있다.


    ```
	referenceCodes.replaceAll(code -> Character.toUpperCase(code.charAt(0)) + code.subsring(1));
	```
