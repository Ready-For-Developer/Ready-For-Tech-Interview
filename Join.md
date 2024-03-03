## <div align="center">Join</div>
<div align="center">마지막으로 공부한 날짜 : 2024.03.03</div>
</br></br>

</br></br>

## NESTED LOOP JOIN

- 줄여서 NL JOIN
- 2개 이상의 테이블에서 하나의 집합을 기준으로 순차적으로 상대방 Row를 결합하여 원하는 결과를 조합하는 조인 방식
- 중첩 FOR문과 같은 원리

```sql
// 조인을 할때 먼저 액세스 되는 테이블을 Driving Table이라고 하며 나중에 액세스 되는 테이블을 Driven Table
for(i=0; i<dept.length; i++) { -- driving table 
    for(j=0; j<emp.length; j++) { -- driven table
       // Search
    } 
}
```

</br></br>

### **NESTED LOOPS JOIN의 장단점**

**1.** 인덱스에 의한 랜덤 액세스에 기반하고 있기 때문에 대량의 데이터 처리 시 적합하지 않습니다.

**2.** Driving Table로는 데이터가 적거나 where절 조건으로 row의 숫자를 줄일 수 있는 테이블이어야 합니다.

**3.** Driven Table에는 조인을 위한 적절한 인덱스가 생성되어 있어야 합니다.

**4.** 선행 테이블의 결과를 통해 후행 테이블을 액세스 할 때 랜덤 I/O가 발생합니다.

</br></br>

## **SORT MERGE JOIN**

- 조회의 범위가 많을 때 주로 사용하는 조인 방법론
- 중첩 FOR문과 유사한 방식
- 위와 다른 방식으로 JOIN 컬럼 위주로 정렬을 시킨 후 JOIN
- 주로 조인 조건 칼럼에 인덱스가 없거나, 출력해야 할 결과 값이 많을 때 사용

```sql
select /**USER_MERGE(A B) */ A.Color, B.SIZE,...
from TABLE_A A,TABLE_B B
where a.joinkey_a = b.joinkey_b -- join key에 대한 인덱스가 테이블 둘 모두 다 없음
and a.color = 'RED' --인덱스 있음
and b.size = 'MED'; --인덱스 없음
```

</br></br>

## **HASH JOIN**

- 배치에서 좋은 수행원리
- 조인될 두 테이블 중 하나를 해시 테이블로 선정하여 조인될 테이블의 조인 키 값을 해시 알고리즘으로 비교하여 매치되는 결과값을 얻는 방식
- 주로 많은 양의 데이터를 조인해야 하는 경우에 주로 사용

</br></br>

### **HASH JOIN의 동작 방식**

![HASH JOIN의 동작 방식](https://github.com/fsm12/Ready-For-Tech-Interview/assets/74345771/11573b8a-a5f5-4b82-b039-0c9f791dd266)


**1.** 둘 중 작은 집합(Build Input)을 읽어 Hash Area에 해시 테이블을 생성한다. (해시 함수에서 리턴 받은 버킷 주소로 찾아가 해시 체인에 엔트리를 연결)

**2.** 반대쪽 큰 집합(Probe Input)을 읽어 해시 테이블을 탐색하면서 JOIN 한다.

**3.** 해시 함수에서 리턴 받은 버킷 주소로 찾아가 해시 체인을 스캔하면서 데이터를 찾는다.

</br></br>

# 레퍼런스

[조인 수행 원리](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?mod=document&uid=356)

</br></br>
