# Design Patterns dans AutoTrade-Engine

Pour garantir une architecture industrielle et maintenable, nous appliquerons plusieurs Design Patterns classiques (GoF). Voici ceux qui seront au cœur du moteur.

## 1. Strategy Pattern (Comportement)
*   **Pourquoi l'utiliser :** Permet de définir une famille d'algorithmes, de les encapsuler et de les rendre interchangeables.
*   **Où l'utiliser :** Pour les stratégies de trading. Le moteur ne doit pas se soucier de savoir si on exécute un croisement de moyennes mobiles ou un RSI.
*   **Exemple :**
    ```java
    public interface TradingStrategy {
        Signal evaluate(MarketSnapshot snapshot);
    }
    public class MovingAverageCrossoverStrategy implements TradingStrategy { ... }
    public class RsiReversionStrategy implements TradingStrategy { ... }
    ```

## 2. Factory / Abstract Factory Pattern (Création)
*   **Pourquoi l'utiliser :** Pour instancier des objets complexes sans exposer la logique de création, surtout lorsque les classes concrètes dépendent d'une configuration.
*   **Où l'utiliser :**
    *   Instanciation dynamique des stratégies à partir d'un fichier de configuration (JSON/YAML).
    *   Création des connexions aux brokers (BinanceAdapter vs KrakenAdapter).
*   **Exemple :** `StrategyFactory.createStrategy(StrategyConfig config)`

## 3. Observer Pattern (Comportement) / Event Bus
*   **Pourquoi l'utiliser :** Pour définir une dépendance un-à-plusieurs afin que, quand l'état d'un objet change, tous les dépendants soient notifiés.
*   **Où l'utiliser :** Le cœur du moteur de trading réactif. Lorsqu'un nouveau `Candle` (bougie) arrive, tous les indicateurs techniques et stratégies abonnées doivent être mis à jour.
*   **Exemple :**
    Nous utiliserons un **Event Bus** (potentiellement via Spring ApplicationEvents ou Reactor pour la performance) au lieu de l'Observer strict, pour découpler totalement les producteurs de données des consommateurs (les stratégies).
    *   Evénements : `CandleClosedEvent`, `OrderFilledEvent`.

## 4. Builder Pattern (Création)
*   **Pourquoi l'utiliser :** Séparer la construction d'un objet complexe de sa représentation, pour créer différentes représentations avec le même processus.
*   **Où l'utiliser :** Pour la création des Ordres de Bourse (`Order`). Un ordre peut avoir de nombreux paramètres optionnels (Take Profit, Stop Loss, Time In Force, Limit Price).
*   **Exemple :**
    ```java
    Order order = Order.builder()
        .symbol("BTC/USDT")
        .type(OrderType.LIMIT)
        .side(OrderSide.BUY)
        .quantity(0.5)
        .price(45000.0)
        .stopLoss(44000.0)
        .build();
    ```

## 5. Adapter Pattern (Structure)
*   **Pourquoi l'utiliser :** Convertir l'interface d'une classe en une autre interface attendue par le client.
*   **Où l'utiliser :** C'est la base des "Ports & Adapters" de notre architecture. Permet de standardiser les réponses des différentes API de cryptomonnaies ou courtiers.
*   **Exemple :**
    Une interface métier `MarketDataPort`. L'implémentation `BinanceMarketDataAdapter` récupère le JSON spécifique de Binance et le convertit en notre objet métier `Candle`.

## 6. Dependency Injection (Inversion of Control)
*   **Pourquoi l'utiliser :** Découpler la création des objets de leur utilisation.
*   **Où l'utiliser :** Partout, géré par le conteneur Spring. Essentiel pour injecter les mocks lors des tests unitaires du domaine.