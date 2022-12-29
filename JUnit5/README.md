# JUnit 5
> 간단한 JUnit 5 사용법

- `@Test` : 메서드가 테스트 메서드임을 나타냅니다.
```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {

    private final Calculator calculator = new Calculator();

    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }

}
```

- `@DisplayName` : 테스트 클래스 또는 테스트 메서드에 대한 사용자 지정 표시 이름을 선언합니다.
```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("A special test case")
class DisplayNameDemo {

    @Test
    @DisplayName("Custom test name containing spaces")
    void testWithDisplayNameContainingSpaces() {
    }

    @Test
    @DisplayName("╯°□°）╯")
    void testWithDisplayNameContainingSpecialCharacters() {
    }

    @Test
    @DisplayName("😱")
    void testWithDisplayNameContainingEmoji() {
    }

}
```

- `@BeforeEach` : 주석이 달린 메서드가 현재 클래스의 각 테스트 메서드 전에 실행되어야 함을 나타냅니다.
- `@AfterEach` : 주석이 달린 메서드가 현재 클래스의 각 테스트 메서드 다음에 실행되어야 함을 나타냅니다.
- `@BeforeAll` : 주석이 달린 메서드가 현재 클래스의 모든 테스트 메서드 전에 실행되어야 함을 나타냅니다.
- `@AfterAll` : 주석이 달린 메서드가 현재 클래스의 모든 테스트 메서드 다음에 실행되어야 함을 나타냅니다.
- `@Disabled` : 테스트 클래스 또는 테스트 메서드를 비활성화하는 데 사용됩니다.

```java
import static org.junit.jupiter.api.Assertions.fail;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class StandardTests {

    @BeforeAll
    static void initAll() {
    }

    @BeforeEach
    void init() {
    }

    @Test
    void succeedingTest() {
    }

    @Test
    void failingTest() {
        fail("a failing test");
    }

    @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }

    @Test
    void abortedTest() {
        assumeTrue("abc".contains("Z"));
        fail("test should have been aborted");
    }

    @AfterEach
    void tearDown() {
    }

    @AfterAll
    static void tearDownAll() {
    }

}
```

# AssertJ
> JUnit의 Assert보다 테스트의 의도를 더 쉽게 파악할 수 있는 기능을 제공한다.

- 모든 assertThat 메소드 및 유틸리티 메소드의 진입점
```java
import static org.assertj.core.api.Assertions.*;
```

- 기본 Assertions
```java
assertThat(frodo.getName()).isEqualTo("Frodo");
assertThat(frodo).isNotEqualTo(sauron);
```

- 문자열 Assertions
```java
assertThat(frodo.getName()).startsWith("Fro")
                           .endsWith("do")
                           .isEqualToIgnoringCase("frodo");
```

- 컬렉션 Assertions
```java
assertThat(fellowshipOfTheRing).hasSize(9)
                               .contains(frodo, sam)
                               .doesNotContain(sauron);
```

- `as()` : 오류 메시지 앞 표시되고 테스트를 설명한다
```java
assertThat(frodo.getAge()).as("check %s's age", frodo.getName()).isEqualTo(33);
```

- 예외 Assertions
```java
assertThatThrownBy(() -> { throw new Exception("boom!"); }).hasMessage("boom!");
```

- 추출 기능
```java
assertThat(fellowshipOfTheRing).extracting(TolkienCharacter::getName)
                               .doesNotContain("Sauron", "Elrond");
```

- 튜플로 그룹화된 여러 값을 한 번에 추출
```java
assertThat(fellowshipOfTheRing).extracting("name", "age", "race.name")
                               .contains(tuple("Boromir", 37, "Man"),
                                         tuple("Sam", 38, "Hobbit"),
                                         tuple("Legolas", 1000, "Elf"));
```

- 컬렉션 필터링
```java
assertThat(fellowshipOfTheRing).filteredOn(character -> character.getName().contains("o"))
                               .containsOnly(aragorn, frodo, legolas, boromir);
```

- 필터링과 추출 결합
```java
assertThat(fellowshipOfTheRing).filteredOn(character -> character.getName().contains("o"))
                               .containsOnly(aragorn, frodo, legolas, boromir)
                               .extracting(character -> character.getRace().getName())
                               .contains("Hobbit", "Elf", "Man");
```

- Boolean Assertions
```java
assertThat("".isEmpty()).isTrue();
assertThat("foo".isEmpty()).isFalse();
```

- Iterable/Array Assertions
```java
List<String> list = Arrays.asList("1", "2", "3");

assertThat(list).contains("1");
assertThat(list).isNotEmpty();
assertThat(list).startsWith("1");
```

- Map Assertions
```java
assertThat(map)
  .isNotEmpty()
  .containsKey(2)
  .doesNotContainKeys(10)
  .contains(entry(2, "a"));
```

---

- [예제 소스](https://github.com/japygo/spring-junit-test)

---

# 참고 사이트
- [JUnit 5](https://junit.org/junit5/)
- [AssertJ](https://github.com/assertj/assertj)
- [Introduction to AssertJ](https://www.baeldung.com/introduction-to-assertj)