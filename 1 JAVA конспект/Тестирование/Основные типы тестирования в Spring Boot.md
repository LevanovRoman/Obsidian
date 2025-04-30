## 🧩 Основные типы тестирования в Spring Boot:

|Тип теста|Что тестирует|Аннотации|
|---|---|---|
|**Unit-тесты**|Отдельные классы и методы без Spring Context|`@Test` (из JUnit 5), без Spring|
|**Интеграционные тесты**|Взаимодействие компонентов с подъемом Spring Context|`@SpringBootTest`|
|**Web-тесты**|Тестирование REST-контроллеров|`@WebMvcTest`, `@MockMvc`|
|**Тестирование репозиториев**|JPA-репозитории и запросы к БД|`@DataJpaTest`|

---

## 🛠 Что нужно для тестирования

Spring Boot тестирует на базе **JUnit 5** и **Spring Test Framework**.

Если у тебя Maven, подключи зависимости в `pom.xml`:
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

Эта зависимость подтянет:

- JUnit 5 (основа тестов)
    
- Mockito (моки)
    
- AssertJ (удобные ассерты)
    
- Spring Test (для интеграционных тестов)
    
- Hamcrest (еще ассерты)
    

---

## 🧪 Примеры тестов

### 1. Unit-тест обычного сервиса (без Spring Boot Context)
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorServiceTest {

    private final CalculatorService calculatorService = new CalculatorService();

    @Test
    void addNumbers() {
        assertEquals(5, calculatorService.add(2, 3));
    }
}
```

**Когда использовать:** Проверка отдельных методов, простые юнит-тесты.

---

### 2. Интеграционный тест всего приложения
```java
import org.springframework.boot.test.context.SpringBootTest;
import org.junit.jupiter.api.Test;

@SpringBootTest
class ApplicationTest {

    @Test
    void contextLoads() {
        // Проверяем, что контекст стартует без ошибок
    }
}
```

**Когда использовать:** Проверить, что все компоненты правильно поднимаются вместе.

---

### 3. Тестирование REST-контроллера через `MockMvc`
```java
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.web.servlet.MockMvc;
import org.junit.jupiter.api.Test;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(MyController.class)
class MyControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void testHelloEndpoint() throws Exception {
        mockMvc.perform(get("/hello"))
            .andExpect(status().isOk())
            .andExpect(content().string("Hello, World!"));
    }
}
```

**Когда использовать:** Проверка, что контроллеры правильно обрабатывают запросы.

---

### 4. Тестирование репозитория JPA (`@DataJpaTest`)
```java
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.beans.factory.annotation.Autowired;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;

@DataJpaTest
class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    void testSaveUser() {
        User user = new User();
        user.setName("John");
        userRepository.save(user);

        assertThat(userRepository.findByName("John")).isNotNull();
    }
}
```

**Когда использовать:** Тестирование логики базы данных через JPA.

---

## 🚀 Быстрые советы:

- Для ускорения тестов можно использовать **H2 in-memory database** для тестирования репозиториев вместо реальной PostgreSQL.
    
- **Mock сервисы** через `@MockBean`, чтобы не дергать реальные зависимости в интеграционных тестах.
    
- Тесты Spring Boot удобно запускать через `mvn test` или прямо в IDE.
    

---

## 📜 Что стоит помнить:

- **Unit-тесты** → Проверяем бизнес-логику изолированно.
    
- **Интеграционные тесты** → Проверяем связку сервисов, контроллеров, баз данных.
    
- **MockMvc** → Легкие тесты API без поднятия сервера.
    
- **SpringBootTest** → Полная загрузка контекста, тяжелее и дольше.

