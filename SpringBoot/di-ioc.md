# DI(Dependency Injection) & IOC(Inversion of Control)

DI(의존성 주입)은 프로그램이 자신이 수행해야 할 작업에 필요한 의존성(서비스 또는 객체)을 전달받는 대신 직접 생성하는 것을 방지하는 디자인 패턴이다.
이는 코드를 더 모듈화하고, 테스트 가능하고 유지 관리가 용이하게 만든다.

IOC(제어의 역전)은 소프트웨어 엔지니어링의 원칙으로, 객체 또는 프로그램의 부분의 제어권을 컨테이너 또는 프레임워크에 전달하는 것을 말한다.
이는 서비스 구현과 응용 프로그램에서 사용되는 방식을 분리하는 데 도움이 되어, 의존성 주입(DI)과 같은 기술을 통해 이루어질 수 있다.

## DI와 IOC 예시

"MessageService"라는 인터페이스와 "EmailService"라는 이 인터페이스를 구현한 클래스가 있다고 가정해봅시다.

```java
public interface MessageService {
    void sendMessage(String message, String recipient);
}

public class EmailService implements MessageService {
    public void sendMessage(String message, String recipient) {
        // send email logic
    }
}
```

"MessageService"를 사용하여 메시지를 보내는 "MyClient" 클래스가 있다고 가정해봅시다.
DI 없이 "MyClient" 클래스는 "EmailService" 인스턴스를 직접 생성하고 이를 사용하여 메시지를 보냅니다.

```java
public class MyClient {
    private MessageService service = new EmailService();

    public void sendMessage(String message, String recipient) {
        service.sendMessage(message, recipient);
    }
}
```

DI의 경우, "MyClient" 클래스는 "MessageService"의 인스턴스를 생성하지 않고 생성자 인수로 받습니다.

```java
public class MyClient {
    private MessageService service;

    public MyClient(MessageService service) {
        this.service = service;
    }

    public void sendMessage(String message, String recipient) {
        service.sendMessage(message, recipient);
    }
}
```

"MyClient" 클래스의 호출자가 "MessageService"를 사용할 구현체를 제어하는 것이 바뀌었으며, "MyClient" 클래스에 영향을 미치지 않고 다른 "MessageService" 구현체를 제공 할 수 있게 되었습니다.

```java
MessageService testService = new MessageService() {
    public void sendMessage(String message, String recipient) {
    // test implementation
    }
};

MyClient client = new MyClient(testService);
```

"MyClient" 클래스에서 "MessageService"의 사용 구현을 제어할 수 없고, 이 제어권은 "MyClient" 클래스의 호출자에게 넘어갔습니다.
호출자는 "MyClient" 클래스에서 사용할 다른 "MessageService" 구현을 제공할 수 있습니다.
이는 IOC의 목적을 달성하는 예입니다.
