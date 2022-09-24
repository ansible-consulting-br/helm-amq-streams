# Automação Helm para o AMQ Streams

## Pré-requisitos

### Instalar o Helm

O Helm pode ser baixado e instalado seguindo as intruções em: https://helm.sh/docs/intro/install/

## Instalação do operator

```bash
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

## Instalação do AMQ Streams


### Estrutura do projeto

```bash
.
├── Chart.yaml
├── examples
│   ├── kafka
│   │   ├── values-auth-keycloak.yaml
│   │   ├── values-auth-scram-sha-512.yaml
│   │   ├── values-auth-tls.yaml
│   │   ├── values-ephemeral-single.yaml
│   │   ├── values-ephemeral.yaml
│   │   ├── values-persistent-single.yaml
│   │   └── values-persistent.yaml
│   └── users
│       ├── values-users-auth-scram-sha-512.yaml
│       └── values-users-auth-tls.yaml
├── templates
│   ├── 1-kafka.yaml
│   ├── 2-kafka-topics.yaml
│   ├── 3-kafka-users.yaml
│   └── 4-kafka-metrics-config.yaml
├── values-topics.yaml
├── values-users.yaml
└── values.yaml
```

### Instalando o Kafka


#### Instalação básica

A instalação básica leva em consideração apenas o `values.yaml` com um cluster efêmero (sem persistência), sem usuários e tópicos.

```bash
helm install amq-streams helm/
```

É possível compor values para instalar o kafka com diferentes configurações, por exemplo:

```bash
helm install amq-streams --values=helm/values-users.yaml --values=helm/values-topics.yaml helm/
```

#### Instalações avançadas

Compondo arquivos de values para adicionar/substituir configurações, podemos instalar o Kafka com as configurações:

##### AMQ Streams persistente

```bash
helm install amq-streams --values=helm/examples/kafka/values-persistent.yaml --values=helm/values-users.yaml --values=helm/values-topics.yaml helm/
```

##### AMQ Streams persistente com autenticação TLS

```bash
helm install amq-streams --values=helm/examples/kafka/values-auth-tls.yaml --values=helm/examples/values-users-auth-tls.yaml --values=helm/values-topics.yaml helm/
```

##### AMQ Streams persistente com autenticação Scram-sha-512

```bash
helm install amq-streams --values=helm/examples/kafka/values-auth-scram-sha-512.yaml --values=helm/examples/values-users-auth-scram-sha-512.yaml --values=helm/values-topics.yaml helm/
```

##### AMQ Streams persistente com autenticação RH-SSO (Keycloak)

Nessa configuração, não é preciso criar usuários.

```bash
helm install amq-streams --values=helm/examples/kafka/values-auth-keycloak.yaml --values=helm/values-topics.yaml helm/
```

**OBS:** Um exemplo de configuração do RH-SSO (Keycloak) pode ser encontrado no artefato *Red Hat AMQ Streams 2.X OpenShift Installation and Example Files*, na página de downloads da Red Hat (https://access.redhat.com/jbossnetwork/restricted/listSoftware.html?downloadType=distributions&product=jboss.amq.streams&productChanged=yes).

### Parâmetros para deploy do Helm

Parâmetro                             | Descrição
------------------------------        | --------------------------------------------
`amqStreams.kafka.name`               | Nome do cluster Kafka
`amqStreams.kafka.replicas`           | Quantidade de réplicas do Kafka
`amqStreams.kafka.version`            | Versão do Kafka
`amqStreams.kafka.listeners`          | Listeners Kafka
`amqStreams.kafka.authorization`      | Configuração de autorização para o cluster kafka 
`amqStreams.kafka.resources`          | Recursos de CPU e memória para o Kafka
`amqStreams.kafka.jvmOptions`         | Opções de JVM para o Kafka
`amqStreams.kafka.readinessProbe`     | Configuração de readinessProbes para o cluster kafka 
`amqStreams.kafka.livenessProbe`      | Configuração de livenessProbes para o cluster kafka 
`amqStreams.kafka.logging`            | Configurações logs do Kafka
`amqStreams.kafka.config`             | Configurações gerais do Kafka
`amqStreams.kafka.storage`            | Configuraões de storage para o Kafka
`amqStreams.kafka.metricsConfig`      | Configurações de métricas do Kafka
`amqStreams.zookeeper.replicas`       | Quantidade de réplicas do Zookeeper
`amqStreams.zookeeper.resources`      | Recursos de CPU e memória para o Zookeeper
`amqStreams.zookeeper.jvmOptions`     | Opções de JVM para o Zookeeper
`amqStreams.zookeeper.readinessProbe` | Configuração de readinessProbes para o Zookeeper
`amqStreams.zookeeper.livenessProbe`  | Configuração de livenessProbes para o Zookeeper
`amqStreams.zookeeper.storage`        | Configuraões de storage para o Zookeeper
`amqStreams.zookeeper.metricsConfig`  | Configurações de métricas para o Zookeeper
`amqStreams.topics`                   | Tópicos a serem criados
`amqStreams.topics[*].name`           | Nome do tópico
`amqStreams.topics[*].partitions`     | Quantidade de partições do tópico
`amqStreams.topics[*].replicas`       | Quantidade de replicas do tópico
`amqStreams.topics[*].config`         | Configurações gerais do tópico
`amqStreams.users`                    | Usuários a serem criados
`amqStreams.users[*].name`            | Nome do usuário
`amqStreams.users[*].authentication`  | Configurações de autenticação para o usuário
`amqStreams.users[*].authorization`   | Configurações de autorização para o usuário

