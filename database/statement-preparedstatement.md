### Statement

- SQL 문을 매번 컴파일해서 실행하는 것
- 쿼리의 내용이 조금씩 변형될 때 유리

- Static SQL 사용 (Embedded SQL)
    - Static SQL - String형 변수에 담지 않고 코드 사이에 직접 기술한 SQL문

```c
int main()
{
   printf("사번을 입력하십시오 : ");
   scanf("%d", &empno);
   EXEC SQL WHENAVER NOT FOUND GOTO notfound;
   EXEC SQL SELECT ENAME INTO :ename
            FROM   EMP
            WHERE  EMPNO = :empno;
   printf("사원명 : %s.\n", ename);

notfound:
   printf("%d는 존재하지 않는 사번입니다. \n", empno);
}
```

### PreparedStatement

- 최초 SQL 컴파일 후에 SQL문을 저장하는 것
    - 동일한 쿼리 반복 수행 환경에서 DB에 훨씬 적은 부하를 주어 성능이 더 좋다
- 쿼리의 내용이 거의 안 바뀌고 인자 값만 달라질 때 유리

- Dynamic SQL 사용
    - Dynamic SQL - String형 변수에 담아서 기술하는 SQL문

```c
int main()
{
   char select_stmt[50] = "SELECT ENAME FROM EMP WHERE EMPNO = :empno";
   // scanf("%c", &select_stmt); --> SQL문을 동적으로 입력 받을 수도 있음

   EXEC SQL PREPARE sql_stmt FROM :select_stmt;
   EXEC SQL DECLARE emp_cursor CURSOR FOR sql_stmt;
   EXEC SQL OPEN emp_cursor USING :empno;
   EXEC SQL PETCH emp_cursor INTO :ename;
   EXEC SQL CLOSE emp_cursor;
   printf("사원명 : %s.\n", ename);
}
```

### 결론

- PreparedStatement SQL문의 최초 파싱 과정이 더 무겁긴 하다.
- 하지만 파싱은 테이블 레코드 읽기 과정의 1/10 수준이다.
- 그러므로 SQL Injection 문제 등을 보완해주는 **PreparedStatement** 사용이 좋다.
    - SQL Injection - 임의의 SQL 문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 공격 행위
