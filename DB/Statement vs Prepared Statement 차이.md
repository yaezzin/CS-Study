# Statement와 Prepared Statement

SQL은 실행 될 때 마다

1. 쿼리 문장 분석 (Parsing)
2. 컴파일 (Compile)
3. 실행 (Execute)

3가지 단계를 거친다.
그러나, 이름에서 알 수 있는 것 과 같이 Statement는 쿼리문을 수행할 때 마다 3단계를 모두 거치는 것과 달리, Prepared Statement의 경우 미리 컴파일이 되어있어 매번 3단계를 모두 실행하지 않아도 된다.

## Statement
<pre>
<code>
String sqlstr = "SELECT name, memo FROM TABLE WHERE name =" + num
Statement stmt = conn.createStatement();
ResultSet rst = stmt.executeQuery(sqlstr);
</code>
</pre>

이런 쿼리가 있다고 가정해보자, executeQuery() 나 executeUpdate() 를 실행하는 시점에 파라미터로 SQL문을 전달하는데, 이 때 전달되는 쿼리는 완성된 형태로 한눈에 쿼리를 파악하기 쉽다. 하지만, 쿼리를 수행하는 과정에서 매번 컴파일을 하기 때문에 Prepared Statement에 비해 효율성이 떨어진다 .

<hr/>

## Prepared Statement

### Prepared Statement란?
데이터베이스 관리 시스템(DBMS)에서 동일하거나 비슷한 데이터베이스 문을 높은 효율성으로 반복적으로 실행하기 위해 사용되는 기능을 말한다.

### Prepared Statement는 어떤식으로 동작할까?

1. 준비(Prepare): 먼저 애플리케이션은 쿼리의 뼈대만을 만들고 이를 DBMS로 보낸다. 특정값은 지정하지 않은 채로 남겨진다<pre><code>
INSERT INTO products (name, age) VALUES (?, ?);
</code>
</pre>

2. 컴파일(Compile): 그 다음, DBMS는 쿼리의 뼈대를 컴파일하며(최적화 및 변환) 아직 실행하지 않고 결과만 저장한다.

3. 실행(Execute): 나중에 애플리케이션이 틀의 변수에 값(바인드)을 지정하면 DBMS는 (결과를 반환할 수도 있는) 쿼리를 실행한다. 애플리케이션은 여러 값으로 원하는 횟수만큼 쿼리를 실행할 수 있다. 위의 예에서 첫 번째 변수로 "Paul"로, 두 번째 변수로 "20"을 지정한다.

예를 들어보자, 
<pre>
<code>
String sqlstr = "SELECT name, memo FROM TABLE WHERE num = ?"
PreparedStatement stmt = conn.preparedStatement();
stmt.setInt(1, num);
ResultSet rst = stmt.executeQuery(sqlstr);
</code>
</pre>


처음 예시와는 달리 Prepared Statement는 쿼리를 실행 할 때 틀을 미리 만들어 이를 Parsing한 다음 결과를 캐싱한다.

이후에, ?에 들어갈 변수를 입력받으면 bind, execute, patch하여 쿼리를 수행한다.
매번 쿼리를 수행할 때 마다, 캐시에 저장되어 있는 부분을 사용하는 만큼 Statement보다 성능우위를 점할 수 있다.

또한, 바인딩 된 데이터는 SQL 문법이 아닌 내부의 인터프리터나 컴파일 언어로 처리하므로 문법적인 의미를 가질 수 없어 binding 단계에서 입력된 부분은 쿼리로 인식되지 않는다.
즉, SQL injection에 대응할 수 있다.


