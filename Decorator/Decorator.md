> ## 데코레이터 패턴이란?

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fp9EO8%2FbtqCcVHXQ8F%2Fxjbvd3dhLc6RN9ONulsjm1%2Fimg.png)

### 특징
- 여러 decorator들을 활용하여 다양한 기능을 제공한다.
    - 상속보다 유연한 방식을 갖는다.
    - 지속적인 기능의 추가 제거가 용이하다.
- 다른 데코레이터나 컴포넌트를 포함하여야한다.    
    - 기반 스트림 클래스 : 직접 읽고 쓸 수 있다. (Component)
    - 보조 스트림 : 추가 기능 제공 (Decorator)
    
### 구성
- 최상위 컴포넌트 (예시 : Coffee)
- 하위 컴포넌트 (예시 : EtiopiaAmericano)
- 데코레이터 (예시 : Decorator)
- 하위 데코레이터 (예시 : Moch, Latte)

- 하위 컴포넌트와 데코레이터는 최상위 컴포넌트를 상속받는다.
- 데코레이터는 혼자 쓰이지않고, 상속용(abstract)으로 구현한다.
  > @Decorator
  > - 최상위 컴포넌트의 default Constructor가 없으므로, 생성자를 명시해준다.
  > 
  > @하위 Decorator
  > - 상위 클래스 Decorator의 default 생성자가 없으므로 매개변수가 있는 super를 명시적으로 호출해야한다.
- 하위 데코레이터는 데코레이터를 상속받아 각 기능을 구현한다.
> 하위 컴포넌트 생성시 하위 데코레이터를 wrapper 형식을 통해 원하는 기능을 추가한다.


<details>
<summary>최상위 컴포넌트 예제코드</summary>

```markdown
public abstract class Coffee {

    public abstract void brewing();
}
```
</details>

<details>
<summary>하위 컴포넌트 예제 코드</summary>

```java
public class EtiopiaAmericano extends Coffee{
    @Override
    public void brewing() {
        System.out.println("Etiopia Americano");
    }
}
```
</details>

<details>
<summary>데코레이터 예제코드</summary>

```java
// 데코레이터는 혼자 쓰이지 않는다. (상속용으로 쓰인다.)
public abstract class Decorator extends Coffee{

    // component를 멤버 변수, 생성자 parameter로 갖는다.
    Coffee coffee;
    public Decorator(Coffee coffee){
        this.coffee = coffee;
    }

    @Override
    public void brewing() {
        coffee.brewing();
    }
}
```
</details>

<details>
<summary>하위 데코레이터 예제코드</summary>

```java
public class Moch extends Decorator{
    public Moch(Coffee coffee) {
        super(coffee);
    }

    @Override
    public void brewing() {
        super.brewing();
        System.out.println("Adding Moch syrup");
    }
}
```

```java
public class Latte extends Decorator{
    // 상위 클래스 Decorator의 default 생성자가 없으므로 매개변수가 있는 super를 명시적으로 호출해야한다.
    public Latte(Coffee coffee) {
        super(coffee);
    }

    @Override
    public void brewing() {
        super.brewing();
        System.out.print("Adding Milk");
    }
}
```
</details>

<details>
<summary>테스트 코드</summary>

```java
public class CoffeeTest {
    public static void main(String[] args) {
        // 하위(상세) 컴포넌트 생성
        Coffee etio = new EtiopiaAmericano();
        etio.brewing();

        // 하위(상세) Decorator로 기능을 추가한 컴포넌트
        Coffee mochEtio = new Moch(new EtiopiaAmericano());
        mochEtio.brewing();
    }
}
```
</details>
