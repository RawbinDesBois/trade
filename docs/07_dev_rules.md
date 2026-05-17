# Règles de Développement & Conventions

Ce document dicte les standards de qualité, de nommage et de processus pour AutoTrade-Engine.

## 1. Workflow Git (Git Flow simplifié)
*   **Branche principale** : `main` (code stable, prêt pour la production).
*   **Branche de développement** : `develop` (intégration des nouvelles features).
*   **Branches de fonctionnalités** : `feat/nom-de-la-feature` (ex: `feat/csv-parser`).
*   **Branches de correction** : `fix/nom-du-bug` (ex: `fix/order-status-bug`).
*   **Processus** : Aucun push direct sur `main` ou `develop`. Passage obligatoire par une **Pull Request (PR)**.

## 2. Conventions de Commits (Conventional Commits)
Le format des messages de commit doit suivre le standard suivant :
`<type>: <description courte (en anglais ou français clair)>`

Types autorisés :
*   `feat:` : Nouvelle fonctionnalité.
*   `fix:` : Correction d'un bug.
*   `refactor:` : Modification du code qui n'ajoute pas de feature et ne corrige pas de bug.
*   `test:` : Ajout ou modification de tests manquants.
*   `docs:` : Modification de la documentation seulement.
*   `chore:` : Tâches de maintenance (mise à jour dépendances, build, etc.).

*Exemple :* `feat: ajout du parser CSV pour l'historique des prix`

## 3. Qualité du Code (Clean Code & SOLID)
*   **S**ingle Responsibility Principle : Une classe = une seule responsabilité.
*   **O**pen/Closed Principle : Le code (ex: Moteur de stratégie) doit être ouvert à l'extension (ajouter une nouvelle stratégie) mais fermé à la modification (ne pas modifier le moteur lui-même).
*   **D**ependency Inversion : Dépendre des interfaces, pas des implémentations.
*   **Nommage** :
    *   Classes en `PascalCase`.
    *   Méthodes et variables en `camelCase`.
    *   Soyez descriptif : préférez `calculateMovingAverage()` à `calcMA()`.

## 4. Tests et Couverture
*   **TDD encouragé** : Écrire les tests avant les cas d'utilisation critiques (ex: calcul du PnL).
*   **Couverture minimale** : 80% sur la couche `domain` et `application`.
*   **Frameworks** : JUnit 5 pour l'exécution, Mockito pour les bouchons, AssertJ pour les assertions (plus lisible).
*   Un changement de code qui baisse la couverture de code est refusé.

## 5. Gestion des Exceptions et Logs
*   **Exceptions Métier** : Créer des exceptions spécifiques héritant de `RuntimeException` (ex: `InsufficientFundsException`, `InvalidOrderException`). Pas de catch silencieux.
*   **Logs** : Utiliser SLF4J.
    *   `ERROR` : Exceptions non gérées, crash du moteur, perte de connexion broker.
    *   `WARN` : Comportement inattendu mais géré (ex: retry sur une API).
    *   `INFO` : Démarrage de services, exécution d'un trade, fin d'un backtest.
    *   `DEBUG` : Flux de données, valeurs d'indicateurs (uniquement utile en dev).

## 6. Documentation
*   **JavaDoc** : Obligatoire sur toutes les interfaces publiques (Ports) et les classes majeures du Domaine. Doit décrire *pourquoi* et non *comment*.
*   Les évolutions architecturales doivent être documentées dans les fichiers `docs/`.