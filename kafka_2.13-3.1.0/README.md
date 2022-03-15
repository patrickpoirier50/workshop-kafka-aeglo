# Workshop Kafka AEGLO

**Prérequis JAVA 8+, Bash Shell**

## Étape 1 - Démarrer l'environnement Kafka

### Ouvrir un terminal Bash

```
cd kafka_2.13-3.1.0

#Démarrer le service ZooKeeper
bin/zookeeper-server-start.sh config/zookeeper.properties
```

### Ouvrir un second terminal

```
cd kafka_2.13-3.1.0

#Démarrer un Kafka broker service
bin/kafka-server-start.sh config/server.properties
```

## Étape 2 - Créer un topic pour ranger les évènements

Ouvrir un 3e terminal

```
cd kafka_2.13-3.1.0

#Afficher les commandes possibles sur un topic
bin/kafka-topics.sh

#Afficher les topics
bin/kafka-topics.sh --bootstrap-server localhost:9092 --list

#Création d'un topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic quickstart-events

#Afficher les détails d'un topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic quickstart-events
```

## Étape 3 - Produire des évènements dans un topic

```
#Démarrer un producteur
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092

#Écrire des messages au niveau du producteur lorsque la connection est établie.
Ceci est un test
Ceci est un autre test
```

## Étape 4 - Lire les évènement

Dans un nouveau terminal

```
#Démarrer un consommateur pour lire les évènements
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```
