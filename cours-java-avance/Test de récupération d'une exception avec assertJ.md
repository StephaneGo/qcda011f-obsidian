

Date: 18/06/2026
Tags: 
[[-Spring framework]]
[[-Tests]]


# Notes

# vérification de déclenchement d'une exception avec assertJ dans une appli spring boot

Voici un exemple concret de vérification du déclenchement d'une exception avec **AssertJ** dans un test **Spring Boot** (JUnit 5).[^1][^3]

## 1. Exemple minimal

```java
// Imports
import org.junit.jupiter.api.Test;
import org.assertj.core.api.Assertions;
import static org.assertj.core.api.Assertions.assertThatThrownBy;

class CalculatorTest {

    @Test
    void whenDivideByZero_thenThrowArithmeticException() {
        Calculator calculator = new Calculator();

        assertThatThrownBy(() -> calculator.divide(10, 0))
            .isInstanceOf(ArithmeticException.class)        // type d'exception
            .hasMessage("Division by zero");                // message exact [web:23]
    }
}
```

Avec cette classe :

```java
public class Calculator {
    public int divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Division by zero");
        }
        return a / b;
    }
}
```


## 2. Avec une exception personnalisée (propriétés)

Si tu as une exception avec des propriétés, tu peux les extraire :

```java
class CustomException extends RuntimeException {
    private int errorCode;

    public CustomException(String message, int errorCode) {
        super(message);
        this.errorCode = errorCode;
    }

    public int getErrorCode() {
        return errorCode;
    }
}
```

Test :

```java
@Test
void whenCustomException_thenCheckProperties() {
    assertThatThrownBy(() -> {
        throw new CustomException("Custom error", 404);
    })
        .isInstanceOf(CustomException.class)
        .hasMessage("Custom error")
        .extracting("errorCode")  // appel au getter errorCode
        .isEqualTo(404);
}
```


## 3. Vérification de la cause de l'exception

Pour vérifier la cause (par exemple un `IllegalStateException` dans une `RuntimeException`) :

```java
class DummyService {
    public void someOtherMethod(boolean b) {
        throw new RuntimeException(
            "Runtime exception occurred",
            new IllegalStateException("Illegal state")
        );
    }
}
```

Test :

```java
@Test
void verifiesCauseType() {
    DummyService service = new DummyService();

    assertThatThrownBy(() -> service.someOtherMethod(true))
        .isInstanceOf(RuntimeException.class)
        .hasMessage("Runtime exception occurred")
        .hasCauseInstanceOf(IllegalStateException.class)  // type de la cause
        .havingCause()                                    // puis cause plus précise
        .withMessageMatching(".*Illegal state.*");
}
```


## 4. Dans un contexte Spring Boot (service + dépendance)

Exemple avec un service qui lance une exception si l'utilisateur n'existe pas :

```java
@Service
class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUserById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new IllegalArgumentException("User not found: " + id));
    }
}
```

Test avec `@SpringBootTest` :

```java
@SpringBootTest
class UserServiceTest {

    @Autowired
    private UserService userService;

    @MockBean
    private UserRepository userRepository;

    @Test
    void whenUserNotFound_thenThrowIllegalArgumentException() {
        Long id = 999L;
        when(userRepository.findById(id)).thenReturn(null);

        assertThatThrownBy(() -> userService.getUserById(id))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessage("User not found: 999");
    }
}
```


## 5. Points importants

- Utilise `assertThatThrownBy` pour séparer l’acte (`act`) de l’assertion (`assert`).[^3]
- Toujours vérifier :
    - le type avec `isInstanceOf(...)`
    - le message avec `hasMessage(...)`
- Pour distinguer la cause : `hasCauseInstanceOf(...)`, `havingCause()`, `withMessageMatching(...)`.[^4][^1]
- Si aucune exception n’est levée, le test échoue avec :
`Expecting code to raise a throwable.`.[^4]

Si tu veux, je peux adapter cet exemple à un cas concret de ton projet (service, repository, exception métier, etc.).
<span style="display:none">[^10][^2][^5][^6][^7][^8][^9]</span>

<div align="center">⁂</div>


# Références


[^1]: https://www.wimdeblauwe.com/blog/2020/05/08/assertj-test-cause-of-exception/

[^2]: https://agileek.github.io/java/2015/04/06/assertJ-3.0.0/

[^3]: https://www.machinet.net/tutorial-fr/deep-dive-assertThatThrownBy-java-testing

[^4]: https://java.19633.com/fr/tag-java-1/assert-1/1001004455.html

[^5]: https://www.developpez.net/forums/d2155113/java/plateformes-java-ee-jakarta-ee-spring-serveurs/spring/spring-boot/test-unitaires-exception-bien-levee-ne-renvoie-message-attendu/

[^6]: https://www.danielassani.com/courses/developpement-backend-robuste-avec-java-et-spring-boot/lessons/developpement-backend-robuste-avec-java-et-spring-boot-gestion-des-erreurs-et-validation-des-donnees-dans-spring-boot

[^7]: https://www.youtube.com/watch?v=hKahfnKiJV8

[^8]: https://openclassrooms.com/fr/courses/6100311-testez-votre-code-java-pour-realiser-des-applications-de-qualite/6440256-donnez-du-sens-a-vos-assertions-avec-assertj

[^9]: https://www.capfi.fr/la-newsroom/blog/monitorer-les-exceptions-dans-des-applications-spring-boot/

[^10]: https://fr.linkedin.com/pulse/handling-custom-exceptions-spring-boot-comprehensive-guide-asim-2bbgf?tl=fr

