
 ---
 title: 2026-03-05
 author: 강병호 (이름)
 date: 2026-03-05 (날짜)
 category: TIL/강병호/2026/03 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---



## SQL 인젝션(SQL Injection)의 정의

**SQL 인젝션**은 웹 애플리케이션에서 사용자로부터 입력받은 값을 적절한 검증 없이 SQL 쿼리에 포함시킬 때 발생하는 보안 취약점입니다. 공격자는 입력란에 특수문자나 SQL 구문을 삽입하여 애플리케이션의 의도와는 다른 쿼리가 실행되도록 조작합니다.

이를 통해 공격자는 다음과 같은 행위를 수행할 수 있습니다.

* **인증 우회:** 비밀번호를 모르더라도 관리자 계정으로 로그인
* **데이터 탈취:** 데이터베이스에 저장된 민감 정보 조회
* **데이터 조작 및 삭제:** 데이터 수정 또는 테이블 삭제(DROP)

---

## 작동 원리 분석

### 취약한 코드 예시 (Java/JDBC)

아래 코드는 사용자의 입력을 문자열 연결 방식(`+`)으로 직접 쿼리에 포함하고 있어 SQL 인젝션에 매우 취약합니다.

```java
public boolean login(String username, String password) {
    // 사용자 입력값을 직접 SQL 문자열에 결합 (위험)
    String sql = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'";
    
    try (Connection conn = DriverManager.getConnection("DB_URL");
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {
        return rs.next();
    } catch (SQLException e) {
        throw new RuntimeException("Database error", e);
    }
}

```

### 공격 시나리오

1. **인증 우회 (주석 사용)**
* 입력값: `username = admin' --`
* 생성된 쿼리: `SELECT * FROM users WHERE username = 'admin' --' AND password = '...'`
* 설명: `--` 이후의 구문이 주석 처리되어 비밀번호 검증 없이 `admin` 계정으로 로그인이 성공합니다.


2. **논리적 참 조건 활용**
* 입력값: `username = ' OR '1'='1`
* 생성된 쿼리: `SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '...'`
* 설명: `1=1`은 항상 참이므로 전체 조건이 참이 되어 첫 번째 사용자 계정으로 로그인됩니다.


3. **UNION 연산자 활용**
* 공격자는 `UNION SELECT`를 사용하여 다른 테이블(예: 결제 정보, 개인정보 테이블)의 데이터를 조회할 수 있습니다.



---

## SQL 인젝션 방어 전략

### 1. PreparedStatement 사용 (가장 권장됨)

`Statement` 대신 `PreparedStatement`를 사용합니다. 플레이스홀더(`?`)를 사용하면 DB 엔진이 쿼리 구조를 미리 컴파일하고, 입력값은 단순한 '파라미터'로만 취급합니다.

```java
String sql = "SELECT * FROM users WHERE username = ? AND password = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, username); // 입력값이 쿼리 구조를 변경할 수 없음
pstmt.setString(2, password);

```

### 2. ORM 프레임워크 활용

JPA(Java Persistence API)나 Hibernate와 같은 ORM을 사용하면 내부적으로 파라미터 바인딩을 수행하므로 직접적인 SQL 인젝션 위험을 크게 낮출 수 있습니다. 단, **Native Query**를 직접 작성할 때는 여전히 주의가 필요합니다.

### 3. 입력값 검증 (Input Validation)

사용자가 입력한 값에 `SELECT`, `DROP`, `UNION`, `--`, `;` 등 SQL 예약어가 포함되어 있는지 검증하는 로직을 추가합니다. 하지만 이는 우회 가능성이 있으므로 보조적인 수단으로 활용해야 합니다.

### 4. 데이터베이스 권한 최소화 (Least Privilege)

웹 애플리케이션이 사용하는 DB 계정에 모든 권한을 주지 않습니다. 서비스 운영에 필요한 `SELECT`, `INSERT`, `UPDATE` 권한만 부여하고 `DROP TABLE`과 같은 관리 권한은 차단하여 피해를 최소화합니다.

### 5. 에러 메시지 노출 제한

데이터베이스 에러 발생 시 상세한 쿼리 내용이나 구조를 사용자에게 노출하지 않습니다. 공격자는 에러 메시지를 통해 테이블 명이나 컬럼 명 정보를 유추할 수 있기 때문입니다.
