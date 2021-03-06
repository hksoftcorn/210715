# 프로젝트를 위한 RDBMS 설계방법

> ✔ 설계 프로세스 = 요구사항 분석 → 개념적 설계 → 논리적 설계 → 물리적 설계
>
> ✔ 정규화 vs 반정규화



## 1. 설계 프로세스

**DB설계의 필요성**

- 프로젝트, 명세서 등의 정보 요구사항에 대한 정확한 이해
- 분석자, 개발자, 사용자간의 원활한 의사소통 수단
- 데이터 중심의 분석 방법
- 현행 시스템만이 아닌 신규 시스템 개발의 기초 제공

### 1.1. 설계를 위한 요구사항 분석

> 데이터베이스에 대한 사용자의 요구사항을 수집하고 분석해서 아래와 같이 요구사항(기능) 명세서를 작성

```
요구사항 명세 샘플 - 항공사 DB
회원으로 가입하려면 ID, 비밀번호, 성명, 신용카드 정보를 입력해야 한다.
회원의 신용카드 정보는 여러 개를 저장할 수 있다.
신용카드번호, 유효기간을 저장할 수 있다.
회사가 보유한 비행기에 대해 비행기 번호, 출발 날짜, 출발 시간 정보를 저장하고 있다.
비행기 좌석에 대한 좌석 번호, 등급 정보를 저장하고 있다.
회원은 좌석을 예약하는데, 회원 1명은 좌석을 하나만 예약할 수 있고, 한 좌석은 회원 한 명만 예약할 수 있다.
```

### 1.2. 개념적 설계

> 작성한 요구사항 명세서에서 데이터베이스를 구성하는데 필요한 개체, 속성, 개체 간의 관계를 추출하여 ERD 생성

1. 개체(entity)와 속성(attribute) 추출 - 대부분 명사로 선별
   - 요구사항에서 개체는 대부분 명사로 이루어져있으나, 속성과 구별하여 추출
   - 위 명세에서 회원, 신용카드, 비행기, 좌석은 개체
   - 아이디, 비밀번호, 성명, 카드번호, 유효기간, 비행기 번호, 출발 날짜, 출발 시간, 좌석번호, 등급 등은 속성
   - 필요하다고 생각되는 것들.
2. 개체 간의 관계(relationship)을 추출 - 대부분 동사로 선별, 개체 간의 관계를 나타내는 동사
   - 여러 가지로 분류해서 정의.관계에 속한 속성, 1:1 / 1:N / N:M, 필수적/선택적 참여...
   - 회원 - 신용카드 정보: 여러 개를 저장 (1:N)회원 한 명은 좌석을 하나만 예약 가능, 한 좌석은 회원 한 명만 예약 가능 (1:1)
3. 개념 설계 기반으로 ERD 생성
   - □ 개체, ○ 속성, ◇ 관계
   - 1:1, 1:N, N:M도 반영

### 1.3. 논리적 설계

> 모든 개체는 릴레이션(Table)으로 변환 (개체 : 테이블, 속성 : 테이블의 속성)

#### 논리적 설계

1. 개체 -> 릴레이션(Table)
   - 개체 -> 테이블 / 속성 -> 테이블의 속성
2. N:M 관계 -> 릴레이션(Table)
   - 관계 -> 릴레이션명 / 관계속성 -> 릴레이션 속성
   - N:M 관계에서는 바로 연결할 수 없으므로 테이블을 추가 생성
3. 1:N, 1:1 관계 -> 외래키(Foreign Key)
   - 일반적으로 1:N 관계에서 하나만 있는 쪽의 기본키를 N쪽 릴레이션에 포함, FK로 지정
   - 일반적 1:1 관계는 FK를 서로 주고 받음. 양쪽 모두에 FK 추가.
4. 다중 값 속성 -> 독립 릴레이션(Table)
   - 릴레이션에서는 다중 값 속성을 가질 수 없음 -> 다중 값 속성은 별도의 릴레이션으로 생성
     - 참고: 제 1 정규화는 다중값을 갖는 속성이 있을 때, 한 속성이 원자값(하나의 값)을 갖도록 변경
   - ex) 사원 릴레이션 -> 자녀 릴레이션을 별도로 생성, 사원번호(FK), 자녀명 으로 구성



### 1.4. 물리적 스키마 및 구현

> ERD를 실제 테이블로 생성한다.

- ERD를 실제 테이블로 생성: Workbench 같은 DB Tool, SQL 스크립트 모두 가능해야 함.
  - 실무에서 CLI에서 DB 생성하는 경우 있음.



## 2. 반정규화

> 정규화된 엔티티타입, 속성, 관계를 시스템의 성능향상, 개발과 운영의 단순화를 위해 모델을 통합하는 프로세스

- 정규화 모델
  - SQL작성이 용이하지 않고 과다한 테이블 조인이 발생하여 성능이 저하될 가능성이 높습니다.
- 반정규화 모델
  - 같은 데이터가 여러 테이블에 걸쳐 존재하므로 무결성이 깨질 우려가 있습니다.
  - 하지만, 여러 개의 테이블이 단순해지므로 SQL작성이 용이하고 성능이 나아지는 경우가 있다.

### 2.1. 반정규화 방법

- 1:1 / 1:N 관계 테이블 병합
- 수퍼/서브 타입 테이블 병합
- 수직 분할(집중화된 일부 컬럼을 분리)
- 수평 분할(행으로 구분하여 구간별 분리)
- 테이블 추가

### 2.2. 컬럼 반정규화

- 중복컬럼 추가
- 파생 컬럼 추가
- PK에 의한 컬럼 추가
- 응용시스템 오작동을 위한 컬럼 추가

### 2.3. 관계 반정규화

- 중복 관계 추가(이미 A테이블에서 있지만, C테이블에 추가함으로써 Read에서 성능상 이득을 볼 수 있음)



## 3. 추가 Tip

1. 반정규화 => 무결성이 깨짐 => 데이터 구조의 오류를 허용
2. 반정규화 하면 서버 성능 이득이 있는 반면, 미래에 데이터 오류 발생 가능성 큼,
3. 반정규화는 고수가 아니면 자제, 차라리 하드웨어 증설할 것

=============================================== 

- DB 에서 시간 column 은 varchar 사용하지 말고 unix_time 같은 시간 type 사용 할 것, 그렇치 않으면 최근 일주일 검색 같은 query 문 만들기 어려워짐 

- varchar - 2020051221 (X) unix_time - 1594034034 (O)

- 시간 관련 data type - https://m.blog.naver.com/nieah914/221810697040

- 테이블 설계에서 key는 varchar 같은 type 사용 (x), int  같은 정수 type 만 사용 모든 테이블에 id, num 같은 column 을 key 로 사용하기를 추천, 그러면 일관된 규칙으로 테이블 사용하기 쉬워짐 - https://stackoverflow.com/questions/2103322/varchar-as-foreign-key-primary-key-in-database-good-or-bad

===================================================



