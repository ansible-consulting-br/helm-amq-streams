# Automação Helm para o AMQ Streams

## Pré-requisitos

### Instalar o Helm

O Helm pode ser baixado e instalado seguindo as intruções em: https://helm.sh/docs/intro/install/

## Instalação do operator

```
helm install -f helm-operator/values.yaml amq-streams helm-operator/
```

## Instalação do AMQ Streams

```
helm install -f helm/examples/values-ephemeral-single.yaml amq-streams helm/
```
