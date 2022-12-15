# java.lang 패키지

## Object 클래스

![image](https://user-images.githubusercontent.com/97823928/161383757-09acf47d-b859-4910-b01b-7709eae77a75.png)

### 📌 equals(Object obj)

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

* 매개변수로 객체의 참조변수를 받아서 비교 후 그 결과를 boolean값으로 알려줌
* 그렇기 때문에 서로 다른 두 객체를 equals 메서드로 비교하면 항상 false로 결과를 얻게 된다.

```java
class EqualsEx1 {
    public static void main(String[] args) {
        Value v1 = new Value(10);
        Value v2 = new Value(10);
        
        if (v1.equals(v2))
            System.out.println("v1과 v2는 같습니다.");
        else
            System.out.println("v1과 v2는 다릅니다.");
        
        v2 = v1;
        
        if (v1.equals(v2))
            System.out.println("v1과 v2는 같습니다.");
        else
            System.out.println("v1과 v2는 다릅니다.");          
    }
}

class Value {
    int value;
    
    Value(int value) {
        this.value = value;
    }
}
```

|변수명|주소값|value값|
|--|------|--|
|v1|0x1234|10|
|v2|0x2345|10|

* 두개의 Value 인스턴스를 생성 후 equlas메서드를 통해서 비교했음
  * equals 메서드는 주솟값으로 비교를 하기 때문에 두 인스턴스의 값이 10으로 같더라도 비교 결과는 false; 
* ```v1 == v2``` : false
* ```v2 = v1 수행 후 v1 == v2``` : true 
  * v2에 v1이 참조하는 인스턴스의 주소값이 저장되므로 서로 같은 주소값이 저장되므로 !
* equals의 값이 같으려면 주솟값(객체) 같아야함!!

#### 그렇다면 value값을 비교하도록 할 순 없을까?
→ equal 메서드를 객체에 저장된 내용을 비교하도록 오버라이딩 하자

```java
public boolean equals(Object obj) {
    if(obj != null && obj instanceof Person) {
        return id == ((Person)obj).id;
    }
    else {
        return false;
    }
}
```

### 📌 hashCode()

해시함수는 찾고자하는 값을 입력하면 그 값이 저장된 ```위치```를 알려주는 **해시코드**를 반환
* 일반적으로 해시코드가 같은 두 객체가 존재하는 것이 가능
* 앞서 살펴본 것처럼 클래스의 인스턴스 변수 값으로 객체의 같고 다름을 판단하는 경우 hashCode메서드도 오버라이딩해줘야함..!

```java
class HashCodeEx1 {
    public static void main(String[] args) {
        String str1 = new String("abc");
        String str2 = new String("abc");
        
        //String 클래스는 문자열의 내용이 같으면 동일한 해시코드를 반환하도록 hashCode메서드가 오버라이딩 되어있음
        System.out.println(str1.equals(str2)); //true
        System.out.println(str1.hashCode()); //96354
        System.out.println(str2.hashCode()); //96354
        
        //identityHashCode()는 객체의 주소값으로 해시코드를 생성하므로 모든 객체에 대해 항상 다른 해시코드값을 반환
        System.out.println(System.identityHashCode(str1)); //27134973
        System.out.println(System.identityHashCode(str2)); //1284693
    }
}
```


### 📌 toString()

```java
class CardToString {
    Card c1 = new Card();
    Card c2 = new Card();
    
    c1.toString(); // Card@19e0bfd
    c2.toString(); // Card@139a55
}
```
* Card 클래스의 경우 Object클래스의 toString을 호출했기 때문에 클래스이름과 해시코드를 출력
* toString()을 (오버라이딩 하지않았을 경우) 호출하면 ```클래스이름@16진수의해시코드```를 반환
* 하지만 String 클래스의 경우에는 인스턴스가 갖고 있는 문자열을 반환하도록 오버라이딩 되어있어서 클래스 이름과 해시코드 대신 그 문자열을 출력한다.

### 📌 clone()

clone()은 자신을 복제하여 새로운 인스턴스를 생성하는 역할
* 보통 원래의 인스턴스는 보존하고 clone메서드를 통해 새로운 인스턴스를 생성하여 작업을 하면 이전의 값이 보존되어야 하는데...
* Object클래스의 clone의 경우에는 단순히 인스턴스변수의 값만 복사하기 때문에 완전한 복제가 이루어지지 않는다.
  * 배열의 경우 복제된 인스턴스도 동일한 배열의 주소를 갖기 때문에 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미치게 된다
  * 그러므로 clone메서드의 오버라이딩이 필요하다...!! 

```java
class Point implements Cloneable {
    int x, y;
    
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public String toString() {
        return "x=" + x + ", y=" + y;
    }
    
    // clone메서드의 접근제어자는 public!!
    // 조상에 정의된 메서드를 자손에서 오버라이딩할 때는 조상의 접근범위와 같거나 더 넓어야 하므로 
    public Object clone() {
        Object obj = null;
        try {
            obj = super.clone(); // 조상 클래스의 clone()을 호출
        } catch(CloneNotSupportedException e) {} 
        return obj;
    }
}

class CloneEx1 {
    public static void main(String[] args) {
        Point original = new Point(3, 5);
        Point copy = (Point)original.clone();
        System.out.println(original); // x=3. y=5
        Sysstem.out.println(copy);    // x=3, y=5
    }
}
```

#### clone()을 사용하려면
1. 복제할 클래스가 Cloneable 인터페이스를 구현해야함
* Cloneable 인터페이스를 구현했다 == 클래스 작성자가 복제를 허용한다는 뜻
* 이렇게 함으로써 데이터의 보호가 가능함
2. clone()을 오버라이딩을 하면서 접근 제어자를 protected에서 public으로 변경한다
3. try-catch 내에서 조상클래스의 clone()을 호출한다.

### 공변 반환 타입

공변 반환타입은 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경하는 것을 허용하는 것!
* 공변반환 타입을 사용하면 조상 타입이 아닌 실제로 반환되는 자손 객체의 타입으로 반환할 수 있어 형변환이 줄어든다는 장점이 있다

```java
public Point clone() {
    Object obj = null;
    try {
        obj = super.clone();
    } catch(CloneNotSupportedExceptuon e) {}
    return (Point)obj;
}
```
### 얕은 복사와 깊은 복사

![image](https://user-images.githubusercontent.com/97823928/161388414-7656cc17-7399-43c8-8968-c08c95eb4566.png)


```얕은 복사``` : 원본을 복사하면 복사본도 영향을 받음  
* 단순히 Object 클래스의 clone()을 호출할 뿐
* Object 클래스의 clone()은 원본 객체가 가지고 있는 값만 그대로 복사!!

```java
public Circle shallowCopy() {
    Objct obj = null;
    try {
        obj = super.clone();
    } catch (CloneNotSupportedException e) {}
    return (Circle)obj;
}
```

```깊은 복사``` : 원본가 복사본이 서로 다른 객체를 참조하여 원본의 변경이 복사본에 영향을 미치지 않음

```java
public Circle deepCopy() {
    Objct obj = null;
    try {
        obj = super.clone();
    } catch (CloneNotSupportedException e) {}
    
    Circle c = (Circle)obj;
    c.p = new Point(this.p.x, this.p.y); //복제된 객체가 새로운 Point 인스턴스를 참조하도록 함!
    return c;
}
```
* 복제된 객체가 새로운 Point 인스턴스를 참조하도록 함!
* 원본이 참조하고 있는 객체까지 복사한 것


### 📌 getClass()

자신이 속한 클래스의 ```Class 객체```를 반환하는 메서드

```java
public final class Class implements .. { // Class 클래스

}
```

#### Class 객체가 뭔데 ?

* 클래스의 모든 정보를 담고 있으며, 클래스 당 1개 존재
* 클래스 파일이 클래스 로더에 의해서 메모리에 올라갈 때 자동으로 생성
* 클래스 로더는 실행 시 필요한 클래스를 동적으로 메모리에 로드하는 역할을 함
* 기존에 생성된 클래스 객체가 메모리에; 존재하는지 확인하고 있으면 객체의 참조를, 없으면 클래스 패스에 지정된 경로를 따라서 클래스 파일을 찾는다
* 못찾으면 ClassNotFoundException이 발생하고, 찾으면 해당 클래스 파일을 읽어서 class 객체로 반환

#### Class 객체를 얻는 방법
클래스의 정보가 필요할 때 먼저 Class 객체에 대한 참조를 얻어와야 함

```java
Class obj = new Card().getClass(); //생성된 객체로 부터 얻는 방법
Class obj = Card.class; //클래스 리터럴(*.class)로부터 얻는 방법
Class obj = Class.forName("Card"); //클래스 이름으로 부터 얻는 방법 -> 특정 클래스파일을 메모리에 올릴때 주로 사용
```

```java
Card c = new Card(); // new 연산자로 객체 생성
Card c = Card.class.newInstance(); Class 객체를 이용해서 객체 생성 
```
클래스 객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등에 대한 모든 정보를 얻을 수 있으므로 Class 객체를 통해 객체를 생성하고 메서드를 호출하는 등 보다 동적인 코드를 작성할 수 있다.
