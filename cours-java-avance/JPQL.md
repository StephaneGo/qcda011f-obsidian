

Date: 18/06/2026
Tags: 
[[-Spring framework]]
[[-DAL]]

# Notes



## 1) Définition

JPQL signifie **Java Persistence Query Language**. Contrairement au SQL, il interroge les **entités JPA** et non directement les tables de la base de données.[^8][^1]

L’idée clé à retenir est simple : on écrit des requêtes en utilisant le **nom de la classe entité** et les **noms de ses attributs**, pas les noms des tables ni des colonnes.[^1]

## 2) Différences avec SQL

- JPQL travaille sur les objets Java.
- SQL travaille sur les tables et colonnes de la base.
- JPQL est plus portable entre bases de données.
- Pour du SQL natif, il faut passer par `nativeQuery = true`.[^9][^1]

Exemple mental :

- SQL : `SELECT * FROM users`
- JPQL : `SELECT u FROM User u`


## 3) Syntaxe de base

La structure classique d’une requête JPQL est :

```java
SELECT alias
FROM Entite alias
WHERE condition
```

Exemple :

```java
@Query("SELECT e FROM Employee e WHERE e.salary > :salary")
List<Employee> findRichEmployees(@Param("salary") Double salary);
```

Points à retenir :

- `Employee` est le nom de l’entité.
- `salary` est un attribut Java.
- `e` est un alias de l’entité.[^5][^1]


## 4) Avec `@Query`

Dans Spring Data JPA, on utilise souvent JPQL dans le repository avec `@Query`.[^7][^9]

Exemple :

```java
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

    @Query("SELECT e FROM Employee e WHERE e.department = :dept")
    List<Employee> findByDepartment(@Param("dept") String dept);
}
```

Tu peux aussi utiliser la position des paramètres :

```java
@Query("SELECT e FROM Employee e WHERE e.department = ?1")
List<Employee> findByDepartment(String dept);
```


## 5) Paramètres

JPQL supporte les paramètres nommés et positionnels.[^9][^1]

- Paramètre nommé : `:name`
- Paramètre positionnel : `?1`

Le plus lisible en pratique est souvent le **paramètre nommé**, surtout dans les requêtes longues.[^1]

## 6) Requêtes courantes

JPQL permet surtout :

- `SELECT`
- `UPDATE`
- `DELETE`

Exemple `UPDATE` :

```java
@Modifying
@Query("UPDATE Employee e SET e.active = false WHERE e.lastLogin < :date")
int disableInactiveEmployees(@Param("date") LocalDate date);
```

Pour ce type de requête, il faut généralement `@Modifying` et une transaction.[^4][^1]

## 7) Jointures

JPQL gère très bien les relations entre entités.

Exemple :

```java
@Query("SELECT e FROM Employee e JOIN e.department d WHERE d.name = :name")
List<Employee> findEmployeesByDepartmentName(@Param("name") String name);
```

Ici, on joint via la **relation objet** `e.department`, pas via une clé étrangère SQL.[^8][^1]

## 8) Fonctions utiles

JPQL propose des fonctions classiques comme :

- `CONCAT`
- `LENGTH`
- `LOWER`
- `UPPER`
- `BETWEEN`
- `LIKE`

Exemple :

```java
@Query("SELECT e FROM Employee e WHERE LOWER(e.name) LIKE LOWER(CONCAT('%', :name, '%'))")
List<Employee> searchByName(@Param("name") String name);
```


## 9) Ce qu’il faut éviter

- Utiliser des noms de tables au lieu des entités.
- Utiliser des noms de colonnes au lieu des attributs Java.
- Oublier `@Modifying` pour un `UPDATE` ou `DELETE`.
- Mettre du SQL natif alors que JPQL suffit.[^9][^1]

Attention aussi : l’`INSERT` n’est pas l’usage habituel de JPQL dans les exemples Spring Data JPA, et on passe souvent par SQL natif pour ce cas.[^4][^1]

## 10) Bonnes pratiques

- Utilise JPQL pour les requêtes métier lisibles et portables.
- Préfère les paramètres nommés `:param`.
- Garde les requêtes complexes dans le repository, pas dans le service.
- Teste toujours les requêtes sur une vraie base de test.
- Passe en SQL natif seulement si JPQL devient limitant.[^2][^9]


## 11) À retenir

- JPQL = langage de requête sur les **entités**.
- `@Query` permet d’écrire des requêtes JPQL dans Spring Data JPA.
- On utilise les **noms d’entités** et **attributs Java**.
- `@Modifying` est nécessaire pour `UPDATE`/`DELETE`.
- `nativeQuery = true` sert à exécuter du SQL natif.[^7][^1]


# Références

