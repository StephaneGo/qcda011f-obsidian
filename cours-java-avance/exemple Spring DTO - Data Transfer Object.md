

Date: 24/06/2026
Tags: 

[[-Spring framework]]

# Notes

# peux tu me donner des exemples simples d'utilisation de DTO dans une application java spring boot

Oui — dans une appli **Spring Boot**, un DTO sert surtout à transporter des données entre le contrôleur, le service et le client, sans exposer directement l’entité JPA. C’est utile pour simplifier la réponse JSON, éviter d’envoyer des champs sensibles et adapter les données selon le besoin de l’API.[^1][^5][^9]

## Exemple 1 : création d’un utilisateur

Tu peux définir un DTO pour l’entrée, avec seulement les champs nécessaires à la création.

```java
public class CreateUserDTO {
    private String username;
    private String email;

    public CreateUserDTO() {}

    public CreateUserDTO(String username, String email) {
        this.username = username;
        this.email = email;
    }

    // getters et setters
}
```

Dans le contrôleur :

```java
@PostMapping("/users")
public ResponseEntity<UserDTO> createUser(@RequestBody CreateUserDTO dto) {
    User user = userService.create(dto);
    return ResponseEntity.ok(new UserDTO(user.getId(), user.getUsername(), user.getEmail()));
}
```

Ici, le client n’envoie pas l’`id`, car il est généré par la base, ce qui est un cas classique d’usage des DTO.[^5][^9]

## Exemple 2 : DTO de sortie

Pour renvoyer des données au client, tu peux utiliser un autre DTO plus adapté à l’affichage.

```java
public class UserDTO {
    private Long id;
    private String username;
    private String email;

    public UserDTO(Long id, String username, String email) {
        this.id = id;
        this.username = username;
        this.email = email;
    }

    // getters
}
```

Le service peut transformer l’entité en DTO avant de l’envoyer :

```java
public UserDTO getUser(Long id) {
    User user = userRepository.findById(id).orElseThrow();
    return new UserDTO(user.getId(), user.getUsername(), user.getEmail());
}
```

Cette approche évite de retourner toute l’entité, ce qui aide à garder une API plus propre et plus sûre.[^1][^5]

## Exemple 3 : champ calculé

Un DTO peut aussi contenir un champ qui n’existe pas dans l’entité, par exemple un total calculé.

```java
public class ContractDTO {
    private Long id;
    private String customerName;
    private long totalAmount;

    public ContractDTO(Long id, String customerName, long totalAmount) {
        this.id = id;
        this.customerName = customerName;
        this.totalAmount = totalAmount;
    }

    // getters
}
```

Dans ce cas, le service prépare une vue métier adaptée à l’API, au lieu d’exposer la structure brute de la base.[^7][^1]

## Exemple 4 : validation d’entrée

Les DTO sont aussi pratiques pour valider les données reçues.

```java
public class CreateTodoDTO {
    @NotBlank
    private String title;

    @NotBlank
    private String description;

    // getters et setters
}
```

Puis dans le contrôleur :

```java
@PostMapping("/todos")
public ResponseEntity<Void> create(@Valid @RequestBody CreateTodoDTO dto) {
    todoService.create(dto);
    return ResponseEntity.ok().build();
}
```

Cela permet de vérifier les données avant d’entrer dans la logique métier.[^9][^1]

## Règle simple

Une bonne règle est la suivante :

- **Entity** : pour la base de données.
- **DTO** : pour les échanges API.
- **Service** : pour la logique métier et le mapping entre les deux.[^5][^9][^1]


# Références

