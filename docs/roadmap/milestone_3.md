# Milestone 3 : Persistance et Statistiques de Performance

## 🎯 Objectif
Sauvegarder les résultats de backtesting (Trades, Portefeuilles) dans une base de données PostgreSQL et calculer des métriques de performance avancées pour juger de la pertinence d'une stratégie.

## 🚀 Fonctionnalités
1.  Intégration de Spring Data JPA et PostgreSQL.
2.  Création des entités JPA (dans la couche Infrastructure) et des Mappers Domain <-> Entity.
3.  Sauvegarde de l'historique des `Trade` et de l'évolution du `Portfolio`.
4.  Moteur de statistiques : Calcul du Win Rate, Profit Factor, Max Drawdown, Ratio de Sharpe.

## 📋 Prérequis
*   Milestone 2 complété.
*   PostgreSQL installé et tournant via Docker.
*   Connaissance des bases de SQL et Hibernate/JPA.

## ⏱️ Temps estimé
1 à 2 semaines.

## 🧠 Connaissances nécessaires
*   Spring Data JPA (Repositories, `@Entity`).
*   Pattern de Mapping (MapStruct ou conversion manuelle) pour ne pas polluer le Domain.
*   Mathématiques financières basiques (Drawdown, Sharpe).
*   Testcontainers (pour les tests d'intégration avec BDD).

## ⚠️ Difficultés possibles
*   **Couplage fort** : Le piège classique est de transformer nos objets du `Domain` (`Trade`, `Portfolio`) en `@Entity`. C'est strictement interdit. Il faut créer des classes spécifiques `TradeEntity` et mapper.
*   **Performance BDD** : Un backtest génère beaucoup de trades. Il faudra peut-être envisager du *batch insert* au lieu d'insérer trade par trade.

## ✅ Critères de validation (Définition du "Terminé")
*   [ ] PostgreSQL est configuré et utilisé.
*   [ ] Une exécution de backtest sauvegarde ses résultats en BDD.
*   [ ] Il est possible de requêter (via un UseCase) les statistiques d'un backtest passé.
*   [ ] Le Max Drawdown et le Win Rate sont calculés avec exactitude (prouvé par des tests).

## 🧪 Tests à écrire
*   Tests des Mappers (Domain <-> Entity).
*   Tests d'intégration des Repositories utilisant **Testcontainers** (une vraie BDD jetable pour les tests).
*   Tests unitaires intensifs sur la classe `PerformanceCalculator`.

## 💡 Conseils du Tech Lead
*   Ne sauvegarde pas chaque `Candle` du backtest en base pour l'instant, c'est trop lourd. Sauvegarde uniquement le résultat (les ordres, les trades et l'évolution du capital).
*   Utilise Flyway ou Liquibase pour versionner les schémas de ta base de données dès le début.