# ServeurJava — Passerelle MQTT → PostgreSQL pour une station météo

Serveur Java qui **reçoit les mesures d'une station météo via MQTT** (température et humidité au format JSON) et les **enregistre dans une base PostgreSQL**. Ces données sont ensuite affichées par le site web du projet [ServeurJavaDjango](https://github.com/EdNganji/ServeurJavaDjango).

## Architecture

```
Station / simulateur ──MQTT──▶ Broker (test.mosquitto.org) ──▶ ServeurJava ──JDBC──▶ PostgreSQL (meteo)
                                                                                  │
                                                                     Site Django ◀┘
```

## Fonctionnement

- `Serveur` démarre l'abonnement MQTT.
- `Suscribe` s'abonne au topic du broker et reçoit chaque message.
- `Convert` transforme le message JSON (ex. `{"Temperature": 27.3, "Humidite": 25}`) en objet `Capteur` grâce à **Gson**.
- `Sender` insère la mesure dans la table `bathurst_capteurs` de la base `meteo`.
- `PahoTest001Producer` simule une station en publiant des valeurs aléatoires (température entre 26 et 28 °C, humidité entre 20 et 30 %).

> `AssurancePROGVIE.java` et `Traitement.java` sont un exercice Java indépendant (calcul de primes d'assurance).

## Technologies

- Java
- [Eclipse Paho](https://eclipse.dev/paho/) (client MQTT)
- Gson (JSON)
- PostgreSQL + JDBC

## Prérequis

- Java 17 ou plus récent
- PostgreSQL avec une base `meteo` (la table est créée par les migrations du projet Django)
- Bibliothèques : `org.eclipse.paho.client.mqttv3`, `gson`, pilote JDBC PostgreSQL

## Lancer le projet

1. Adapter les informations de connexion à la base dans `src/Sender.java`.
2. Lancer `Serveur.java` (réception et enregistrement).
3. Dans un autre terminal, lancer `PahoTest001Producer.java` pour envoyer des données de test.

## Auteur

**Daniel Nganjip** — projet réalisé en 2023.
