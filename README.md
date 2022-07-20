# JSP 블로그 프로젝트
- 메타코딩 / JSP오프라인수업1강 환경설정 편

## 환경
- Windows 10 x64
- jdk 1.8
- tomcat 9.0
- sts tool
- mysql 8.0
- lombok
- gson (json 파싱)
- encoding utf-8

## MySQL 데이터베이스 생성 및 사용자 생성
- 권한 부여, *.*(모든권한) 할당할 id %(접근, 모든 곳에서 접근가능)
```sql
create user 'bloguser'@'%' identified by 'bitc5600';
GRANT ALL PRIVILEGES ON *.* TO 'bloguser'@'%';
create database blog;
```

## MySQL 테이블 생성
- 워크벤치 재실행 후 MySQL Connections 에서 + 버튼 누름
- Connection name, username : bloguser
- password에 store in vault... 눌러 비밀번호 입력 bitc5600
- Test Connection 확인 후 생성된 사용자로 접속
- use blog;

```sql
CREATE TABLE user(
	id int primary key auto_increment,
	username varchar(100) not null unique,
	password varchar(100) not null,
	email varchar(100) not null,
	address varchar(100),
	userRole varchar(20),
	createDate timestamp
) engine=InnoDB default charset=utf8;

CREATE TABLE board(
	id int primary key auto_increment,
	userId int,
	title varchar(100) not null,
	content longtext,
	readCount int default 0,
	createDate timestamp,	
	foreign key (userId) references user (id)
) engine=InnoDB default charset=utf8;

CREATE TABLE reply(
	id int primary key auto_increment,
	userId int,
	boardId int,
	content varchar(300) not null,
	createDate timestamp,	
	foreign key (userId) references user (id) on delete set null,
	foreign key (boardId) references board (id) on delete cascade
) engine=InnoDB default charset=utf8;
```