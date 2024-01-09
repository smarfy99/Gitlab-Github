# 24.01.08 ~ 12 기술스택 정리
## 01.08 JPA - Querydsl
### 🚀 동적 쿼리 작성방법
- 동적 쿼리를 해결하는 두 가지 방식
#### BoolenanBuilder 사용
```java
@Test
    public void dynamicQuery_BooleanBuilder () throws Exception {
        String usernameParam = "member1";
//        Integer ageParam = 10;
        Integer ageParam = null;

        List<Member> result = searchMember1(usernameParam, ageParam);
        assertThat(result.size()).isEqualTo(1);
    }

    private List<Member> searchMember1(String usernameCond, Integer ageCond) {

        BooleanBuilder builder = new BooleanBuilder();
        // 생성자에 인자값으로 초기값 설정 가능
//        BooleanBuilder builder = new BooleanBuilder(member.username.eq(usernameCond));
        
        if (usernameCond != null) {
            builder.and(member.username.eq(usernameCond));
        }

        if (ageCond != null) {
            builder.and(member.age.eq(ageCond));
        }

        return queryFactory
                .selectFrom(member)
                .where(builder)
                .fetch();
    }

```
- BooleanBuilder를 통해 원하는 query 조건문을 and 또는 or 연산을 추가 후 <span style='background-color: #ffff00; color:black'>**queryfactory의 where에서 생성한 builder**</span>을 사용한다.

#### Where 다중 파라미터 사용
```java
@Test
public void 동적쿼리_WhereParam() throws Exception {
    String usernameParam = "member1";
    Integer ageParam = 10;
    List<Member> result = searchMember2(usernameParam, ageParam);
    Assertions.assertThat(result.size()).isEqualTo(1);
}
private List<Member> searchMember2(String usernameCond, Integer ageCond) {
    return queryFactory
            .selectFrom(member)
            .where(usernameEq(usernameCond), ageEq(ageCond))
            .fetch();
}
private BooleanExpression usernameEq(String usernameCond) {
     return usernameCond != null ? member.username.eq(usernameCond) : null;
}
private BooleanExpression ageEq(Integer ageCond) {
    return ageCond != null ? member.age.eq(ageCond) : null;
}
```
- `where`조건에 null 값은 무시됨
- 메서드를 다른 쿼리에서도 <span style='background-color:#ffff00; color:black;'>**재활용**</span> 할 수 있음
- 쿼리 자체의 가독성이 높아진다.

#### 조건으로 만든 메서드 조합
```java
private BooleanExpression allEq(String usernameCond, Integer ageCond) {
	 return usernameEq(usernameCond).and(ageEq(ageCond));
}
```
- `null`체크는 주의해서 처리


### 🚀 수정, 삭제 벌크 연산
#### 쿼리 한 번으로 대량 데이터 수정
```java
long count = queryFactory
 .update(member)
 .set(member.username, "비회원")
 .where(member.age.lt(28))
 .execute();
```

#### 기존 숫자에 1 더하기
```java
long count = queryFactory
 .update(member)
 .set(member.age, member.age.add(1))
 .execute();

// 곱하기
// update member  set age = age + 1
```

#### 쿼리 한 번으로 대량 데이터 삭제
```java
long count = queryFactory
 .delete(member)
 .where(member.age.gt(18))
 .execute();
```
⚠ 영속성 컨텍스트에 있는 엔티티를 무시하고 실행되기때문에 해당 쿼리를 실행 후 ***영속성 컨텍스트를 초기화*** 하는 것이 안전
```java
EntityManager em = new EntityManager();
...
em.flush();
em.clear();
```

### :rocket: SQL Function 호출
- SQL function은 JPA와 같이 **Dialect에 등록된 내용만 호출**할 수 있다.
- member → M으로 변경하는 replace함수 예시
    
    ```java
    String result = queryFactory
        .select(Expressions.stringTemplate("function('replace', {0}, {1}, {2})", 
            member.username, "member", "M"))
        .from(member)
        .fetchFirst();
    ```
    
- 소문자 변경 예시
    
    ```java
    .select(member.username)
    .from(member)
    .where(member.username.eq(Expressions.stringTemplate("function('lower', {0})", 
    member.username)))
    ```
    
    - lower 같은 ansi 표준 함수들은 querydsl이 상당부분 내장하고 있다. 따라서 다음과 같이 처리해도 결과는 같다.
    
    ```java
    .where(member.username.eq(member.username.lower()))
    ```