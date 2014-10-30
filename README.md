PHP와 MYSQL의 연동
============

MYSQLi를 이용한 연동
----
#### __DB에 연결하기__<br>
모든 DB작업을 수행하기 전에 먼저 DB에 연결해야 합니다.<br>
DB에 연결하는 작업은 mysqli_connect함수를 통해서 수행할 수  있습니다.
```PHP
$mysql = mysqli_connect(
  $servername, $username, $password, $dbname);
if(!$mysql)
  echo "connection failed";
```
접속할 서버의 주소와, 유저 아이디, 비밀번호, DB이름을 적어주어야 합니다.<br>
접속에 성공하면 mysqli 오브젝트를, 실패할 경우에는 null을 리턴합니다.<br>
<br>
#### __DB 연결 종료하기__<br>
모든 DB작업이 끝나면 연결을 종료해야 합니다.<br>
연결 종료는 mysqli_close함수를 통해 수행합니다.<br>
```PHP
mysqli_close($mysql);
```
<br>
#### __쿼리 실행하기__<br>
DB에 연결이 되었다면, DB에 쿼리를 보내 실행시킬 수 있습니다.<br>
mysqli_query 함수는 접속된 연결에 쿼리를 보내 실행하며, 결과를 가져옵니다<br>
```PHP
$query = "SELECT * FROM accounts";
$result = mysqli_query($mysql, $query);
```
<br>
#### __결과 가져오기__<br>
이제 mysql_query함수가 반환한 result에서 결과들을 가져와야 합니다.<br>
결과는 row 단위로 반환되며, 결과 row는 0개일수도, 1개 이상일수도 있습니다.<br>
아래의 코드는 결과가 총 몇 개의 row로 구성되어있는지 가져옵니다.
```PHP
$rows = mysqli_num_rows($result);
echo "{$rows}개의 결과 row가 있습니다.";
```
<br>
아래의 코드는 row를 하나 가져옵니다. <br>
여러개의 결과 row들 중 0번째부터 순차적으로 가져오게 되며, 더 이상 row가 없을 경우에는 null을 반환합니다.<br>
```PHP
$row = mysqli_fetch_assoc($result);
```
마지막으로 row에서 각 column에 대한 데이터를 가져오는 일만 남았습니다.<br>
아래의 코드는 row에서 값을 가져옵니다.<br>
```PHP
echo "id : {$row["id"]} / nickname : {$row["nickname"]}";
```
<br>
ORM을 이용한 연동
----
ORM은 Object Relational Mapping의 약자로,<br>
SQL을 이용하여 제어하는 것이 아니라, DB의 테이블을 PHP의 오브젝트에 직접 매핑하여 접근하는 방식입니다.<br>
ORM에는 여러가지 구현체가 있으며, 그 중 [idiorm](https://github.com/j4mie/idiorm)을 사용하여 DB작업해보겠습니다.<br>
<br>
#### __SELECT 쿼리 사용하기__<br>
for_table 메소드를 이용해 접근할 테이블 이름을 지정하고,<br>
where 메소드를 이용하여 검색할 컬럼 명과, 검색할 값을 지정합니다.<br>
마지막으로 find_one 메소드를 이용하여 검색을 수행합니다.
```PHP
$result = ORM::for_table("accounts")
  ->where("id", "john")
  ->find_one();
```
만약 찾고자 하는 결과 row가 복수개라면 find_many 메소드를 이용합니다.
```PHP
$result = ORM::for_table("accounts")
  ->where("nickname", "pjc")
  ->find_many();
```
<br>
#### __UPDATE 쿼리 사용하기__<br>
find 메소드를 사용해 검색한 결과값에 값을 수정하고,<br>
save 메소드를 사용하여 다시 DB에 반영할 수 있습니다.
```PHP
$result = ORM::for_table("accounts")
  ->where("id", "john")
  ->find_one();
  
$result->nickname = "hello";

$result->save();
```
#### __DELETE 쿼리 사용하기__<br>
find 메소드를 사용해 검색한 결과값을 지우고 싶을 때는<br>
delete 메소드를 사용합니다.
```PHP
$result = ORM::for_table("accounts")
  ->where("id", "john")
  ->find_one();

$result->delete();
```
#### __INSERT 쿼리 사용하기__<br>
테이블에 새 값을 넣을 때는 create 메소드를 이용합니다.<br>
for_table 메소드로 값을 생성할 테이블을 지정한 뒤, create메소드를 호출합니다.
```PHP
$row = ORM::for_table("accounts")
  ->create();
```
생성된 row에 값을 설정합니다.
```PHP
$row->id = "park";
$row->nickname = "hello world";
```
마지막으로 save 메소드를 호출하면 DB에 값이 반영되게 됩니다.
```PHP
$row->save();
```
