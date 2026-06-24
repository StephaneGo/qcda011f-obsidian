

Date: 24/06/2026
Tags: #HTTP #Web #API #Rest

	HTTP définit un ensemble de **méthodes de requête** pour indiquer le but de la requête et ce qui est attendu en cas de succès. Bien qu'elles puissent aussi être des noms, ces méthodes de requête sont parfois appelées _verbes HTTP_. ^[1]
## Les verbes HTTP

Les principaux verbes HTTP utilisés dans une API REST sont :

- GET : récupérer une ou plusieurs ressources.
    
- POST : créer une nouvelle ressource.
    
- PUT : mettre à jour complètement une ressource existante.
    
- PATCH : mettre à jour partiellement une ressource.
    
- DELETE : supprimer une ressource.
    

Chaque verbe doit être utilisé de manière cohérente avec son intention. Par exemple, une création ne doit jamais être faite avec GET, car ce verbe doit rester idempotent et sans effet de bord.

## Les statuts HTTP

Les statuts HTTP permettent d’indiquer le résultat d’une requête. Ils sont essentiels pour que le client comprenne si l’opération a réussi ou échoué.

Les grandes catégories sont :

- 2xx : succès (ex : 200 OK, 201 Created).
    
- 4xx : erreur côté client (ex : 400 Bad Request, 404 Not Found).
    
- 5xx : erreur côté serveur (ex : 500 Internal Server Error).
    

Une bonne pratique consiste à toujours retourner un code HTTP adapté à la situation. Par exemple :

- 201 pour une création réussie.
    
- 404 si une ressource est introuvable.
    
- 400 si la requête est invalide.

## Organisation des contrôleurs

Une autre bonne pratique essentielle est d’organiser l’API par domaine métier.

Cela signifie créer un RestController par entité ou domaine fonctionnel, par exemple :

- UserController pour les utilisateurs.
    
- ProductController pour les produits.
    
- OrderController pour les commandes.
    

Cette organisation permet :

- Une meilleure lisibilité du code.
    
- Une séparation claire des responsabilités.
    
- Une évolutivité plus simple du projet.

## Bonnes pratiques pour formater une réponse API (APIResponse)

Au-delà du statut HTTP, il est recommandé de standardiser le format des réponses retournées par l’API. Cela facilite la compréhension, la maintenance et l’intégration côté client.

Une réponse API cohérente doit contenir :

- Un statut (succès ou échec).
    
- Un message explicatif.
    
- Les données (si disponibles).

###### Exemple:

`class APIResponse<T> {`  
`private String status;`  
`private String message;`  
`private T data;`  
`}`

### ResponseEntity vs retour direct

 Utiliser `ResponseEntity` :   

- Permet de contrôler le statut HTTP (200, 201, 204, 400, etc.).
    
- Permet aussi de personnaliser les headers et le corps de la réponse.
    
- Recommandé pour une API robuste.
    

Exemple :

- `ResponseEntity.ok(data)` → 200 OK
    
- `ResponseEntity.noContent()` → 204 No Content
    
- `ResponseEntity.badRequest()` → 400 Bad Request
# Notes

## Bonnes pratiques supplémentaires APIResponse<T>

- Toujours fournir un message clair et utile, surtout en cas d’erreur.
    
- Éviter d’exposer des détails techniques sensibles dans les messages.
    
- Garder une structure cohérente entre toutes les routes de l’API.
    
- Utiliser des noms de champs explicites (status, message, data).


# Références

[^1]: https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Methods
