


Date: 26/06/2026
Tags:  


# Notes

# 📚 Java / Spring Boot - Fiche de révision

---

# 🏛️ Architecture

## Architecture en couches

Une application est découpée en plusieurs couches ayant chacune une responsabilité précise.

```text
Utilisateur
     │
     ▼
Controller (IHM)
     │
     ▼
Service (logique métier)
     │
     ▼
Repository (accès aux données)
     │
     ▼
Base de données
```

### Avantages

- Séparation des responsabilités
- Maintenance facilitée
- Tests simplifiés
- Réutilisation du code

---

## Couplage faible

Limiter les dépendances entre les classes.

👉 On l'obtient principalement grâce aux **interfaces** et à l'injection de dépendances.

---

## SOA

**Service Oriented Architecture**

Une application est découpée en plusieurs services indépendants qui communiquent entre eux.

---

## Interopérabilité

Capacité de plusieurs applications (Java, C#, Python...) à communiquer grâce à des standards communs (HTTP, JSON...).

---

# 🔄 CRUD

| Action | HTTP |
|---------|------|
| Create | POST |
| Read | GET |
| Update | PUT / PATCH |
| Delete | DELETE |

CRUD = Cycle de vie d'une donnée.

---

# 🧪 Tests

## Boîte blanche

Le testeur connaît le code.

### Test unitaire

Teste une seule méthode ou une seule classe.

Méthode **AAA**

```text
Arrange
Act
Assert
```

Utilise souvent des **Mocks**.

---

### Test d'intégration

Teste plusieurs composants ensemble.

Exemple :

Service ↔ Repository ↔ Base de données

---

## Boîte noire

Le testeur ne connaît pas le code.

- Fonctionnels
- Acceptation (recette)
- Manuels
- Exploratoires
- Automatisés

### Gherkin

```gherkin
Given
When
Then
```

---

# 🗄️ JPA

## Définition

JPA est une spécification permettant de faire le lien entre les objets Java et les tables SQL.

Hibernate est son implémentation la plus utilisée.

---

## ORM

Object Relational Mapping

```text
Classe Java
      ⇅
Hibernate
      ⇅
Table SQL
```

---

## Annotations

| Annotation | Rôle |
|------------|------|
| `@Entity` | Entité persistante |
| `@Id` | Clé primaire |
| `@GeneratedValue` | Génération automatique |
| `@Column` | Configuration d'une colonne |
| `@Transient` | Non persisté |

---

## Relations

- `@OneToOne`
- `@OneToMany`
- `@ManyToOne`
- `@ManyToMany`

### Cascade

- ALL
- PERSIST
- REMOVE

### mappedBy

À utiliser dans une relation bidirectionnelle.

Permet d'éviter deux tables d'association.

---

# 📦 JpaRepository

Fournit automatiquement les opérations CRUD.

```java
save()

findById()

findAll()

delete()
```

---

# 🎁 Optional

Conteneur pouvant contenir une valeur... ou être vide.

Évite les `NullPointerException`.

Méthodes utiles :

- `orElse()`
- `orElseThrow()`
- `isPresent()`
- `isEmpty()`

---

# 🌐 API REST

## Principes

✔ Utiliser les verbes HTTP

✔ Utiliser des noms de ressources

✔ Ne jamais mettre de verbe dans l'URL

✔ Utiliser le pluriel

Exemple :

```text
GET    /clients

POST   /clients

PUT    /clients/{id}

PATCH  /clients/{id}

DELETE /clients/{id}
```

---

## Verbes HTTP

| Verbe | Action |
|--------|--------|
| GET | Lire |
| POST | Créer |
| PUT | Modifier complètement |
| PATCH | Modifier partiellement |
| DELETE | Supprimer |

---

## Codes HTTP

| Code | Signification |
|-------|---------------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 500 | Internal Server Error |

---

## ResponseEntity

Permet de définir :

- le code HTTP
- les en-têtes
- le contenu de la réponse

---

## ApiResponse

```json
{
  "status": true,
  "message": "",
  "data": { }
}
```

---

# 🔐 Spring Security

## Authentification

Qui êtes-vous ?

---

## Autorisation

Que pouvez-vous faire ?

---

## RBAC

Role Based Access Control.

Les permissions sont attribuées selon les rôles.

---

## JWT

Jeton signé contenant les informations d'authentification.

Permet une architecture **Stateless**.

---

## Stateful vs Stateless

### Stateful

Le serveur garde la session.

### Stateless

Chaque requête contient les informations nécessaires.

Exemple :

JWT

---

## SSO

Une seule authentification permet d'accéder à plusieurs applications.

---

## IdP

Identity Provider.

Serveur chargé de l'authentification.

---

## CSRF

Attaque utilisant la session d'un utilisateur connecté.

---

## SSRF

Le serveur est forcé à effectuer une requête vers un autre serveur.

---

## HATEOAS

Les réponses REST contiennent des liens vers les ressources disponibles.

---

# 🍃 MongoDB

## NoSQL

Base de données non relationnelle.

---

## MongoDB

Documents BSON.

Pas de jointures.

Schéma flexible.

Document ≤ **16 Mo**

---

## Collection

Équivalent d'une table SQL.

---

## Document

Équivalent d'une ligne SQL.

---

## BSON

Version binaire du JSON.

---

## CQRS

Séparation des lectures et des écritures.

Exemple :

- SQL → écriture
- MongoDB → lecture

---

# ⚙️ Spring

## Bean

Objet géré par Spring.

Annotations :

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`

---

## Injection de dépendances

Spring crée automatiquement les objets nécessaires.

Avantages :

- Couplage faible
- Tests faciles
- Maintenance

---

## IoC

Inversion of Control.

Spring gère le cycle de vie des objets.

---

## DTO

Objet servant à transférer des données.

Il évite d'exposer directement les entités JPA.

---

# 📝 Logger

Ordre des niveaux :

```text
DEBUG
INFO
WARN
ERROR
```

---

# 🎯 À retenir

> [!important]
> **À connaître par cœur :**
>
> - Architecture en couches
> - CRUD
> - Couplage faible
> - JPA
> - JpaRepository
> - Optional
> - DTO
> - ORM / Hibernate
> - REST
> - Verbes HTTP
> - Codes HTTP
> - ResponseEntity
> - Authentification / Autorisation
> - JWT
> - MongoDB


# Références

- Stephane Gobin