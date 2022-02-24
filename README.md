# Workshop Kafka AEGLO

**Prérequis JAVA 8+, Bash Shell**

## Étape 1 - Démarrer l'environnement Kafka

Se positionner dans le répertoire bin de kafka

Démarrer le service ZooKeeper (Terminal #1)

```
zookeeper-server-start.sh config/zookeeper.properties
```

Démarrer Kafka broker service (Terminal #2)

```
kafka-server-start.sh config/server.properties
```

## Étape 2 - Créer un topic pour ranger les évènements

Création d'un topic (Terminal #3)

```
kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```
