**NoSQL은 기존 관계형 데이터베이스의 한계를 극복하기 위한 데이터 저장소의 새로운 형태이다.**


## NoSQL?

- **Not Only SQL**
- No SQL
- Non-Relational Operational Database SQL

> 1998년 카를로 스트로찌(Carlo Strozzi)라는 엔지니어가 공개한 표준 SQL 인터페이스를 채용하지 않은 자신의 경량 Open Source 관계형 데이터베이스를 NoSQL이라고 명명한데서 유래


### NoSQL 정의

NoSQL은 **기존 관계형 데이터베이스**(RDBMS)가 아닌 다른 형태의 데이터 저장 기술을 의미.

> **Not Only SQL** : RDBMS가 갖고 있는 특성뿐만 아니라, 다른 특성들을 부가적으로 지원한다는 것을 의미
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5bdab18f-0d26-42e8-84eb-f2deb693a40f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051410Z&X-Amz-Expires=86400&X-Amz-Signature=5b4ee90d3f9ea5c5a2cd84d509712d481e65dfe7968195de3e54b0622a93f607&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## NoSQL 종류

### Document DB

- Value가 계층적인 형태의 도큐먼트로 저장 (객체지향에서의 객체와 유사)
- Value는 하나의 단위로 취급되어 저장. 즉, 하나의 객체를 여러개의 테이블에 나눠 저장할 필요가 없음.
- JSON 형태
- MongoDB, CouthDB 등이 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b4eb953d-d8d4-4786-8417-f6d2d14e057b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051427Z&X-Amz-Expires=86400&X-Amz-Signature=25d88b6242f25f734d6f33bce9b0df1b68c0d7873c101039deeeaf45463fa5f5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### Wide Column DB

- 컬럼 패밀리 모델
    - 키 공간 : RDBMS의 스키마와 비슷. 모든 열 패밀리를 포함
        
        ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cd374ae7-7069-4985-b9a3-ad260b3aa3af/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051440Z&X-Amz-Expires=86400&X-Amz-Signature=d6705de5a9dff3a99bfba535733a046f8b0624ea9bb10499e259951483c2d4d3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
        
    
    - 컬럼 패밀리 예시
        - 컬럼 패밀리는 여러 행으로 구성
        - 각 행은 각자 다른 수의 행과 열을 포함. 그리고 열은 다른 행의 열과 일치할 필요가 없음.
        
        ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ea34f914-b52a-4439-befd-dc108304cd10/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051452Z&X-Amz-Expires=86400&X-Amz-Signature=4d95e06e7d978b15ceb852ab79c61d6cf0e94dc5b917b0f3a0fff74a0a0041fb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
        
        ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ba4678a1-b5dc-4bc4-ba0a-e5a7d4504c5e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051500Z&X-Amz-Expires=86400&X-Amz-Signature=1e757679f293d09e87e7ac8f9e9206f23ee59763a50f95c1444fdaa2fd59ea1c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
        
- HBase, Hypertable 등이 있다.

### Key-Value Database

- 데이터가 Key와 Value의 쌍으로 저장되며, 값은 어떠한 형태의 데이터라도 담을 수 있다. 심지어는 이미지나 비디오도 가능하다.
- Redis, Riak, Amazon Dynamo DB 등이 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a039637a-7741-40b6-ae75-20dadd8e42f5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051512Z&X-Amz-Expires=86400&X-Amz-Signature=ef3474868c3d70875dd12c6a09634cd92b1de6e46ad43549281130650ad81c56&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### Graph Database

- 데이터를 Node와 Edge, Property와 함께 그래프 구조를 사용하여 데이터를 표현
- 개체와 관계를 그래프 형태로 표현한 것이므로 관계형 모델이라고 할 수 있으며, 데이터 간의 관계가 탐색의 키일 경우에 적합
- 페이스북이나 트위터 같은 소셜 네트워크에서(내 친구의 친구를 찾는 질의 등) 적합하고, 연관된 데이터를 추천해주는 추천 엔진이나 패턴 인식 등의 데이터베이스로도 적합
- Neo4J 등이 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c35448db-eec1-454d-9f8b-f7d8a36f86ed/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051521Z&X-Amz-Expires=86400&X-Amz-Signature=08cc0144d50e6a8bd373c8d5050e19a62e371f71d8b2541800704e052de50d69&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## RDBMS vs NoSQL

| 비교 | RDBMS | NoSQL |
|:--:|:--:|:--:|
|**확장성**|수직적 확장|수평적 확장|
|**일관성**|스키마 O|스키마 X|


### 수평적 확장성

- **확장성** (Scalability) : 처리할 작업량이 늘어날때마다, 늘어나는 요구에 맞춰 크기를 키우거나 기능을 확장시킬 수 있는가?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/16626f73-5aa2-4d09-95d0-6b0b10f22e91/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051716Z&X-Amz-Expires=86400&X-Amz-Signature=6e0384b60be01b42c8ae9cbca415b828ce5eae6533dd7e6f31834152a14ad6c5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- **수평적 확장** (Horizontal Scalability)은 더 많은 서버를 추가해서 데이터베이스를 전체적으로 분산시키는 것을 의미한다. 하나의 데이터베이스는 작동하지만 여러 호스트에서 작동한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ad2287ba-3725-4cfc-81bf-871ac3ac0b84/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051728Z&X-Amz-Expires=86400&X-Amz-Signature=13af0970994e246b630873fe5b4b0931bacd2b8b8bdadd6bef6df39aa3d8bed1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- **수직적 확장** (Vertical Scalability)는 CPU나 RAM 같은 부품이나 하드웨어를 추가해주거나 교체를 해 전체적인 성능을 향상시키는 것을 의미한다. 그래서 소프트웨어의 설계나 구조에 변화를 주거나 시간을 따로 쏟을 필요가 없다. 단순하게 데이터베이스 서버의 성능을 향상시킨다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5f7eae0e-f276-4e35-998c-4a316d22882d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051738Z&X-Amz-Expires=86400&X-Amz-Signature=3b2dba199cb5107b1f7a98a03cca6a2053817467c21284b91af913def084fa62&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### 스키마

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/de9d1600-b4d7-40fa-80a4-b5d6ea17a806/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211017T051748Z&X-Amz-Expires=86400&X-Amz-Signature=497635fbcf97438f3c1e34cd2c18c09ec59bcd9689faa23b9ee97814e2bbea8e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### 언제 사용해야 할까?

- **RDBMS**는 데이터 구조가 명확하며 변경 될 여지가 없으며 명확한 스키마가 중요한 경우 사용. 또한 중복된 데이터가 없어(데이터 무결성) 변경이 용이하기 때문에 관계를 맺고 있는 데이터가 자주 변경이 이루어지는 시스템에 적합
- **NoSQL**은 정확한 데이터 구조를 알 수 없고 데이터 구조가 변경/확장이 될 수 있는 경우에 사용. 또한 데이터 중복이 발생할 수 있으며 중복된 데이터가 변경될 시에는 모든 컬렉션에서 수정을 해야한다. 아울러 막대한 데이터를 저장해야 해서 Database를 Scale-Out를 해야 되는 시스템에 적합.
