# 2주차
SQLite Browser : SQL을 경험해보기 가장 괜찮은 툴

* Relational Database :
* 용어정리
	1. Database :  많은 테이블을 포함한 개념.
	2. Relation ( or Table) : tuples와 attributes를 포함한 개념.
	3. Tuple ( or Row ) : 일반적으로 한 **객체(사람, 음악 트랙 등등…)**  를 지칭하는 필드들의 집합. 
	4. Attribute ( or Column, Field ) : 한 객체(object)에서 가질 수 있는 여러가지 속성들(elements) 중 하나.
	![](week2/screenshot%202019-04-21%20PM%204.06.09%202.png)

* SQL (Structured Query Language) : 데이터 베이스에 이슈와 명령을 날릴 수 있는 언어.
	* 테이블 생성
	* 데이터 가져오기
	* 데이터 삽입
	* 데이터 삭제

* 대형 프로젝트에서의 데이터 베이스 관련 역할.
	* 개발자 : db 관련 이슈에서 어플리케이션에 해당하는 이슈들을 모니터링 한다.
	* 데이터베이스 관리자 (DBA) : db관련 이슈에서 데이터 베이스 관련 부분을 모니터링 하거나 최적화 시킨다.
	* 두 role 모두 **데이터 모델** 을 구성하는데 참여한다.
![](week2/IMG_3E731D565C78-1%202.jpeg)
![](week2/IMG_D75E594D9D8E-1%202.jpeg)


* 데이터베이스 모델 ( Database Model )  = **DB Schema**: 데이터베이스의 구조, 혹은 포맷.
* 현업에서 사용되는 데이터 베이스들
	* Oracle : 큰 규모, 상업적, 기업 레벨, 상당히 복잡한 구조인 데이터 베이스에서 사용
	* MySql : Scalable에 있어서 가볍지만, 상당히 빠르다. **(상업적으로 사용 가능한 오픈소스)**
	* Sql Server (Microsoft) : Very Nice 하다. Access 측면에서도 (?) 
	* 작은 규모를 위한 db
		* SQLite
		* HSQL
		* Postgress

* SQL 명령어 사용하기.
	* 테이블 생성
```
CREATE TABLE Users {
	name VARCHAR(128),
	email VARCHAR(128),
}		
//CREATE TABLE {테이블 이름} {
//		{Attr 이름} {스키마}(최대 길이),
//		{Attr 이름} {스키마}(최대 길이),
//}			
```
	
	* 삽입

	`INSERT INTO Users (name, email) VALUES (‘Kristin’, ‘kf@umich.edu’)`

	`INSERT INTO {테이블 이름} (Attr1, Attr2) VALUES ({Attr1 value}, {Attr2 Value})`

	* 삭제

	`DELETE FROM Users WHERE email='ted@umich.edu’`
	`DELETE FROM {테이블 이름} WHERE {Attr}={Value}`

	* 검색 ( Select )

	`SELECT * FROM Users`    User테이블로 부터 모두(*) 가져온다.
	`SELECT * FROM Users WHERE email='csev@umich.edu`    User테이블에서 email이 'csev@umich.edu’ 인 Row(Tuple)들을 가져온다.

	* 정렬 ( ORDER BY )

	`SELECT * FROM Users ORDER BY email`    User테이블로 부터 **email attr기준으로 정렬하여** 모두(*) 가져온다.

	`SELECT * FROM Users ORDER BY email desc`    User테이블로 부터 email attr기준으로 **내림차순** 정렬하여 모두(*) 가져온다.

	`SELECT * FROM Users ORDER BY email asc`    User테이블로 부터 email attr기준으로 **오름차순** 정렬하여 모두(*) 가져온다.

![](week2/screenshot%202019-04-21%20PM%204.05.43%202.png)

	* 예제코드
```
import sqlite3

conn = sqlite3.connect('emaildb.sqlite')
cur = conn.cursor()

cur.execute('DROP TABLE IF EXISTS Counts')

cur.execute('CREATE TABLE Counts (email TEXT, count INTEGER)')

fname = input('FILE NAME: ')
if len(fname) < 1: fname = 'mbox-short.txt'
fh = open(fname)
for line in fh:
    if not line.startswith('From: '): continue
    pieces = line.split()
    email = pieces[1]
    #cur.execute('SELECT count FROM Counts WHERE email = {}'.format(email)) # [a-zA-z1-9]@[...] 은 SQL문에 삽입시에 위험한 표현.
    cur.execute('SELECT count FROM Counts WHERE email = ?',(email,))
    row = cur.fetchone()
    if row is None:
        #cur.execute('INSERT INTO Counts (email, count) VALUES ({}, 1)'.format(email))
        cur.execute('INSERT INTO Counts (email, count) VALUES (?, 1)',(email,))

    else:
        #cur.execute('UPDATE Counts SET count = count + 1 WHERE email = {}'.format(email))
        cur.execute('UPDATE Counts SET count = count + 1 WHERE email = ?',(email,))
    conn.commit()

conn.close()

```

