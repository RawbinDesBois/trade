# Milestone 1 : Fondations et Backtesting Simple (CSV)

## 🎯 Objectif
Poser l'architecture de base du projet (Hexagonale), implémenter le cœur du domaine de marché (Modèle de données) et créer un premier moteur capable de lire un historique de prix depuis un fichier CSV pour simuler une exécution (sans logique de stratégie complexe pour l'instant).

## 🚀 Fonctionnalités
1.  Structure des packages (Domain, Application, Infrastructure).
2.  Modèles métier de base : `Candle` (OHLCV - Open, High, Low, Close, Volume), `Tick`, `Symbol`.
3.  Port et Adaptateur d'ingestion de données (CSV parser).
4.  Moteur d'événements synchrone simple (Event Bus minimaliste pour diffuser les bougies lues).
5.  Portefeuille (Portfolio) basique pour tracer un solde initial.

## 📋 Prérequis
*   Structure du projet Spring Boot générée.
*   Configuration de Maven (dépendances JUnit, Lombok potentiellement, OpenCSV ou Jackson pour le CSV).
*   Fichier CSV de données historiques de cryptomonnaies (ex: BTC/USDT en 1h) pour les tests.

## ⏱️ Temps estimé
1 à 2 semaines (selon le rythme d'apprentissage des concepts d'architecture).

## 🧠 Connaissances nécessaires
*   Architecture Hexagonale (théorie).
*   Manipulation de fichiers en Java (NIO ou librairie externe).
*   Gestion du temps et des dates en Java (`java.time.ZonedDateTime` impératif pour la gestion des fuseaux horaires du marché).
*   JUnit 5 pour les tests unitaires.

## ⚠️ Difficultés possibles
*   **Gestion des dates** : Les formats CSV varient énormément (timestamps unix vs strings ISO).
*   **Tentative de sur-ingénierie** : Vouloir mettre une base de données dès le début. *Ici, on s'en tient au CSV en mémoire.*
*   **Fuite du domaine** : Mettre des annotations CSV directement sur l'entité `Candle` du domaine. (Solution : Créer un objet DTO spécifique dans l'infrastructure).

## ✅ Critères de validation (Définition du "Terminé")
*   [ ] Un fichier CSV de 1000 lignes peut être chargé et parsé en une liste de `Candle` valides.
*   [ ] Le domaine `Candle` est pur (aucune annotation externe).
*   [ ] Une classe `BacktestEngine` (UseCase) peut boucler sur ces bougies et les afficher/publier séquentiellement.
*   [ ] Les tests unitaires valident le parsing correct (vérification des valeurs Open, Close, etc.).
*   [ ] Aucun couplage entre l'infrastructure de parsing et le moteur de backtest (utilisation des Ports).

## 🧪 Tests à écrire
*   Test unitaire du parser CSV avec des données correctes.
*   Test unitaire du parser CSV avec des données corrompues (validation des exceptions métier).
*   Test du `BacktestEngine` avec un Mock du port d'ingestion.

## 💡 Conseils du Tech Lead
*   **N'utilise pas les `Date` classiques de Java**. Utilise exclusivement `Instant` ou `ZonedDateTime`. En finance, l'horodatage précis au millième de seconde est critique.
*   Sépare strictement le `CsvCandleDto` (qui sert à lire le fichier) de ton entité métier `Candle`. C'est l'adaptateur qui fera le mapping. C'est l'essence même de l'architecture hexagonale.