# Vision du Projet : AutoTrade-Engine

## 1. Description Générale
AutoTrade-Engine est une plateforme professionnelle de backtesting et de trading automatisé. Conçue pour être robuste, évolutive et performante, elle permet aux utilisateurs de :
- Charger et analyser des données de marché historiques (CSV, puis API temps réel).
- Concevoir et configurer des stratégies de trading algorithmique.
- Simuler l'exécution de ces stratégies (Backtesting) pour évaluer leurs performances.
- Exécuter ces stratégies en mode "Paper Trading" (simulation sur le marché en direct).
- Visualiser les métriques de performance et l'état des portefeuilles.

## 2. Objectifs Techniques et Pédagogiques
Ce projet sert de vitrine de compétences en ingénierie logicielle (niveau Tech Lead). 
L'accent est mis sur :
- **L'excellence architecturale** (Architecture Hexagonale, DDD).
- **La qualité du code** (Clean Code, SOLID, Design Patterns).
- **La testabilité** (TDD, tests unitaires et d'intégration isolés).
- **Les bonnes pratiques industrielles** (Git Flow, CI/CD, Documentation).

## 3. Stack Technique Imposée
- **Langage :** Java 21
- **Framework :** Spring Boot 3.x
- **Gestion de dépendances :** Maven
- **Base de données :** PostgreSQL (évolutif vers TimescaleDB pour les séries temporelles)
- **Communication Temps Réel :** WebSockets
- **Sérialisation :** Jackson (JSON)
- **Déploiement :** Docker & Docker Compose
- **Versioning :** Git

## 4. Évolutions Futures Prévues
Le moteur est pensé dès le jour 1 pour accueillir plus tard :
- L'optimisation automatique des paramètres de stratégie (Algorithmes Génétiques).
- L'intégration de modèles d'Intelligence Artificielle (Deep Learning) pour la prédiction de signaux.
- La connexion à des API de courtiers réels (Binance, Interactive Brokers) pour du Live Trading.
