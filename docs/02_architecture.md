# Architecture : Hexagonale & Domain-Driven Design (DDD)

## 1. Choix Architectural
Pour **AutoTrade-Engine**, l'architecture retenue est l'**Architecture Hexagonale** (ou Ports et Adaptateurs), fortement inspirée par les principes du **Domain-Driven Design (DDD)**.

### Pourquoi ce choix ?
Le domaine du trading algorithmique est extrêmement complexe et critique. La logique de calcul des indicateurs, les règles de gestion du risque et les stratégies de prise de position constituent la valeur réelle du logiciel.
*   **Isolation du métier** : Le code métier (Domain) ne doit dépendre d'aucun framework technique (pas de dépendance Spring, pas de dépendance Base de Données dans le coeur métier).
*   **Testabilité** : Un backtest n'est finalement qu'un run du moteur avec des adaptateurs bouchonnés (fichiers CSV au lieu d'une API de broker). L'architecture hexagonale permet de substituer ces adaptateurs instantanément.
*   **Évolutivité** : Passer d'un stockage CSV à PostgreSQL, ou d'une API Binance à une API Kraken, ne nécessitera aucune modification du code métier.

### Pourquoi écarter les autres ?
*   *Clean Architecture* : Très similaire à l'Hexagonale. L'Hexagonale met cependant un accent plus explicite sur les "Ports" (Interfaces) de communication vers l'extérieur, ce qui est parfait pour nos flux de données (Data Feeds) et d'exécution (Brokers).
*   *Architecture N-Tiers / Modulaire standard* : Trop couplée aux bases de données. La logique métier finit inévitablement diluée dans les services de données.
*   *Event-Driven Architecture (EDA)* : Nous l'utiliserons *à l'intérieur* de l'Hexagone (Event Bus pour le flux de ticks), mais en tant que pattern de communication, pas comme architecture globale du monolithe.

## 2. Définition des Couches et Responsabilités

### 2.1. Couche `domain` (Le Cœur / L'Hexagone)
*   **Responsabilité** : Contient les règles métier pures, les entités, les Value Objects.
*   **Composants** : `Candle`, `Trade`, `Strategy`, `Order`, `Portfolio`.
*   **Ports** : Contient les *Interfaces* (Ports) que le domaine utilise pour communiquer avec l'extérieur (ex: `MarketDataPort`, `OrderExecutionPort`, `PortfolioRepository`).
*   **Règle stricte** : Aucune annotation Spring, Jackson ou JPA ici. Uniquement du Java pur.

### 2.2. Couche `application` (Les Cas d'Utilisation)
*   **Responsabilité** : Orchestre les appels entre le domaine et l'infrastructure pour réaliser une action utilisateur spécifique.
*   **Composants** : `RunBacktestUseCase`, `ConnectBrokerUseCase`, `GetPortfolioStatsUseCase`.
*   **Règle stricte** : Ne contient pas de logique de trading. Se contente de récupérer des données, d'appeler les méthodes du domaine, et de sauvegarder le résultat.

### 2.3. Couche `infrastructure` (Les Adaptateurs de Sortie)
*   **Responsabilité** : Implémente les Ports définis par le domaine. Gère la technique pure.
*   **Composants** :
    *   `PostgresPortfolioRepository` (implémente `PortfolioRepository` via Spring Data JPA).
    *   `CsvMarketDataAdapter` (implémente `MarketDataPort`).
    *   `BinanceExecutionAdapter` (implémente `OrderExecutionPort`).

### 2.4. Couche `interfaces` (Les Adaptateurs d'Entrée)
*   **Responsabilité** : Les points d'entrée de l'application (ceux qui déclenchent les Use Cases).
*   **Composants** : `BacktestController` (REST API), `MarketDataWebSocketHandler`, CLI commands.

## 3. Flux de Dépendance
La règle de dépendance est stricte et va toujours vers l'intérieur :
`Interfaces` -> `Application` -> `Domain` <- `Infrastructure`

*L'infrastructure dépend du domaine (elle implémente ses interfaces), le domaine ne dépend de RIEN.*