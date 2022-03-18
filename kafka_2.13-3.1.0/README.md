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

## Étape 5 - Reculer le offset

```
kafka-console-consumer --topic quickstart-events --offset 1  --bootstrap-server localhost:9092 --partition 0
```

## Étape 6 - Topic avec plusieurs partitions et plusieurs consumer

```
#Création d'un topic avec plusieurs partitions
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic quickstart-events-parallelism --partitions 2

#Afficher les détails d'un topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic quickstart-events-parallelism

#Démarrer un consommateur pour lire les évènements
bin/kafka-console-consumer.sh --topic quickstart-events-parallelism --from-beginning --bootstrap-server localhost:9092 --group parallelism-demo

#Afficher les consumer group
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --list

#Afficher les détails d'un consumer group
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --group parallelism-demo --describe

#Partir un 2e consumer dans le même consumer group
bin/kafka-console-consumer.sh --topic quickstart-events-parallelism --from-beginning --bootstrap-server localhost:9092 --group parallelism-demo

#Afficher les détails d'un consumer group pour voir maintenant le rebalancement
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --group parallelism-demo --describe

#Démarrer un producteur
bin/kafka-console-producer.sh --topic quickstart-events-parallelism --bootstrap-server localhost:9092

#Écrire des messages au niveau du producteur lorsque la connection est établie.
Ceci est un test
Ceci est un autre test

#On peut voir que les messages ne sont pas tous envoyés au même consumer
```

## Étape 7 - Topic avec un delete cleanup policy

```
#Repartir le broker avec une configuration pour avoir un cleanup plus rapide des logs dans les topics

#Création d'un topic avec un delete retention policy
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic quickstart-events-delete --config retention.ms=20000 --config cleanup.policy=delete

#Afficher les détails d'un topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic quickstart-events-delete

#Démarrer un producteur
bin/kafka-console-producer.sh --topic quickstart-events-delete --bootstrap-server localhost:9092

#Écrire des messages au niveau du producteur lorsque la connection est établie.
Ceci est un test
Ceci est un autre test

#Démarrer un consommateur pour lire les évènements
bin/kafka-console-consumer.sh --topic quickstart-events-delete --from-beginning --bootstrap-server localhost:9092

#Attendre 30 seconds et redémarrer un consommateur pour lire les évènements
bin/kafka-console-consumer.sh --topic quickstart-events-delete --from-beginning --bootstrap-server localhost:9092
```

## Étape 8 - Topic avec un compact cleanup policy

```
#Création d'un topic avec un compact cleanup policy
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic quickstart-events-compact --config "cleanup.policy=compact" --config "delete.retention.ms=100"  --config "segment.ms=100" --config "min.cleanable.dirty.ratio=0.01"

#Afficher les détails d'un topic
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic quickstart-events-compact

#Démarrer un producteur
bin/kafka-console-producer.sh --topic quickstart-events-compact --bootstrap-server localhost:9092 --property "parse.key=true" --property "key.separator=:"

#Écrire des messages au niveau du producteur lorsque la connection est établie.
emp1:{"name":"John Doe"}
emp2:{"name":"Jane Doe"}
emp1:{"name":"John Doe", "age":44}
emp3:{"name":"John Smith", "age":33}
p1:10$
p2:7$
p1:11$

#Démarrer un consommateur pour lire les évènements
bin/kafka-console-consumer.sh --topic quickstart-events-compact --from-beginning --bootstrap-server localhost:9092 --property  print.key=true --property key.separator=:

#Attendre 30 seconds et redémarrer un consommateur pour lire les évènements
bin/kafka-console-consumer.sh --topic quickstart-events-compact --from-beginning --bootstrap-server localhost:9092 --property  print.key=true --property key.separator=:
```
