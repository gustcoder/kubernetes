![Badge Concluído](http://img.shields.io/static/v1?label=STATUS&message=CONCLUÍDO&color=GREEN&style=for-the-badge)
![Badge Kubernetes](http://img.shields.io/static/v1?label=KUBERNETES&message=V.1.30.0&color=blue&style=for-the-badge)
![Badge PDI](http://img.shields.io/static/v1?label=PDI&message=LOGCOMEX&color=purple&style=for-the-badge)

<h1>Comandos Básicos</h1>

## Aplicando arquivo de config do POD
```kubectl apply -f kubernetes/pod.yaml```

## Sempre que modificar o yaml, necessário fazer o apply novamente


## Listando PODs
```kubectl get pods```

## Listando detalhes de um POD
```kubectl describe pod NAME```

## Redirecionando portas do POD
```kubectl port-forward pod/appname 8000:80```

## Deletando um POD
```kubectl delete pod appname```

# ReplicaSet
## Aplicando arquivo de config do ReplicaSet
```kubectl apply -f kubernetes/replicaset.yaml```

## Listando PODs (após ReplicaSet irá trazer 2 PODs)
```kubectl get pods```

## Listando ReplicaSets
```kubectl get replicasets```

## Neste caso, se um POD for excluído, o ReplicaSet cria outro no lugar

## O "problema" do ReplicaSet é que é preciso apagar todos os PODs existentes para subir uma nova versão de imagem, caso necessário

## Para otimizar isso, usamos o Deployment, onde basta mudar a propriedade "kind" do arquivo yaml. Com isso, quando houver uma modificação de imagem/versão e for feito um apply, os PODs serão terminados e recriados automaticamente

## Com Deployment, os ReplicaSet antigos são mantidos (kubectl get replicasets)

# Fazendo Rollout e Revisões

## Listando histórico de versões
```kubectl rollout history deployment appname```

## Voltando para última versão antes do problema
```kubectl rollout undo deployment appname```

## Voltando para uma versão específica
```kubectl rollout undo deployment appname --to-revision=REVISION```

## Para obter o número da revisão, usar o comando history > REVISION

## Listando detalhes do Deployment
```kubectl describe deployment appname```
