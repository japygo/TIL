# 간단한 Restful 웹 서비스 구축

> - http://localhost:8080/greeting 요청 시 아래 응답을 받는다.
> ```json
> {"id":1,"content":"Hello, World!"}
> ```
> - http://localhost:8080/greeting?name=User 요청 시 아래 응답을 받는다.
> ```json
> {"id":2,"content":"Hello, User!"}
> ```

### 1. 프로젝트 생성

- IDE에 Spring Initializr 통합이 있는 경우 IDE에서 프로젝트를 만든다.
- 또는 https://start.spring.io 에서 프로젝트를 만든다.
- 프로젝트 생성 시 `Dependencies`에 `Spring Web`을 추가한다.

### 2. 모델 클래스 생성

- 응답을 보낼 데이터를 모델링한 클래스를 만든다.
```java
public class Greeting {
    
    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```

### 3. 컨트롤러 생성
```java
@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
        return new Greeting(counter.incrementAndGet(), String.format(template, name));
    }
}
```

### 4. 서비스 확인

- IDE에서 웹 애플리케이션을 실행한다.
- 또는 `./gradlew build` 빌드 후 `java -jar 파일명.jar` 실행한다.
- http://localhost:8080/greeting 요청 후 응답을 확인한다.
- http://localhost:8080/greeting?name=User 요청 후 응답을 확인한다.

---

- ### [예제 소스](https://github.com/japygo/spring-restful-test.git)

---

# 참조 사이트

- [Spring Guides](https://spring.io/guides/gs/rest-service/)