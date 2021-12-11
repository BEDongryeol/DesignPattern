## Factory Method Pattern (팩토리 메서드 패턴)

![Factory Method Pattern](https://reactiveprogramming.io/books/patterns/img/patterns-articles/factory-method-diagram.png)

### 1. 정의

생성 패턴 중 하나로, `인스턴스의 생성`을 하위 클래스에서 위임한다.

`new`를 통해 객체를 생성할 수도 있지만, `factory method`를 통해 `동적으로` 생성할 수 있다.
	- 예시) Map 등의 구조를 통해 keyword에 해당하는 객체를 생성해준다.

Template method에서 생성되는 부분만 `abstract`로 설정하여 패턴을 적용할 수도 있다.

### 2. 장점

- `클라이언트`와 객체 `생성 모듈` 간의 결합도를 낮출 수 있다.

- 결합도가 낮고, 자료형이 하위 클래스에 의해 결정되기 때문에 `확장이 용이`하다.

- 여러 객체를 `동일한 형태`로 프로그래밍 할 수 있다.

### 3. 단점

- 객체가 늘어날수록 클래스 수가 많아져 복잡도가 증가한다.

- 객체가 추가되면 하위 클래스를 재정의 해주어야한다.


### 4. 구현

![UML Diagram](https://reactiveprogramming.io/books/patterns/img/patterns-articles/factory-method-diagram.png)

1. IProduct: 원하는 객체를 추상화하여 생성한다 (Inteface)

2. Concrete Product: 객체를 생성한다.

3. Abstract Factory: 객체를 생성할 `Factory` 객체를 추상화하여 생성한다.
	- Concrete Factory의 `default` 동작을 추가하기 위한 권고 사항이다.
	- `Template Method Pattern`의 변하는 부분을 생성을 담당하는 factory에 위임하는 것이다.

4. Concrete Factory: 분기 등을 통해 객체를 생성할 로직을 작성한다.

### 5. 패턴의 흐름

![Sequence Diagram](https://reactiveprogramming.io/books/patterns/img/patterns-articles/factory-method-sequence.png)

1. Client가 객체 생성 메서드를 호출한다.

2. Concrete Factory가 분기를 통해 알맞은 객체를 생성하여 전달한다.


### 6. 활용

- 생성해야 하는 객체를 예측할 수 없을 때

- 동적으로 객체를 생성하고 싶을 때

Application에 1개 이상의 DB를 연결할 때, 동적으로 필요한 DB Connection을 생성하여 전달할 수 있다.
	- 이 때 Connection을 매번하는 것은 많은 비용이 요구되므로, 상황을 고려하여 static 등으로 할당해 놓는 것이 좋을 거 같다.

![DB에 적용](https://reactiveprogramming.io/books/patterns/img/patterns/factory-method.png)



참고 : ![Reactive Programming](https://reactiveprogramming.io/blog/en/design-patterns/factory-method)

