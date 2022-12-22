## java.util.Objects 클래스

Objects 클래스는 Object클래스의 보조 클래스로 모든 메서드가 static이다.
* 객체의 비교나 널 체크에 유용

### 1. static boolean isNull(Object obj)
* 해당 객체가 널인지 확인하여 널이면 true, 아니면 false를 반환

### 2. static boolean nonNull(Object obj)
* isNull()과 정반대의 일을 함
* !Objects.isNull(obj)와 동일

### 3. static <T> T requireNonNull(T obj)

```java
static <T> T requireNonNull(T obj)
static <T> T requireNonNull(T obj, String message)
static <T> T requireNonNull(T obj, Supplier<String> messageSupplier)
```

* 해당 객체가 널이 아니어야 하는 경우에 사용
* 만일 객체가 널이면 NullPointerException을 발생시킴
* 두 번째 매개변수로 지정하는 문자열은 예외 메시지가 됨

```java
void setName(String name) {
    if (name == null) {
        throw new NullPointerException("Name must not be null.");
       
    }
    this.name = name;
}

// requireNonNull의 호출만으로 유효성 검사를 할 수 있음
void setName(String name) {
    this.name = Objects.requireNonNull(name, "Name must not be null.");
}
```

### 4. static int compare(Object a, Object b, Comparator c)

Object클래스에는 equals()만 있고 대소비교를 위한 compare가 없는 반면 Objects 클래스에는 존재!!
* a와 b 객체를 비교하고, 두 비교대상이 같으면 0, 크면 양수, 작으면 음수를 반환
* 이 메서드는 a와 b를 비교하는데 사용할 비교 기준이 필요한데, 그 역할을 Comparator가 함

### 5. static boolean equals(Object a, Object b)

Object 클래스에 정의된 equals()와 다른 점 : **null 검사를 하지 않아도 됨**

```java
if (a != null && a.equals(b){ 
    // Object클래스의 equals() : a가 null인지 반드시 확인해야 함
    ...
}
```

```java
public static boolean equals(Object a, Object b) {
    return (a == b) || (a != null && a.equals(b));
}

if (Objects.equals(a, b) {
    // 매개변수의 값이 null인지 확인할 필요가 X
}	
```

* equals()의 내부에서 a와 b의 널 검사를 하기 때문에 따로 널 검사를 위한 조건식을 넣지 않아도 됨
* a와 b 모두가 null인 경우 참을 반환

### 6. static boolean deepEquals(Object a, Object b)

```java
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Objects.equals(str2D, str2D2));		    // false
System.out.println(Objects.deepEquals(str2D, str2D2));		// true
```

* 2차원 문자열 배열을 비교할 때는 equals()만으로는 비교할 수 없음
* equals()와 반복문을 같이 사용해야하는데, deepEquals()를 사용하면 쉬워
