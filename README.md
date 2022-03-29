# Automação Helm para o AMQ Streams

## Pré-requisitos

### Instalar o Helm

O Helm pode ser baixado e instalado seguindo as intruções em: https://helm.sh/docs/intro/install/

## Instalação do operator

```
helm install -f helm-operator/values.yaml amq-streams helm-operator/
```

### Parâmetros para deploy do Helm

Parâmetro                                      | Descrição
------------------------------                 | -------------------------
`amqStreams.namespace.name`                    | Namespace onde vão ser feitas as implantações
`amqStreams.subscriptions.name`                | Nome da subscription
`amqStreams.subscriptions.channel`             | Canal da subscription
`amqStreams.subscriptions.installPlanApproval` | LInstall plan approval (Automatic ou Manual)
`amqStreams.subscriptions.source`              | Fonte da subscription (Default: redhat-operators)
`amqStreams.subscriptions.sourceNamespace`     | Namespace do operator (Default: openshift-marketplace)
`amqStreams.subscriptions.startingCSV`         | Versão inicial (Default: amqstreams.v2.0.1-1)

## Instalação do AMQ Streams

```
helm install -f helm/examples/values-ephemeral-single.yaml amq-streams helm/
```

### Parâmetros para deploy do Helm

Parâmetro                         | Descrição
------------------------------    | -------------------------
`amqStreams.namespace.name`       | 
`amqStreams.kafka.name`           | Nome do cluster Kafka
`amqStreams.kafka.replicas`       | Quantidade de réplicas do Kafka
`amqStreams.kafka.listeners`      | Listeners Kafka
`amqStreams.kafka.resources`      | Recursos de CPU e memória para o Kafka
`amqStreams.kafka.jvmOptions`     | Opções de JVM para o Kafka
`amqStreams.kafka.config`         | Configurações gerais do Kafka
`amqStreams.kafka.storage`        | Configuraões de storage para o Kafka
`amqStreams.zookeeper.replicas`   | Quantidade de réplicas do Zookeeper
`amqStreams.zookeeper.resources`  | Recursos de CPU e memória para o Zookeeper
`amqStreams.zookeeper.jvmOptions` | Opções de JVM para o Zookeeper
`amqStreams.zookeeper.storage`    | Configuraões de storage para o Zookeeper
`amqStreams.topics`               | Tópicos a serem criados
`amqStreams.topics[*].name`       | Nome do tópico
`amqStreams.topics[*].partitions` | Quantidade de partições do tópico
`amqStreams.topics[*].replicas`   | Quantidade de replicas do tópico