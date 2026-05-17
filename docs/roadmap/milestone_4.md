# Milestone 4 : Connexion API et Paper Trading Temps Réel

## 🎯 Objectif
Passer du monde simulé (CSV) au monde réel en se connectant à une API d'échange (ex: Binance ou Kraken) via WebSockets pour recevoir les flux de prix en direct et exécuter la stratégie en temps réel (sans vrai argent).

## 🚀 Fonctionnalités
1.  Adaptateur de flux de marché en temps réel (`BinanceWebSocketAdapter`).
2.  Moteur d'Event Bus asynchrone (Spring Events asynchrones ou Reactor) pour gérer la haute fréquence.
3.  Orchestration du mode "Paper Trading" (le portefeuille est en BDD, mais les ordres simulent une exécution sur les prix réels de l'API).
4.  Gestion des déconnexions et reconnexions API.

## 📋 Prérequis
*   Milestone 3 complété.
*   Compréhension des WebSockets en Java.
*   Création d'un compte sur une plateforme (ex: Binance Testnet) pour obtenir une clé API (même juste pour les données publiques).

## ⏱️ Temps estimé
2 à 3 semaines.

## 🧠 Connaissances nécessaires
*   Programmation asynchrone / réactive.
*   WebSockets client (ex: via Spring WebFlux ou un client Tyrus).
*   Gestion des Threads et concurrence (Thread Pools).

## ⚠️ Difficultés possibles
*   **Concurrence** : Deux événements arrivent en même temps, modifient le portefeuille et créent une *Race Condition*. La gestion de l'état du portefeuille doit être thread-safe (ou traitée par un seul thread métier).
*   **Fuite de mémoire** : Garder trop de bougies en mémoire pour les indicateurs sans jamais les nettoyer.
*   **Stabilité réseau** : Les WebSockets se coupent. Il faut gérer des pings réguliers et des reconnexions automatiques.

## ✅ Critères de validation (Définition du "Terminé")
*   [ ] Le système reçoit les prix du BTC/USDT en temps réel sans intervention manuelle.
*   [ ] Une stratégie simple est exécutée sur ces données.
*   [ ] Le portefeuille virtuel évolue en live.
*   [ ] Si on coupe le wifi, l'application le détecte et tente de se reconnecter.

## 🧪 Tests à écrire
*   Test de résilience : mocker une WebSocket qui se ferme aléatoirement et vérifier que le service se reconnecte.
*   Tests de thread-safety sur la modification du `Portfolio`.

## 💡 Conseils du Tech Lead
*   C'est ici que l'Architecture Hexagonale brille. Tu n'as *rien* à changer dans ta logique de Stratégie ou de Portefeuille. Tu ajoutes juste un nouvel Adaptateur `BinanceMarketDataAdapter` qui implémente l'interface `MarketDataPort`. Le domaine n'y verra que du feu.