## Iterator, ListIterator, Enumeration

Iterator, ListIterator, Enumeration은 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스이다.
* Enumeration은 Iterator의 구버전, ListIterator는 Iterator의 기능을 향상 시킨 것이다.

### 1. Iterator

```java
public interface Iterator {
    boolean hasNext();
    Object next();
    void remvoe();
}

public interface Collection {
    public Iterator iterator();
}
```

컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator 인터페이스를 정의하고, Collection 인터페이스에는 Iterator를 반환하는 iterator()를 정의하고 있다.
* iterator()은 컬렉션 인터페이스에 정의된 메서드 이므로 컬렉션 인터페이스의 자손인 Set, List에도 포함되어있다.
* 그래서 List, Set 인터페이스를 구현하는 컬렉션은 iterator()가 각 컬렉션의 특징에 맞게 작성되어있다. 

#### ArrayList에 저장된 요소들을 출력하는 예시

```java
Collection c = new ArrayList();
Iterator it = c.iterator();

while(it.hasNext()) {
    System.out.println(it.next());
}
```

* ArrayList 대신 컬렉션 인터페이스를 구현한 다른 컬렉션 클래스에 대해서도 동일한 코드를 사용할 수 있다.
* 참조변수의 타입을 ArrayList가 아닌 Collection으로 한 이유 ?
  * Collection 인터페이스를 구현한 다른 클래스인 LinkedList로 변경할 때 코드 검사를 할 필요 X
  * ArrayList로 선언해두었다면 정의되지 않는 메서드를 호출했을 수도 있기 떄문에 코드 검사가 필요함

#### Map

Map 인터페이스를 구현한 컬렉션 클래스는 키와 값을 쌍으로 저장하므로 iterator()를 직접 호출할 수 었다.
* 대신 keySet()이나 entrySet()과 같은 메서드를 통해 키와 값을 각각 얻어온 다음 iterator()를 호출해야 Iterator를 얻을 수 있다.

```java
Map map = new Map();
Iterator it = map.entrySet().iterator();
```

### 2. ListIterator, Enumeration

* Enumeration은 컬렉션 프레임웍이 만들어지기 전에 사용하던 것으로 Iterator의 구버전이다.
* 이전 버전으로 작성된 소스와의 호환을 위해서 남겨 두고 있을 뿐 가능하면 Iterator를 사용하자
* ListIterator은 Iterator를 상속받아서 기능을 추가한 것으로, 양방향으로의 이동이 가능하다.
* 다만 List 인터페이스를 구현한 컬렉션에서만 사용할 수 있다.

```java
ArrayList list = new ArrayList();
list.add("1");

ListIterator it = list.listIterator();
while(it.hasNest()) {
    // ...
}

while(it.hasPrevious()) {
    // ...
}
```
* Iterator은 단방향으로만 이동하므로 컬렉션의 마지막 요소에 다다르면 더이상 사용할 수 없으나, ListIterator은 양방향으로 이동하므로 각 요소간의 이동이 자유롭다.
* 하지만 이동하기전 hasNext(), hasPrevious()를 통해 이동여부에 대해 확인해주어야 한다.

#### 선택적 기능

```java
public void remove() {
    throw new UnsupportedOperationException();
}
```

ListIterator의 메서드에서는 반드시 구현할 필요가 없는 메서드가 있다.
* 예를 들어 remove()는 구현하지 않아도 되는데, 인터페이스로부터 상속받은 메서드는 추상메서드라 메서드의 몸통을 반드시 만들어 주어야 한다.
* 몸통부분을 { } 비어두는 것보다, 예외를 던져서 구현되지 않은 기능이라는 것을 메서드를 호출하는 쪽에 알려주자!
* 그렇지 않으면 호출하는 쪽에서 소스를 구현해보기 전까지는 이 기능이 바르게 동작하지 않는 이유를 알 방법이 없기 떄문이다.
