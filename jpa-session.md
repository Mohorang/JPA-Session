# JPA


## 관계형 데이터베이스?

- 데이터베이스?
	1. 사용자의 질의에 대하여 즉각적인 처리와 응답이 이루어집니다.
	2. 생성, 수정, 삭제를 통하여 항상 최신의 데이터를 유지합니다.
	3. 사용자들이 원하는 데이터를 동시에 공유할 수 있습니다.
	4. 사용자가 원하는 데이터를 주소가 아닌 내용에 따라 참조 할 수 있습니다.
	5. 응용프로그램과 데이터베이스는 독립되어 있으므로, 데이터의 논리적 구조와 응용프로그램은 별개로 동작됩니다.
- 데이터베이스에 대한 오해 - 
	- 조금이라도 더 많은 데이터를 저장하기 위해서? (x) 
	- 데이터를 더 효율적으로 조회 수정 삭제 하기 위해서 (o)
	- 도서관을 생각해보자
- 데이터베이스 프로그램?
	- 별도의 미들웨어에서 관리 (dbms)
	- 지금은 모두 이해하기는 어렵겠지만, 결국 컴퓨팅 리소스 + 메모리라는 것 만 이해하자 
- 관계형 데이터베이스가 데이터를 저장하는 특징
	1. 행과 열, key와 value등 우리가 엑셀에서 해오던 것처럼 표 형식으로 저장합니다.
	2. "데이터의 무결성을 보장하기 위해서" 다양한 제약 조건이 있습니다. (https://cocoon1787.tistory.com/m/778)
	3. "데이터를 효율적으로 저장하기 위해서, 데이터의 무결성을 위해서" 테이블을 분리합니다. (https://brunch.co.kr/@dan-kim/26)
	4. 그 테이블들간의 "관계"를 통해 효율적으로 데이터를 탐색합니다.
	5. 그 외에도 다양한 공부해야하는 주제들이 있습니다만 데이터베이스를 직관적으로 이해하기 위해 필요한 내용만 추렸습니다.
- sql은 뭔가요?
	- SQL이란 데이터베이스의 "언어"입니다. 관계형 데이터베이스에서 "데이터를 조작하"는 "표준" 언어 입니다
	- 언어?
	- 데이터를 조작?
		- DDL - Data Definition Language (데이터 정의 언어)
		* DML - Data Manipulation Language (데이터 조작 언어)
		* DCL - Data Control Language (데이터 제어 언어)
	- 표준? 방언?
		- Oracle
		- MySql
- 정리
	- 관계형 데이터 베이스를 쓰는 이유?
	- 관계형 데이터 베이스의 특징?
	- 관계형 데이터 베이스를 조작하는 방법?
	- 데이터 베이스, 관계형 데이터 베이스를 공부하는 것 만 몇개월 이상 소요 될 수 있을 만큼 방대한 지식이 있기 때문에 계속 공부해야 할 주제 (데이터베이스 이론, 성능최적화, 동시처리, 이중화 등)

## Java 어플리케이션에서 데이터를 다루는 방법

```
	class Car {
	
		private int carNumber;
		private String carType;
		...
	}

	class Human {
	
	}
	class User extends Human {
	
		private char userType;
		private String userName;
		private Car userCar;
		...
	}

	class Member extends Member {
	
	}

	...
	public User createUser( ... ) {
		Car newCar = new Car( ... );
		User newUser = new User( ... );
		newUser.setCar(newCar);
		return newUser;
	}


```

- "객체 지향 프로그래밍" 언어 java
- 객체 지향 프로그래밍에서의 데이터
	1. 객체 지향 프로그래밍에서의 데이터에 대한 이해가 필요함 
		- struct와 객체의 차이점 (클린 코드 6장 요약) - https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=zzang9ha&logNo=222068362876
		- 객체지향 프로그래밍의 사실과 오해 요약 - https://velog.io/@nzlk112/%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%EC%9D%98-%EC%82%AC%EC%8B%A4%EA%B3%BC-%EC%98%A4%ED%95%B4 
	2. 엄밀히 말하면 객체와 struct는 다르지만, struct처럼 필드 부분을 주로 설명 할 계획
	3. 딱 데이터를 다루는 부분만을 분리하면, 원시타입과 참조타입, 필드 등으로 설명이 가능
	4. 객체는 참조를 통해 전달하는 방식으로 진행됨, 즉 어디서든 참조값만 얻을 수 있다면 그 데이터를 바로 접근하는게 가능
	5. 괜히 객체지향형 프로그래밍 이야기를 해서 그렇지 이렇게 다루는 데이터들을 딱히 어렵지 않게 표에 저장하고 관리 할 수 있을 것 같아 보인다 - 과연??



|     | userType | userName | userCar |
| --- | -------- | -------- | ------- |
| 1   | A        | Minsoo   | 2       |
| 2   | N        | Minjung  | 3       |

...


## 어플리케이션이 데이터베이스를 직접 다룰 때의 문제점

### 01. 그냥 귀찮음

이런 데이터를 다루고 싶다면?
```
class User {
	private int id;
	private String name;
	private int age;
	private long salary;
	...
}
```


테이블 만들기 (직접 db로 접속해서 쿼리 날림)
```
create table user ( 
	ID int not null, 
	name varchar(20) not null, 
	age int not null, address char(25), 
	salary int, 
	primary key(ID)
	);
```
id / name / age / salary / primary 

**유저 조회 로직이라면?**

쿼리 직접 만들기 (어플리케이션에서)
```
String userSelectQuery = "select name, age, salary from user u where u.name = "

String userName = request.getHeader("name");

String needToExecuteQuery = userSelectQuery + userName;

```


쿼리를 jdbc api 통해 직접 실행
```
ResultSet result = jdbcAPI.executeQuery(needToExecuteQuery);
```


쿼리 결과로 유저객체도 직접 만들어야함
```
User user = new User();

user.setId(result.ID);
user.setName(result.name);
user.setAge(result.age);
user.setSalary(result.salary);

```


### 02. SQL 의존적이라 변경에 취약함

유저의 몸무게 데이터가 새로 받아와야한다면?
```
class User {
	private int id;
	private String name;
	private int age;
	private long salary;

	// 추가됨!
	private int weight;
	...
}
```



쿼리 직접 또 수정해야함 (어플리케이션에서)
```
// String userSelectQuery = "select name, age, salary from user u where u.name = "

String userSelectQuery = "select name, age, salary, weight from user u where u.name = "

String userName = request.getHeader("name");

String needToExecuteQuery = userSelectQuery + userName;

```



유저객체에 값세팅 하는 부분도 당연히 추가해줘야함.
```
User user = new User();

user.setId(result.ID);
user.setName(result.name);
user.setAge(result.age);
user.setSalary(result.salary);
// 추가됨!
user.setWeight(result.weight);

```

*단순히 유저 몸무게 하나 더 받아오는데 해야하는 일들이 주렁주렁 달려있으며, 로직이 복잡해지거나 서비스가 커질 수록 변경점은 훨씬 많아지게됨*



### 03. 객체지향 모델과 관계형 데이터베이스의 패러다임 불일치

**선 요약**
출처 : https://knoc-story.tistory.com/m/90
|                        | 객체                                                         | 릴레이션                                                                                    |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| 밀도 문제              | 다양한 크기의 객체를 만들 수 있음, 커스텀한 타입 만들기 쉬움 | 테이블, 기본 데이터 타입                                                                    |
| 서브타입 문제          | 상속, 다형성 구현 쉬움                                       | 상속 없음, 다형적인 관계 표현 불가                                                          |
| 식별성 문제            | 레퍼런스 동일성, 인스턴스 동일성                             | 오직 pk                                                                                     |
| 관계 문제              | 서로간의 객체 참조를 통해 표현, 다대다 가능, 방향이 있다     | 다대다 x, 다대다를 맺어주는 테이블로 처리, 외래키가 있어서 바로 조회 가능(방향 없음) |
| 데이터 네비게이션 문제 | 마음대로 레퍼런스타고 이동 가능                              | 그러한 방식이 비효율적(매번 join, 그리고 다 가저오면 성능문제)                              |

1. 밀도 문제는 딱히 설명이 필요 없음, db의 데이터가 더 정형화되어있고 까다로움

2. 관계형 데이터 베이스에는 상속의 개념이 없음
	- 상속은 객체의 역할과 구현을 분리해주기 위해 객체 지향 프로그래밍에서 가장 핵심적인 기능 중 하나
	- 특정 역할만을 의존하게 되면, 특정 역할의 구현체나, 상속받은 것들의 실제 구현이 바뀌어도 프로그램이 문제없이 동작하기에 변경점이 적음
	- 자세한 내용은 나중에 찾아보면 좋을 것 같다.
	- 어쨌든 이러한 것들을 jpa orm은 다양한 방식으로 풀어 해결해줌


3. 식별성 문제는 각각의 패러다임이 식별을 어떻게 하는지 알아보면 됨, 만약 엔티티 클래스가 아니었다면, 레퍼런스로 비교가 가능한데 자바 객체에서 id라는 필드가 꼭 필요했을까? 어떠한 의미에서는 패러다임 불일치 때문에 자바가 양보했다고 볼 수 있음. 

4. 관계 문제
```
class Member {
	private int memberId;
	private String mamberName;
	private Team team;

}

class Team {
	private int teamId;
	private String teamName;
}


Member member = new Member();
Team team = new Team();

member.setTeam(team);

member.team(); //가능
team.member(); // 불가능
```


| member_id | member_name | team_id |
| --------- | ----------- | ------- |
| 1         | minsoo      | 2       |
| 2         | minjung     | 1       |
| 3         | minsuk      | 1       |
| 4         | minsook     | 3       |
| 5         | daehyun     | 2       |
| 6         | songyi      | 1       |


| team_id | team_name  |
| ------- | ---------- |
| 1       | naver_team |
| 2       | kakao_team |
| 3       | line_team           |

```
"멤버 테이블에있는 민수의 팀아이디와 일치하는 팀의 팀이름을 팀 테이블에서 조회해줘!"
>> kakao_team

"네이버 팀의 팀아이디인 1을 외래키로 가지고 있는 애들을 멤버 테이블에서 조회해줘!"
>> minjung, minsuk, songyi

// 양방향이 가능하다는 소리입니다.
```

++ 다대다가 불가능 한 이유, 풀어내는법 : https://hanamon.kr/%EA%B4%80%EA%B3%84%ED%98%95-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%84%A4%EA%B3%84-%EA%B4%80%EA%B3%84-%EC%A2%85%EB%A5%98/


5. 참조로 전달하는 만큼, 탐색은 치트키 수준으로 가능함 (메모리 주소를 알고 있어서, 물리적으로는 매우매우매우매우 짧은 시간, 논리적으로는 소요되는 시간 없이 참조가 가능), 그러나 데이터베이스에는 조인이라는 과정을 거쳐야 하기에 매우 비용이 크게 소모될 수 있습니다. - https://brunch.co.kr/@dan-kim/27

member.team().getTitle();

**조인이란?** 
ppap?
조인 개념 : https://brunch.co.kr/@dan-kim/27
이너조인 아우터 조인 : https://stanleykou.tistory.com/m/entry/SQL-INNER-%EC%A1%B0%EC%9D%B8%EA%B3%BC-OUTER%EC%A1%B0%EC%9D%B8%EC%9D%B4-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94


## JPA(orm)이 해주는 일들

너무 많아서 간단하게만 요약하고 넘어가겠습니다!
1. 쿼리를 자동으로 만들어 줍니다. 특히 springDataJpa 사용하시는 분들, 테이블 생성, 데이터 조회, 수정, 삭제 쿼리 작성해보신적 없으시죠?
2. 그리고 그러한 과정에서 어플리케이션 레이어에서 sql 의존성을 줄여서 번거로운 작업이 매우매우 단축됩니다.
3. 패러다임의 불일치를 해결해줍니다. - 이건 아무리 머리를 굴려도 요약을 하기가 어렵네요, 다양한 방법을 통해 jpa에 녹아 있으니 나중에 한 번 씩 떠울리면서 아 이래서 패러다임이 불일치하고, 그걸 해결해준다는구나 하고 생각하시면 좋을 것 같습니다.
4. 그러면서 성능도 챙겼습니다. 다양한 최적화가 이루어져 있기 때문입니다.
5. 방언도 지원해줍니다. h2를 붙이던, mySql, oracle 뭘 붙여도 코드의 변경은 없어요, 관계형 db이자 표준을 준수한 sql을 지원한다면, jpa가 방언들도 알아서 처리해줍니다.

등등등 너무 많지만 이것 역시 천천히 공부해보시면 좋을 것 같습니다.


**jpa가 안해주는 일들**
crud위주의 실시간 처리 목표가 아닌 쿼리들(bulk, update, 통계처럼 복잡한 쿼리(join 지옥...))
이럴때는 쿼리를 직접 내셔야 합니다!



## 영속성 컨텍스트 ?
**(지금은 jpa가 이걸 어떻게, 성능까지 챙기면서 해줬는지의 관점에서만 가볍게 이해하면 좋을 것 같습니다)**

트랜잭션 : https://devuna.tistory.com/m/30
영속성 컨텍스트? : https://willseungh0.tistory.com/m/65



## Jpa 실제 어노테이션들


데이터베이스 관계 리마인드 : https://hanamon.kr/%EA%B4%80%EA%B3%84%ED%98%95-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%84%A4%EA%B3%84-%EA%B4%80%EA%B3%84-%EC%A2%85%EB%A5%98/


### 기본 엔티티 매핑 관련
https://incheol-jung.gitbook.io/docs/study/jpa/4

### 연관관계 매핑 관련
https://incheol-jung.gitbook.io/docs/study/jpa/5

1대다 다대다 1대1 다 케이스가 다릅니다. 제가 설명을 특정 케이스만 상정하고 드리거나 다른 예제와 헷갈린적이 있다보니 이부분에서 오해가 있을 수 있었을 것 같네요

**mappedBy? 연관관계 주인?**
```
member.setTeam(team) ??

team.getMembers.add(member) ??

이 두가지의 차이, 연관관계의 주인은 외래키가 있는곳, 연관관계의 주인이 아닌곳에 mappedBy
oneToOne이라면, 다시 한 번 꼭 필요한지 생각해본 후, 자주 조회가 일어나는 쪽 반대
```

### 프록시 관련
https://incheol-jung.gitbook.io/docs/study/jpa/8

**fetchType lazy, eager??**
-  즉시로딩과 지연로딩 차이
	- eager의 경우 모두 조인해서 가져옴
	- lazy의 경우 해당 엔티티를 조회하는경우 다시 개별 쿼리가 나감
```

****case eager :

Member member = repository.findMember();

---------------------------------------------
SELECT
	M.MEMBER_ID AS MEMBER_ID,
	M.TEAM_ID AS TEAM_ID,
	M.USERNAME AS USERNAME,
	T.TEAM_ID AS TEAM_ID,
	T.NAME AS NAME
	
FROM MEMBER M
LEFT OUTER JOIN TEAM T
ON M.TEAM一ID=T.TEAM一ID

WHERE
M.MEMBER_ID='member1'
---------------------------------------------
member.getTeam() // 이미 정보를 디비에서 퍼올려와서 따로 쿼리가 나가지 않고, 이미 정보가 있음


****case lazy : 

Member member = repository.findMember(); // 정말 멤버만 찾아옴

---------------------------------------------
SELECT
	M.MEMBER_ID AS MEMBER_ID,
	M.TEAM_ID AS TEAM_ID,
	M.USERNAME AS USERNAME
	
FROM MEMBER M

WHERE
M.MEMBER_ID='member1'
---------------------------------------------

member.getTeam(); // 팀 정보 조회가 필요할 때 다시 쿼리가 나감

---------------------------------------------
SELECT
	TEAM_ID,
	TEAM_NAME
	
FROM TEAM 

WHERE
TEAM_ID = 'teamid'
---------------------------------------------

```

- member, team 둘 다 빈번하게 조회가 일어나는 상황이라면 디비에 요청을 한 번 덜 보낼 수 있지만, 테이블관계등이 복잡해지고 서비스가 커져서 테이블끼리 연관관계가 많거나 하는 상황에는 n+1 문제 등을 일으킬 수 있어 정말 명확한 상황 아니고서는 지연로딩을 사용한다고 보면 됩니다.

**cascade**
영속성 전이?
영속성 컨텍스트나, 테이블 개념이 아직 익숙하지 않으시다면, 일단은 연관관계 설정된 다른 데이터의 변경이 영향을 미치는지, 안미치는지를 설정한다고 생각하시면 될 것 같습니다.
https://velog.io/@max9106/JPA%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%83%81%ED%83%9C-Cascade



## 정리 & 마무리

### 어떻게 공부를 하는게 좋을까?
먼저 sql과 db관련 공부를 꾸준히 가져가시고,
항상 jpa환경에서 개발하는걸 추천드립니다.
jpa, jdbc, hibrenate 전부 공부 할게 많고, 
알아야 하는 내용도 많지만 기본 밑바탕은 db 지식이 베이스 입니다.
db에 관련된 이슈들을 어떻게 해결해주는지, 어떻게 설정하고 커스텀하는지 등 과 같이
데이터베이스 관련된 지식들로 귀결될 가능성이 많기 때문에 
데이터베이스를 꾸준히 공부하시면서 필요한 지식을 습득하는게 좋은 것 같습니다.

### 일단 과제를 제출해야 하는데 어떻게 처음 테이블 설계를 해야할까?
사실 jpa도 익숙하고, rdb에도 익숙하다면 db테이블이 먼저 나오는게 일반적인 것 같습니다.
만약 그렇지 않으시고, 막연하기만 하다면, 일단 어플리케이션 layer에서 어떤 데이터를 다루고 반환 할 지 생각해보시고, 그에 맞는 객체를 구현하고 필드를 설정해보시고, 그걸 jpa엔티티로 만들어가시면서 진행하시되
꾸준히 erd를 확인하시면서, jpa로 이렇게 구현하면 erd가 이렇게 나오는구나 하는 식으로 공부하시는게 빠르긴 할 것 같습니다.
필요에 의해 테이블과 엔티티를 설계하고, 필요에 의해 연관관계를 맺고, 실제 디비 테이블이 어떻게 나오는지 확인하는 싸이클을 거치는 것도 빠르게 감을 잡기 좋은 것 같습니다.
