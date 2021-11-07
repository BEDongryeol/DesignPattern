> ## Adapter Pattern 이란 무엇인가?

어댑터 패턴은 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스를 변환해주는 패턴으로, **호환성이 없는 인터페이스로 인해 호환이 되지않는 클래스들을 함께 작동할 수 있도록 해준다.**

대부분의 글에서 이 어댑터를 '돼지코'라 불리는 변환 어댑터를 예시로 듭니다.

또한 아래와 같은 사진을 자주 보실 수 있습니다.

![image](https://t1.daumcdn.net/cfile/tistory/99AAB74D5C305D4721)

**여기서 Client는 사용자를 뜻합니다.**

**Target은 돼지코를 꽂을 수 있는 콘센트입니다.** 일본에 여행을 가서 핸드폰을 충전하려고 보니 콘센트가 110V 충전기만 호환되도록 **11자 모양**으로 구멍이 뚫려있습니다. 

**Adaptee는 제가 가지고 있는 220V 충전기**인데 일본 콘센트(Target)에 꽂을 수가 없겠죠?

이 때 필요한게 **Adapter, 돼지코**입니다. {% hl_text success %}돼지코는 220V의 콘센트를 받아 110V로 변환시켜줍니다. 돼지코의 단자, 출력은 Target(110V 콘센트)과 같은 110V입니다.{% endhl_text %} 이 부분을 계속 떠올리셔야 헷갈리지 않게 구현할 수 있습니다.

> ####  **여기서 포인트는 기존의 충전기 자체를 변경하지않고, 그대로 사용할 수 있는 것입니다.**


> ## Adapter Pattern의 구현 

돼지코를 통해 전체적인 흐름을 보았으니, 실무적인 영역으로 넘어가 보겠습니다.

**1. Client는 항상 같은 Target을 통해 Web에 특정한 요청을 하고 있습니다.**

**2. Client에게 새로운 기능을 제공하기 위해 외부에 요청하여 바이트코드 파일을 전달받았습니다.**
- 기존에 기능을 제공하는 Interface와 호환되지 않고, 직접적인 접근이 불가능합니다.

**3. Adapter를 통해 기존 Interface와 외부 코드를 호환시켜줍니다.**
- 이때 Adapter는 위의 돼지코 단자, 출력이 Target, 110V와 같은 것과 같이 기존 Interface와 같아야합니다.

#### 1. Target (콘센트, WebRequester)
- Target은 **WebRequester** 입니다.

```java
public interface WebRequester {
    void requestHandler();
}
```

#### 2. 외부 코드
- 기존 Target Interface와 호환이 되지 않는 형태입니다.
- 예시에서는 제가 임의로 작성하였기에 직접적인 접근도 가능하고 수정이 가능하지만, 바이트코드로 저장해서 불러오면 직접적인 접근이 어렵습니다.
```java
public class NewService {
    public void newService(){
        System.out.println("New Service");
    }
}
```

#### 3. Adapter 생성
- **계속 강조했던 부분이 나옵니다.** 아래 질문에 대해 생각을 잠시 해보고 넘어가시면 좋습니다.
  - **Target(콘센트)은** 몇 V였나요?
  - 돼지코는 220V를 호환시켜주기 위해 **220V를 받아주고**, **결국 몇 V 였을까요?**

여기서부터는 다시 정리를 하며 한 줄씩 작성해 나가보겠습니다.

**3-1. 어댑터 클래스를 만들어줍니다.**
```java
public class Adapter {

}
```
```markdown
질문1. 돼지코 예제에서 Target은 몇 V였나요?

질문2. 돼지코는 몇 V에 꽂으려한 거 였을까요?

< 정답 : 콘센트와 돼지코는 호환이되는 110V였습니다. >
```

**3-2. Adapter를 Target과 호환시키기 위해 형태를 맞춰줍니다.**
- Target, Adapter는 콘센트와 돼지코처럼 호환되기 위해 형태가 같아야겠죠?
- Adapter class에 WebRequester를 implements해줍니다.
    - Interface였으니까 Override를 해주어야겠죠?

```java
public class Adapter implements WebRequester{

    @Override
    public void requestHandler() {
        
    }
}
```

#### 4. 호환시켜주고 싶은 Adaptee를 Adapter에 꽂아줍니다.
- 돼지코만 콘센트에 꽂아두면 안되겠죠 ?, 돼지코에 220V 충전기(Adaptee)를 연결해주어야 의미가 있는 거겠죠 !
- **여기서 Adaptee는 2번에서 보았던 외부 코드, 새로운 기능입니다.**
- NewService 클래스를 Adapter에 주입해주었습니다 (돼지코(Adapter)에 220V(NewService)를 꽂아준 것입니다.)
    - 다른 클래스의 객체를 내부에 주입시키는 것을 의존성 주입(Dependency Injection)이라고 합니다.

```java
public class Adapter implements WebRequester{
    NewService newService;

    public Adapter(NewService newService){
        this.newService = newService;
    }

    @Override
    public void requestHandler() {
        newService.newService();
    }
}
```

#### 5. Adapter Pattern 완성
- 4번과 같이 의존성 주입을 통해 호환이 되지 않던 기능과 인터페이스가 Adapter를 통해 호환이 되게 되었습니다!
- 테스트를 해보겠습니다.
- 궁극적인 형태는 WebRequester(Target)였죠?
    - ```WebRequester webRequester =```
- NewService를 Adapter에 꽂아 형태를 맞춰줍니다.
  - ```new Adapter(new NewService())```

```java
public class Main {
    public static void main(String[] args) {

        WebRequester webRequester = new Adapter(new NewService());
        webRequester.requestHandler();
    }
}
```



