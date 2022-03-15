# Workshop Kafka AEGLO

**Prérequis Docker**

## Étape 1 - Démarrer l'environnement Kafka

### Ouvrir un terminal pour démarrer docker-compose Kafka

```
cd docker
docker-compose up -d
```

### Vérifier si Kafka est bien démarré

```
docker ps
```

## Étape 2 - Créer un topic pour ranger les évènements

```
#Entrer en mode execution du docker containter broker
docker exec -it broker bash

#Afficher les topics
kafka-topics --bootstrap-server localhost:9092 --list

#Création d'un topic
kafka-topics --bootstrap-server localhost:9092 --create --topic quickstart-events

#Afficher les détails d'un topic
kafka-topics --bootstrap-server localhost:9092 --describe --topic quickstart-events
```

## Étape 3 - Produire des évènements dans un topic

```
#Démarrer un producteur
kafka-console-producer --topic quickstart-events --bootstrap-server localhost:9092

#Écrire des messages au niveau du producteur lorsque la connection est établie.
Ceci est un test
Ceci est un autre test
```

## Étape 4 - Lire les évènement

Dans un nouveau terminal (entrer dans le docker containter broker)

```
#Démarrer un consommateur pour lire les évènements
kafka-console-consumer --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```
