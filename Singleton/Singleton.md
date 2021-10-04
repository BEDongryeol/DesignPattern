> ## Singleton Pattern 이란 무엇인가?
프로그램 내에서 오로지 1개만 존재하는 객체를 생성하여, 내부에서 이를 호출하여 사용하는 경우가 있습니다.

이벤트의 스케줄링, 환경설정, 로그 파일의 생성, 하드웨어 기기의 인터페이스, 캐시 등의 다양한 케이스가 있습니다.

위와 같은 상황에서 **Singleton 패턴**을 이용하여 객체를 프로그램 내부에서 단 1개만 생성하고,
멀티 쓰레드 환경에서 이 객체를 공유하고, 동시 접근으로 인해 발생하는 동시성 문제도 해결할 수 있습니다.

### 정리
- #### **Singleton 패턴**
  1. 프로그램 내에 특정 객체가 단 1개가 존재한다.
  2. 프로그램 내부의 다른 부분에서 이 객체를 공유하며 사용한다.

> ## Singleton를 사용하는 이유

### 장점
- 인스턴스를 1개만 생성함으로써, 메모리 공간을 절약할 수 있다.
- 호출할 때마다 생성하지 않아도 되므로, 로딩 시간이 절약된다.
- 어디서든 전역 변수처럼 사용할 수 있고, 다른 인스턴스들과 데이터 공유가 잘 이루어진다.

### 유의 사항
- 클래스 내부에서 객체를 직접 생성하기 때문에 DIP(Dependency Inversion Principle) 를 위배하게 된다.
  - 특정 인터페이스에 의존하는 것이 아닌 한 개의 인스턴스에 의존, 전역적으로 직접 참조하게 됩니다.
  - 이는 의존성 주입(Dependency Injection)을 통해 피할 수 있습니다. 
  
- 멀티 쓰레드 환경에서 안전하게 구현하려면 **Thread-Safe**, **동기화** 처리를 해주어야한다.

> ## Singleton 구현

### 기본 구현
```java
    // 객체에 대한 직접적인 접근을 막기위해 private으로 생성
    private static CentralBank centralBank;
    // 생성자를 외부에서 접근할 수 없도록 private으로 설정
    private CentralBank(){};
    // 객체에 접근할 수 있는 public 코드 작성
    public static CentralBank getCentralBank(){
        // 객체가 null이면 객체 생성
        if (centralBank == null) {
            centralBank = new CentralBank();
        }
        // 객체 return
        return centralBank;
    }
```
인스턴스와 생성자 모두 private으로 설정하여 외부에서 직접적인 접근을 막아줍니다.

getCentralBank() 메서드를 통해서만 인스턴스에 접근이 가능합니다.

**하지만 위 코드는 멀티 쓰레드 환경에서 Thread-safe**를 보장하지 않습니다.

인스턴스가 null인 상태에서 두 개의 쓰레드에서 동시에 getInstance를 호출하면 instance를 두 번 생성할 수 있게 됩니다. 

이를 해결하기 위해서는 **동기화**가 필요합니다.

### 동기화 구현
```java
    // 객체에 대한 직접적인 접근을 막기위해 private으로 생성
    private static CentralBank centralBank;
    // 생성자를 외부에서 접근할 수 없도록 private으로 설정
    private CentralBank(){};
    // 객체에 접근할 수 있는 public 코드 작성
    public static synchronized CentralBank getCentralBank(){
        // 객체가 null이면 객체 생성
        if (centralBank == null) {
            centralBank = new CentralBank();
        }
        // 객체 return
        return centralBank;
    }
```

여러 쓰레드가 동시 접근이 가능한 메서드에 ```synchronized``` 키워드를 추가하여 이를 해결할 수 있습니다.

위 코드에서는 Thread-safe는 보장되지만 **속도가 저하된다는 문제가 발생합니다.**

반갑게도 Thread-safe와 속도를 모두 보장하는 방법이 존재합니다.

아래에서 두 가지 예시로 살펴보도록 하겠습니다.

### Double Check Lock(DCL) 방식

```java
public class CentralBank {
  // volatile 키워드를 통해 cache가 아닌 main memory에서 데이터를 읽고 쓴다.
  private volatile static CentralBank centralBank;

  private CentralBank() {
  }

  public static CentralBank getCentralBank() {
    // null인지 1번 체크
    if (centralBank == null) {
      synchronized (CentralBank.class) {
        // 동기화 후에 null인지 다시 한번 체크 (Double check)
        if (centralBank == null) {
          centralBank = new CentralBank();
        }
      }
    }
    return centralBank;
  }
}
```

여러 쓰레드가 접근하는 인스턴스에 ```volatile``` 키워드를 추가해줍니다.
  - volatile 키워드가 없을 때, 멀티 쓰레드 어플리케이션에서는 작업을 수행하는 동안 **성능 향상**을 위해 메인 메모리에서 읽은 변수의 값을 Cache에 저장합니다.
  - 이 때, 각 CPU Cache에 저장되는 값이 불일치하는 상황이 발생할 수 있습니다.
  - volatile 키워드를 추가하여 각 CPU의 Cache가 아닌 메인 메모리에 값을 저장하고 읽어오기 때문에, 불일치 문제를 해결할 수 있습니다.  
  
인스턴스를 return해주는 ```getCentralBank()```함수에서 **Double-Check**를 수행하게 됩니다.
1. **First Check**
  - 인스턴스를 호출할 때 null이면 동기화를 통해 class에 대한 접근을 Lock합니다.
2. **Second Check**
  - 접근을 막은 상태에서 다시 한 번 인스턴스가 null인지 확인하고, null이면 객체를 생성합니다.

### LazyHolder 방식

```java
public class CentralBank {
  // 외부에서의 생성을 막기위해 private으로 설정
  private CentralBank(){}
  // private한 내부 클래스를 만들고, 그 안에서 final로 인스턴스를 상수화시켜 생성
  private static class LazyHolder{
    private static final CentralBank centralBank = new CentralBank();
  }
  // 호출 시 클래스에서 인스턴스를 호출
  public CentralBank getCentralBank(){
    return LazyHolder.centralBank;
  }
}
```

LazyHolder 방식은 **윌리엄 푸**가 제안한 방법으로 **Bill Pugh Solution**으로 불리기도 합니다.

클래스 내부에 private static class를 만들어 사용하고 싶을 때 public static 메서드를 통해 호출하게 됩니다.

이는 ```private static class```의 특징을 이용하였습니다.
  - **private static class**
    - static이지만 메모리에 바로 올라가지 않고, 누군가가 getCentralBank() 메서드를 호출할 때만 호출이 되게 됩니다.
    - 처음 호출할 때 JVM이 LazyHolder를 load하여 초기화합니다. 
    - LazyHolder 클래스를 초기화하면 인스턴스(centralBank)가 초기화시키기 위해 생성자를 호출합니다.
    - 클래스 초기화 단계는 **JLS에 의해 순차적으로 보장**되므로, 호출 시에 추가적인 동기화는 불필요하게 됩니다.
    - 또한 static 변수인 centralBank를 순차적으로 입력하기 때문에 동기화가 불필요합니다.
    
내부 변수 또한 ```final```키워드를 통해 차후에 값이 변하지 않도록 설정하였습니다.


