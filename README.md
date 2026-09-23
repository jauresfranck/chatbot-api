
Le bot fonctionne selon deux canaux indépendants :
1. **Conversationnel** — `ListenerUpdateTelegram` interroge en continu l'API Telegram
   (`getUpdates`) et répond directement dans le chat selon le texte reçu.
2. **REST** — les contrôleurs exposent les mêmes fonctionnalités comme une API classique,
   pour un usage externe (tests, intégration, scripts).

## Stack technique

| Domaine | Technologies |
|---|---|
| Langage / framework | Java 8, Spring Boot 2.4.5 (Spring Web) |
| Documentation API | springdoc-openapi-ui (Swagger UI) |
| Utilitaires | Lombok, Jackson |
| API externes | API Bot Telegram, OpenWeatherMap |
| Build | Maven |

## Installation

Prérequis : JDK 8+, Maven.

```bash
git clone https://github.com/jauresfranck/chatbot-api.git
cd chatbot-api
```

## Configuration

Le bot a besoin de deux clés, fournies **uniquement** via des variables d'environnement
(jamais en dur dans le code ni commitées) :

| Variable | Description | Où l'obtenir |
|---|---|---|
| `TELEGRAM_BOT_ID` | Token du bot Telegram | [@BotFather](https://t.me/BotFather) sur Telegram |
| `OPENWEATHER_API_TOKEN` | Clé API météo | [openweathermap.org](https://openweathermap.org/api) |

```bash
export TELEGRAM_BOT_ID=ton_token_de_bot
export OPENWEATHER_API_TOKEN=ta_cle_api
```

## Lancer le projet

```bash
mvn spring-boot:run
```

L'application démarre sur `http://localhost:9090`.

## Commandes Telegram

Une fois le bot lancé, envoie-lui un message dans Telegram :

| Message | Réponse |
|---|---|
| `meteo Paris` | Météo actuelle de la ville |
| `forecast Paris` | Prévisions sur plusieurs jours |
| `blague` | Une blague aléatoire |
| `bonne blague` | Une blague notée ≥ 7/10 |
| `mauvaise blague` | Une blague notée < 7/10 |
| autre message | Liste des commandes disponibles |

## API REST

Documentation interactive une fois l'application lancée :
`http://localhost:9090/swagger-ui.html`

| Méthode | Endpoint | Description |
|---|---|---|
| GET | `/weather?city={ville}` | Météo du jour |
| GET | `/weather/forecast?city={ville}` | Prévisions à plusieurs jours |
| GET | `/jokes` | Toutes les blagues |
| GET | `/jokes/random` | Blague aléatoire |
| GET | `/jokes/best` | Meilleure blague (note ≥ 7) |
| GET | `/jokes/worst` | Pire blague (note < 7) |
| GET | `/jokes/{id}` | Blague par identifiant |
| POST | `/jokes` | Ajouter une blague |
| PUT | `/jokes/{id}` | Modifier une blague |
| DELETE | `/jokes/{id}` | Supprimer une blague |
| POST | `/bot/meteo` | Envoyer la météo directement sur Telegram |
| POST | `/bot/forecast` | Envoyer les prévisions directement sur Telegram |
| POST | `/message` | Envoyer un message libre sur Telegram |

## Structure du projet
