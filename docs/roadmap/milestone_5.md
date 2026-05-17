# Milestone 5 : API REST, Dashboard WebSockets et Dockerisation

## 🎯 Objectif
Exposer le moteur via des API standard pour qu'une future interface front-end (React/Angular) puisse le piloter. Packager l'ensemble de l'application pour un déploiement professionnel avec Docker.

## 🚀 Fonctionnalités
1.  Développement des contrôleurs REST (Démarrer/Arrêter un backtest, récupérer l'historique des trades).
2.  Serveur WebSocket sortant : Pousser les mises à jour du portefeuille et les nouveaux trades vers le navigateur web.
3.  Documentation de l'API via Swagger / OpenAPI.
4.  Création des `Dockerfile` et `docker-compose.yml` finaux.

## 📋 Prérequis
*   Milestone 4 complété.
*   Connaissances Spring Web (REST, `@RestController`).
*   Bases de Docker.

## ⏱️ Temps estimé
2 semaines.

## 🧠 Connaissances nécessaires
*   Conception d'API RESTful.
*   Spring WebSocket (STOMP / SockJS).
*   Docker multi-stage builds (pour optimiser l'image Java).

## ⚠️ Difficultés possibles
*   **Sécurité basique** : Exposer des API de trading sans sécurité est dangereux. Même si c'est du paper trading, mettre en place une simple API Key ou Basic Auth sur les endpoints REST.
*   **Mapping DTOs** : Ne jamais renvoyer les objets du Domaine en JSON. Toujours utiliser des objets de réponse (DTOs).

## ✅ Critères de validation (Définition du "Terminé")
*   [ ] On peut lancer un backtest via une requête `POST /api/backtests` avec une configuration JSON.
*   [ ] Swagger UI est disponible sur `http://localhost:8080/swagger-ui.html`.
*   [ ] Un client WebSocket externe (ex: Postman ou un script JS) peut s'abonner aux événements du compte.
*   [ ] Un simple `docker-compose up` démarre la base de données et l'application prêtes à l'emploi.

## 🧪 Tests à écrire
*   Tests d'intégration des Controllers REST via `@WebMvcTest` (sans charger toute l'application).
*   Vérification des codes de retour HTTP (200 OK, 400 Bad Request si conf invalide).

## 💡 Conseils du Tech Lead
*   Pour Docker, utilise une image "Distroless" ou Alpine avec le JRE 21 pour garder un poids minimal.
*   Félicitations, à la fin de cette étape, tu auras un produit complet, démontrable en entretien, et technologiquement très avancé !