## Template Method Pattern (템플릿 메서드 패턴)

![Template Method](https://miro.medium.com/max/786/1*pdT0SiFTawD8j4KWkJRvdA.png)

### 정의
비슷한 형태의 프로그램의 구현에 필요한 `뼈대를 일반화`하여 제공하는 패턴입니다.

알고리즘의 `구조를 변경하지 않고` 알고리즘의 각 단계별로 수행하는 구체적이고, 변화할 수 있는 코드를 `재정의`할 수 있다.


### 장점
- 소스 코드상의 알고리즘에서 재정의(확장, 변경 등)가 필요한 경우 유용하다.
- 코드의 중복이 감소된다.
- 핵심 로직 관리가 유용하다.

### 단점
- 추상 클래스와 자식 클래스의 복잡성이 높아진다

---

![Template Method UML](https://miro.medium.com/max/838/0*F8RVTE7IqH2-DUP7.png)

### 구현
> - 알고리즘을 구성하는 `메서드 목록`과 그 `호출 순서(알고리즘)`을 가진 `템플릿 메서드`
> - 서브클래스가 꼭 `Override` 해야 할 `추상 메서드`
> - 서브클래스가 `Override` 해도 되는 `훅 메서드`(hook method)

#### hook method : 필요할 시 서브 클래스에서 확장할 수 있는 기본적인 행동을 제공하는 메서드

### 고려 사항
- 템플릿 메서드 내의 알고리즘에 해당하는 메서드를 **템플릿 메서드만 사용할 수 있도록 한다.**
    - Keyword : `protected`
- `템플릿 메소드`는 오버라이드 할 수 없도록 구현한다.
    - Keyword : `final`
- 추상 메소드는 접두사를 붙여 `가독성`을 높여준다.
 

### 예제 코드
- `공통되는 로직`과 `개별 로직의 분리`
- 개별 로직 구현

### Template Method
```java
public abstract class QueryExecuteDB {

    Connection conn;
    PreparedStatement stmt;

    public final void execute(){
        getConnection();
        executeQuery();
        closeConnection(stmt, conn);
    }

    // DB 연동
    private void getConnection(){
        System.out.println("DB가 연결되었습니다.");
    }

    // DB 연결 종료
    private void closeConnection(PreparedStatement stmt, Connection conn){
        System.out.println("DB 연결이 종료되었습니다.");
    }

    protected abstract void executeQuery();
}
```

### Concrete Class

```java
public class CreateExecution extends QueryExecuteDB {
    @Override
    protected void executeQuery() {
        System.out.println("Create Query가 실행됩니다.");
    }
}
```

```java
public class DeleteExecution extends QueryExecuteDB {
    @Override
    protected void executeQuery() {
        System.out.println("Delete Query가 실행됩니다.");
    }
}
```
