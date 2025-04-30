**Testcontainers** — это библиотека Java, которая позволяет прямо во время тестов **поднимать реальные контейнеры Docker** (PostgreSQL, Redis, RabbitMQ, Minio, Kafka и т.д.) для использования в тестах.

**Идея:**  
Вместо моков или "фейковых" БД ты тестируешь на настоящих сервисах, только в чистой изолированной среде.

---

## 🛠 Когда **НУЖНО** использовать **Testcontainers**:

|Ситуация|Почему использовать|
|---|---|
|Нужно тестировать реальное подключение к базе данных|Чтобы убедиться, что твой код работает с настоящей Postgres, а не с H2-эмуляцией.|
|Есть зависимость от внешних сервисов (Kafka, MinIO, Redis)|Чтобы тестировать не заглушки, а реальную инфраструктуру.|
|Пишешь интеграционные тесты, где важны транзакции, индексы, расширения БД|Например, TimescaleDB расширяет Postgres — H2 не поможет.|
|Хочешь быть уверенным в продакшн-работоспособности|Контейнер = 99% как реальный сервер.|
|Требуется чистая база перед каждым тестом|Контейнер можно заново поднять или почистить.|

---

## ⚡ Когда **НЕ НУЖНО** использовать **Testcontainers**:

|Ситуация|Почему не нужно|
|---|---|
|Пишешь обычные unit-тесты сервисов, без БД|Нет смысла поднимать тяжелые контейнеры.|
|Проверяешь бизнес-логику без внешних зависимостей|Лучше замокать все через `@MockBean` или Mockito.|
|Очень критичное время выполнения тестов|Контейнеры поднимаются не мгновенно (обычно 5-10 секунд).|
|Нет доступа к Docker в CI/CD среде|Тогда Testcontainers работать не сможет.|

---

## 📦 Пример использования Testcontainers с PostgreSQL

 Добавь зависимость в `pom.xml`:
```java
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <version>1.19.1</version> <!-- версия зависит от проекта -->
    <scope>test</scope>
</dependency>
```

Пример теста с поднятием реальной Postgres:
```java
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.DynamicPropertyRegistry;
import org.springframework.test.context.DynamicPropertySource;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

@SpringBootTest
@Testcontainers
public class UserRepositoryTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15")
            .withDatabaseName("testdb")
            .withUsername("user")
            .withPassword("password");

    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Test
    void testSomethingWithDatabase() {
        // Здесь будет реальная база Postgres из контейнера
    }
}
```

▶ Контейнер поднимается, подключается в Spring Boot как datasource, тесты работают на живой БД.

---

## 🚀 Плюсы Testcontainers:

- Тестируешь на реальных сервисах.
    
- Минимизируешь "сюрпризы" после деплоя на прод.
    
- Нет зависимости от состояния локальной БД/сервиса.
    
- Полностью изолированная среда для каждого теста.
    
- Легко интегрировать в CI/CD пайплайны (если там разрешен Docker).
    

## ⚠️ Минусы Testcontainers:

- Медленнее юнит-тестов.
    
- Требуется Docker на машине.
    
- Иногда сложнее настраивать кэширование образов в CI для ускорения.
    

---

## ✍️ Краткое резюме

| Пишешь обычные сервисы без БД                               | ➔ Unit-тесты без Testcontainers | 
| Пишешь сервисы, которые работают с БД/очередями | ➔ Интеграционные тесты с Testcontainers |
| Хочешь проверять реальное поведение системы       | ➔ Testcontainers — отличный выбор |