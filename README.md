# AutoTrade-Engine 📈🤖

```text
    ___         __        _____               __           ______               _          
   /   | __  __/ /_____  /_  __/________     / /_  ___    / ____/___  ____ _(_)___  ___ 
  / /| |/ / / / __/ __ \  / / / ___/ __ `/  / __ \/ _ \  / __/ / __ \/ __ `/ / __ \/ _ \
 / ___ / /_/ / /_/ /_/ / / / / /  / /_/ /  / /_/ /  __/ / /___/ / / / /_/ / / / / /  __/
/_/  |_\__,_/\__/\____/ /_/ /_/   \__,_/  /_.___/\___/ /_____/_/ /_/\__, /_/_/ /_/\___/ 
                                                                   /____/               
```

[![Java 21](https://img.shields.io/badge/Java-21-orange.svg)](https://java.oracle.com/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2+-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-blue.svg)](https://www.postgresql.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**AutoTrade-Engine** est une plateforme de backtesting et de trading automatisé conçue selon les standards industriels. Elle permet de charger des données historiques, de concevoir et d'évaluer des stratégies d'investissement, et de les exécuter en mode Paper Trading.

## 🎯 Vision du Projet
Ce projet est développé avec une approche de **qualité logicielle absolue** : Architecture Hexagonale, Domain-Driven Design (DDD), Design Patterns éprouvés, et une couverture de tests exhaustive. Il a pour but de fournir une fondation solide, maintenable et évolutive pour des systèmes de trading complexes, et sert de portfolio technique de haut niveau.

## ✨ Fonctionnalités Principales
*   **Backtesting Engine** : Simulation de stratégies sur des données historiques (CSV).
*   **Paper Trading** : Exécution de stratégies en temps réel avec un portefeuille virtuel.
*   **Strategy Management** : Création flexible de stratégies via une architecture modulaire.
*   **Market Data Ingestion** : Normalisation des données de marché (Candles, Ticks).
*   **Performance Analytics** : Calcul des métriques de performance (Win Rate, Drawdown, PnL).
*   **WebSocket API** : Diffusion des données de marché et de l'état du portefeuille en temps réel.

## 🏗️ Stack Technique
*   **Backend** : Java 21, Spring Boot 3
*   **Build** : Maven
*   **Base de données** : PostgreSQL (prêt pour TimescaleDB pour les time-series)
*   **Communication** : WebSockets, Jackson, Spring Events
*   **Déploiement** : Docker, Docker Compose
*   **Contrôle de version** : Git

## 📐 Architecture
Le projet repose sur une **Architecture Hexagonale (Ports & Adapters)** couplée à du **Domain-Driven Design (DDD)**.
*   **Domain** : Cœur métier isolé (Stratégies, Ordres, Portefeuille, Market Data). Zéro dépendance externe.
*   **Application** : Cas d'utilisation (Orchestration du backtest, passage d'ordre).
*   **Infrastructure** : Adaptateurs de sortie (Base de données, API externes, File System).
*   **Interfaces (Controllers/WebSockets)** : Adaptateurs d'entrée (API REST, WS).

## 📂 Arborescence du Projet
```text
AutoTrade-Engine/
├── backend/                  # Application Java Spring Boot
│   ├── src/main/java/com/autotrade/
│   │   ├── domain/           # Entités métier, Value Objects, Ports (Interfaces)
│   │   ├── application/      # Use Cases, Services applicatifs
│   │   ├── infrastructure/   # Adapters BDD, Clients API, Implémentations des Ports
│   │   └── interfaces/       # Controllers REST, WebSockets, Schedulers
│   └── pom.xml
├── frontend/                 # (Futur) Interface utilisateur
├── docs/                     # Documentation technique détaillée
├── scripts/                  # Scripts utilitaires (migration, data fetch)
└── docker/                   # Fichiers Docker Compose, configs BDD
```

## 🚀 Installation Locale

### Prérequis
*   JDK 21
*   Maven 3.9+
*   Docker & Docker Compose

### Lancement avec Docker
```bash
# Démarrer la base de données PostgreSQL
docker-compose -f docker/docker-compose.yml up -d db

# Configurer les variables d'environnement (.env)
cp .env.example .env

# Compiler et lancer l'application
cd backend
mvn clean install
mvn spring-boot:run
```

## 🧠 Fonctionnement du Moteur
Le moteur utilise un **Event Bus** interne. Les données de marché (`TickEvent`, `CandleEvent`) sont publiées sur le bus. Les stratégies (Pattern *Observer*) écoutent ces événements, analysent les indicateurs, et génèrent des `SignalEvent` (Achat/Vente). Le moteur d'exécution valide ces signaux avec le portefeuille et passe les `OrderEvent`.

## 🛡️ Sécurité & Qualité
*   **Tests** : Couverture minimale visée de 80% (JUnit 5, Mockito).
*   **Règles** : Respect strict des principes SOLID et du Clean Code.
*   **Validation** : Analyse statique de code prévue dans la CI/CD.

---
*Projet développé en tant que portfolio d'ingénierie logicielle.*