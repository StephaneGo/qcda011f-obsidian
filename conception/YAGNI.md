

Date: 18/06/2026
Tags: 
[[-Principe de conception]]

# Notes



YAGNI signifie "You Aren't Gonna Need It" (tu n'en auras pas besoin). Principe simple en développement logiciel : ne pas implémenter de fonctionnalités ou abstractions tant qu'elles ne sont pas réellement nécessaires. Objectifs et raisons : 

- **Réduire le travail inutile :** éviter de coder des fonctionnalités qui resteraient inutilisées.  
- **Maintenir la simplicité :** moins de code = moins de bugs et de complexité.  
- **Gagner du temps :** concentrer l'effort sur les besoins immédiats prioritaires.  
- **Faciliter l'évolution :** demander d'abord des retours réels ; adapter la conception en fonction de l'usage réel plutôt que d'hypothèses.

Quand l'appliquer (règles pratiques) :
1. Implémente uniquement ce que demande la fonctionnalité actuelle (faire passer les tests, satisfaire l'usage).  
2. Préfère des solutions simples et itératives plutôt que des architectures anticipatoires.  
3. Évite les abstractions générales tant que leur besoin n'est pas prouvé.  
4. Refactorise quand un besoin récurrent apparaît — alors extrais une abstraction correcte.  
5. Utilise tests et feedback pour valider si une extension est justifiée.

Limites et nuances :
- Ne pas confondre YAGNI avec absence d'architecture : certaines contraintes non fonctionnelles (sécurité, performance, conformité) exigent parfois une conception prévue d'avance.  
- Pour du code très critique ou coûteux à changer, planifier peut être nécessaire.  
- YAGNI s'applique surtout aux fonctionnalités et abstractions anticipées, pas aux bonnes pratiques de base (tests, documentation minimale, modularité raisonnable).

Exemple concret (court) : au lieu de créer dès aujourd'hui un système de permissions complexe pour 10 utilisateurs, commence par un contrôle simple (admin/user). Si, plus tard, plusieurs rôles sont demandés, refactorise pour une gestion de rôles générique.

# Références

