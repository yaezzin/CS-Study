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
 
## Random 클래스

난수를 얻는 방법에는 Math.random()과 Random()이 존재!
* 사실 Math.random()는 내부적으로 Random 클래스의 인스ㅓㄴ스를 생성해서 난수를 생성하므로 둘중에 편한걸 사용하자!

```java
double randNum = Math.random(); // 방법1
double randNum = new Random().nextDouble(); // 방법2
```

#### Q, 1~6 사이의 정수를 난수로 얻고자 한다면?

```java
int num = (int)(Math.random() * 6) + 1;
int num = new Random().nextInt(6) + 1; // nextInt(6)는 0~6 사이의 정수값을 반환
```

## Random클래스의 생성자와 메서드

생성자 Random()은 종자값을 ```System.currentTimeMillis()```로 하기 때문에 실행할 때마다 얻는 난수가 달라짐
* ```System.currentTimeMillis()``` : 현재시간을 천분의 1초단위로 변환해서 반환

```java
public Random() {
    this(System.currentTimeMillis());	//Random(long seed)를 호출한다.
}
```

> 생성자 Random은 같은 종자값을 사용하면 ```같은 값들을 같은 순서로 얻음```
* 종자값(seed) : 난수를 만드는 공식에 사용되는 값으로, 같은 종자값을 넣으면 같은 난수를 얻게 됨
* 종자값이 같은 Random 인스턴스들은 항상 같은 난수를 같은 순서대로 반환

```java
Random r1 = new Random(5);
Random r2 = new Random(5); 

// r1과 r2는 동일한 값들을 순서대로 반환하게 됨
```
    
## java.util.regex 패키지

정규식이란 텍스트 데이터 중에서 원하는 조건, 패턴과 일치하는 문자열을 찾아내기 위해 사용!
* 즉, 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 말함
* 정규식을 이용하면 많은 양의 텍스트 파일 중에서 원하는 데이터를 손쉽게 뽑아낼 수 있고, 형식 체크도 가능
 
```java
Pattern p = Pattenr.compile("c[a-z]*");
Matcher m - p.matcher(data[i]);
if (m.matches()) { ... }
```
 
> Pattern은 정규식을 정의하는데 사용, Matcher는 정규식을 데이터와 비교하는 역할을 함
1. 정규식을 매개변수로 Pattern 클래스의 static메서드인 ```Pattern compile(String regex)```를 호출해서 Pattern 인스턴스를 얻음
2. 정규식으로 비교할 대상을 매개변수로 Pattern 클래스의 ```Matcher matcher(CharSequence input)```를 호출해서 Matcher 인스턴스를 얻음
3. Matcher 인스턴스에 boolean matches()를 호출해서 정규식에 부합하는지 확인   
    
> find()는 주어진 소스 내에서 패턴과 일치하는 부분을 찾아내면 true를 찾지못하면 false를 리턴
* find()룰 호출하여 패턴과 일치하는 부분을 찾아낸 후 find()를 다시 호출하면 이전에 발견한 패턴과 일치하는 부분의 다음부터 다시 패턴매칭을 시작할 수 있음
* 책에 많은 예시가 있다!!!!
    
## Scanner 클래스

Scanner는 화면, 파일, 문자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 주며, JDK1.5부터 추가 됨

```java
Scanner(String source)  
Scanner(File source)    
Scanner(InputStream source)    
Scanner(Readable source)    
Scanner(ReadableByteChannel source)    
Scanner(Patch source)   // JDK 1.7부터 추가
```
    
Scanner 덕분에 좀 더 간편하게 화면으로 부터 입력받을 수 있게 됨
* JDK 1.6부터는 화면 입출면만 전문적으로 담당하는 java.io.Console이 새로 추가됨
* 이클립스와 같은 IDE에서 잘 동작하지 않기 때문에 Scanner를 주로 사용하는데, 사용법이나 성능측면에서는 거의 동일
    
```java
Scanner s = new Scaaner(System.in);
String input = s.nextInt();

Console console = System.console();
String input = console.readLine();
```
    
