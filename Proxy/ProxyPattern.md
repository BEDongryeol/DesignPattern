# Proxy Pattern

## 1. 정의

객체에 대한 접근을 제어하기 위해 `대리자(proxy)`를 둔다.

`proxy`를 통해 대상 객체에 접근하여, 해당 객체가 메모리에 존재하지 않아도 기본적인 정보를 참조하거나 설정할 수 있고 또한 실제 객체의 기능이 반드시 필요한 시점까지 객체의 생성을 미룰 수 있다.

> 10개의 method를 가지고 있는 객체가 있고 3개의 고비용 method, 나머지 7개는 다른 객체가 대신할 수 있는 가벼운 method가 있다고 가정하자.

- 이 때 `proxy를 생성`하여 `가벼운 method`를 처리하고, 고비용의 method가 필요한 시점에만 객체를 생성하는 것이다.

### Proxy란 무엇인가?
<details>
<summary>사전적 정의</summary>

- 선거에서 자신에게 투표하는 것과 같이 다른 사람을 `대신하여 행동`하기 위해 사람에게 부여된 권한 또는 이 권한이 부여된 사람

- 대리 운전, 구매 대행, 대리인 등에 사용된다.
</details>

---

## 2. 장점

- 초기화 비용이 많이 들거나 복잡한 객체를 proxy를 통해 `Lazy`하게 처리할 수 있다.

- 객체의 접근에 대해 사전처리를 할 수 있다.

---

## 3. 단점

- 객체 생성 시 `proxy`를 거치게 되고, proxy 내부에 동기화가 필요한 경우, 성능 저하의 우려가 있다.

---

## 4. 구현

![UML Diagram](https://reactiveprogramming.io/books/patterns/img/patterns-articles/proxy-diagram.png)

### 1. IObject
- `객체`와 `proxy`의 추상화 객체이다.

### 2. Object
- 클라이언트가 실제로 접근하려는 객체

### 3. Proxy
- Object에 접근 시 Object 앞 단에서 접근을 제어하는 proxy 객체

---

## 5. 흐름

![Sequence Diagram](https://reactiveprogramming.io/books/patterns/img/patterns-articles/proxy-sequence.png)

1. Client가 요청을 보내면 proxy를 return 해준다.
2. 실제 객체가 필요한 요청을 보낼 때만 실제 객체가 호출되고, 나머지는 `proxy`가 처리해준다. 


---

## 6. 활용

- 사용자가 서비스에 접근할 때 권한이 있는지 proxy를 통해 체크하고, 유효할 때만 실제 객체에 접근할 수 있도록 구성할 수 있다.

- 큰 이미지 파일과, 텍스트가 같이 있는 웹 페이지에 접근 시 무거운 이미지가 load 될 동안 텍스트를 먼저 제공하도록 구성한다.  
