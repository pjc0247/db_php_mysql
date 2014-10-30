PHP와 MYSQL의 연동
============

MYSQLi를 이용한 연동
----
__DB에 연결하기__<br>
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
__DB 연결 종료하기__<br>
모든 DB작업이 끝나면 연결을 종료해야 합니다.<br>
연결 종료는 mysqli_close함수를 통해 수행합니다.<br>
```PHP
mysqli_close($mysql);
```
<br>
<br>
__쿼리 실행하기__<br>
DB에 연결이 되었다면, DB에 쿼리를 보내 실행시킬 수 있습니다.<br>
mysqli_query 함수는 접속된 연결에 쿼리를 보내 실행하며, 결과를 가져옵니다<br>
```PHP
$query = "SELECT * FROM accounts";
$result = mysqli_query($mysql, $query);
```
<br>
__결과 가져오기__<br>
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
