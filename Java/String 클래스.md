## String 클래스

다른 언어에서는 문자열을 char형의 배열로 다루었지만 자바에서는 문자열을 위한 클래스인 String 클래스를 제공한다.

## 변경 불가능한 클래스

```java
public final class String implements java.io.Serializable, Comparable {
    private char[] value;
    
    // ...
}
```
* String 클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수 ```char[] value```를 인스턴스 변수로 정의해놓고 있다.
* 인스턴스 생성 시 생성자의 매개변수로 받는 문자열은 value에 문자형 배열로 저장된다.

<img width="1013" alt="스크린샷 2022-12-15 오후 6 49 37" src="https://user-images.githubusercontent.com/97823928/207827735-43ea8871-420e-43ea-9d05-b7732d6d7ace.png">

한번 생성된 String인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수 없다!
* '+'를 이용하여 문자열을 결합하는 것은 매 연산마다 새로운 문자열을 가진 String 인스턴스가 생성되어 메모리 공간을 차지하는 것!
* 그러므로 메모리 공간을 차지하게 되므로 가능한 결합횟수를 줄이거나, StringBuffer 클래스를 이용하자

## 문자열의 비교

문자열을 만들 때는 ```문자열 리터럴을 지정하는 방법```, ```String 클래스의 생성자를 사용해서 만드는 방법```이 있음
* String클래스의 생성자를 이용하는 경우 new를 통해 메모리할당이 이루어지기 떄문에 항상 새로운 인스턴스가 생성
* 반면 문자열 리터럴은 이미 존재하는 것을 재사용하는 것!

```java
String str1 = "abc"; // 문자열 리터럴 abc의 주소가 str1에 저장
String str2 = "abc";
String str3 = new String("abc"); // 새로운 String 인스턴스 생성
String str4 = new String("abc");
```

위의 코드가 실행되었을 때의 상황은 다음과 같다!

<img width="615" alt="스크린샷 2022-04-05 오후 7 13 51" src="https://user-images.githubusercontent.com/97823928/161732111-f50b5c1c-89e9-4141-a571-69597b60b99f.png">

* ```equals()```는 두 문자열의 내용을 비교
  * str1 == str2 : true
  * str1.equals(str2) : true   
* ```==```는 인스턴스의 주소를 비교
  * str3 == str4 : false
  * str3.equals(str4) : true  

## 문자열 리터럴

## 1. 빈문자열

자바는 길이가 0인 배열을 생성할 수 있음!

```java
char[] chArr = new char[0];
int[]  iArr = {};
String s = "" ;  // 빈 문자열로 초기화
char c = ' ' ;  // 공백으로 초기화
```
* String s = ""와 같은 문장은 참조변수s가 참조하고 있는 String 인스턴스는 new char[0]처럼 길이가 0인 char형 배열을 저장하는 것
* 일반적으로 각 타입의 기본값으로 초기화 하지만 String과 char은 다음과 같이 "", ' '처럼 초기화함!

## 생성자와 메서드
[여기를 누르시오!](https://github.com/yaezzin/TIL/tree/main/Java/Java%EC%9D%98%20%EC%A0%95%EC%84%9D/9.%20java.lang%ED%8C%A8%ED%82%A4%EC%A7%80%EC%99%80%20%EC%9C%A0%EC%9A%A9%ED%95%9C%20%ED%81%B4%EB%9E%98%EC%8A%A4)

## join()과 StringJoiner

#### join()

```java
String animals = "dog,cat,bear";
String[] arr = animals.split(","); // 문자열을 ','를 구분자로 나눠서 배열에 저장
String str = String.join("_", arr); // 배열의 문자열을 '-'로 구분해서 결합
System.out.println(str); //dog-cat-bear
```

#### StringJoiner

StringJoiner의 경우 JDK 1.8부터 추가 됨

```java
StringJoiner sj = new StringJoiner(",", "[", "]");
String[] strArr = {"aaa", "bbb", "ccc"};

for(String s : strArr)
    sj.add(s.toUpperCase());
    
System.out.println(sj.toString()); // [AAA,BBB,CCC]
```

## 유니코드의 보충 문자

> 문자를 다루는 메서드인데 매개변수 타입이 왜 int일까?

유니코드는 원래 2바이트, 즉 16비트 문자체계인데 이걸로 모자라서 20비트로 확장하였음..  
그래서 하나의 문자를 char타입으로 다루지 못하고 int타입으로 다룰 수 밖에 없음!

확장에 의해서 새로 추가된 문자들을 ```보충 문자```라 하는데,
이를 구별하는 방법은 매개변수가 int인 경우 보충문자를 지원한다고 생각하면 됨 

## 문자 인코딩 변환

## String.format()

: 형식화된 문자열을 만드는 방법, printf()하고 사용법이 동일

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5);
System.out.println(str); //3 더하기 5는 8입니다.
```

## 기본형 값을 String으로 변환    

숫자로 이루어진 문자열을 숫자로, 또는 그 반대로 변환하는 경우 
* '+' 또는 ValueOf() 이용하자
* 서

```java
int i = 100;
String str1 = i + ""; //100을 "100"으로 변환
String str2 = String.ValueOf(i); //100을 "100"으로 변환
```

## String을 기본형으로 변환

* parseInt() 또는 ValueOf() 이용하자

```java
int i = Integer.parseInt("100");
int i2 = Integer.valueOf("100");
```
