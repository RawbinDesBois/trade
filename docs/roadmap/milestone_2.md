# Milestone 2 : Logique de Stratégie et Moteur d'Ordres

## 🎯 Objectif
Introduire le concept de Stratégie de Trading (Strategy Pattern) et le processus de passage d'ordres virtuels (Paper Trading in-memory). Le système doit être capable de générer des signaux d'achat/vente basés sur des conditions simples et de modifier le solde du portefeuille en conséquence.

## 🚀 Fonctionnalités
1.  Implémentation du **Strategy Pattern** (Interface `TradingStrategy`).
2.  Création de deux stratégies de base (ex: `MovingAverageCrossStrategy`, `DummyRandomStrategy`).
3.  Modèles métier : `Order` (Builder Pattern), `Trade` (exécution de l'ordre), `Signal`.
4.  Moteur d'exécution local (`LocalExecutionAdapter`) : simule un broker (remplit les ordres au prix d'ouverture de la bougie suivante).
5.  Calcul du solde du portefeuille après chaque trade (PnL basique).

## 📋 Prérequis
*   Milestone 1 complété (Moteur capable de lire et diffuser des `Candle`).
*   Intégration potentielle d'une librairie mathématique légère pour les indicateurs (ou implémentation manuelle d'une Moyenne Mobile).

## ⏱️ Temps estimé
2 semaines.

## 🧠 Connaissances nécessaires
*   Design Patterns (Strategy, Builder).
*   Logique de file d'attente (gestion des ordres en attente).
*   Tests paramétrés (pour tester les stratégies avec différentes valeurs).

## ⚠️ Difficultés possibles
*   **Look-ahead bias** (Biais d'anticipation) : C'est l'erreur la plus commune en backtesting. Une stratégie ne doit **jamais** pouvoir prendre une décision basée sur le prix de clôture de la bougie actuelle si l'ordre est exécuté sur cette même bougie.
*   **Synchronisation** : S'assurer que le calcul du portefeuille prend en compte les frais de transaction (fees), même simulés.

## ✅ Critères de validation (Définition du "Terminé")
*   [ ] Une interface `TradingStrategy` est définie et utilisée par le moteur.
*   [ ] Au moins 2 stratégies concrètes sont implémentées et testées unitairement.
*   [ ] Quand une stratégie émet un signal, un `Order` est créé.
*   [ ] L'`Order` est "rempli" (transformé en `Trade`) par le moteur d'exécution en simulant la réalité.
*   [ ] Le solde du portefeuille évolue correctement à la hausse ou à la baisse.

## 🧪 Tests à écrire
*   Tests unitaires des calculs de moyennes mobiles.
*   Test du `LocalExecutionAdapter` : vérifier qu'un ordre d'achat diminue le cash et augmente la position en actif.
*   Test d'intégration : Un backtest complet avec la `MovingAverageCrossStrategy` sur 100 bougies qui produit un historique de trades prédictible.

## 💡 Conseils du Tech Lead
*   Introduis le concept de `MarketSnapshot` : l'objet envoyé à la stratégie qui contient l'historique récent des bougies. La stratégie ne doit pas avoir accès à tout le passé ou au futur, juste à une fenêtre glissante.