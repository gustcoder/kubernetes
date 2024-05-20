![Badge Concluído](http://img.shields.io/static/v1?label=STATUS&message=CONCLUÍDO&color=GREEN&style=for-the-badge)
![Badge Kubernetes](http://img.shields.io/static/v1?label=KUBERNETES&message=V.1.30.0&color=blue&style=for-the-badge)
![Badge FullCycle](http://img.shields.io/static/v1?label=FULLCYCLE&message=3.0&color=orange&style=for-the-badge)
![Badge PDI](http://img.shields.io/static/v1?label=PDI&message=LOGCOMEX&color=purple&style=for-the-badge)

<h1>Comandos Básicos</h1>

## Aplicando arquivo de config do POD
```kubectl apply -f kubernetes/pod.yaml```

![Badge Importante](http://img.shields.io/static/v1?label=IMPORTANTE&message=apply&color=orange&style=for-the-badge)
<br>
Obs.: Sempre que modificar/criar um yaml, necessário fazer o <b>apply</b> dele novamente.


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

<b>Obs.: Neste caso, se um POD for excluído, o ReplicaSet cria outro no lugar.</b>

## O "problema" do ReplicaSet

É preciso apagar todos os PODs existentes para subir uma nova versão de imagem, caso necessário, quando usamos o <b>ReplicaSet</b>
Para otimizar isso, usamos o <b>Deployment</b>, onde basta mudar a propriedade ``kind`` do arquivo yaml. 
Com isso, quando houver uma modificação de imagem/versão e for feito um apply, os PODs serão terminados e recriados automaticamente.

É importante lembrar que, com <b>Deployment</b>, os <b>ReplicaSet</b> antigos são mantidos (``kubectl get replicasets``).

# Fazendo Rollout e Revisões

## Listando histórico de versões
```kubectl rollout history deployment appname```

## Voltando para última versão antes do problema
```kubectl rollout undo deployment appname```

## Voltando para uma versão específica
```kubectl rollout undo deployment appname --to-revision=REVISION```
<b>Obs.: Para obter o número da revisão, usar o comando ``kubectl rollout history`` > REVISION</b>

## Listando detalhes do Deployment
```kubectl describe deployment appname```

<h1>Services</h1>

## Aplicando arquivo de config do service (ClusterIP)
```kubectl apply -f kubernetes/service.yaml```

## Listando services
```kubectl get svc```

## Redirecionando portas do Service
```kubectl port-forward svc/appname-service 8000:80```

<b>Obs.: a diferença deste port forward de service para de pod é que, antes acessávamos direto o POD em si. Já com o service (ClusterIP) seria similar a um LoadBalancer, onde o service irá redirecionar a requisição para os pods/aplicações</b>

<b>Obs.: usar targetPort quando for necessário direcionar para uma porta de um container diferente do que o do service (uma coisa é a porta do service, outra é a do container que roda "dentro" do POD</b>

<h1>Proxy - API Kubernetes</h1>
```kubectl proxy --port=8080```

<b>Ao acessar http://localhost:8080 seremos redicionados para a API do K8s</b>

## Service NodePort
```type: NodePort```

Gera e libera uma determinada porta (ou range de portas) em todos os nodes, bastando saber o endereço do node para acessar o serviço (porém é a forma mais "arcaica").

## Aplicando arquivo de config do service (LoadBalancer)
```kubectl apply -f kubernetes/service_loadbalancer.yaml```

Muito utilizado quando um cluster está sendo gerenciado/conectado diretamente a um <b>servidor em nuvem</b>.

<b>Obs.: É gerado um IP externo e acessível (EXTERNAL-IP).</b>

<h1>ConfigMap</h1>

Recurso utilizado para gerenciar variáveis de ambiente.

<b>Importante!!</b>
Sempre que for modificado, necessário ser feito um apply -f tanto no yaml configmap-env como também no deployment.yaml

<h1>Ingressando dentro do POD</h1>

Similar ao Docker, temos o comando:
```
kubectl exec -it pod-name -- bash
```

<h1>Listando Logs do POD</h1>

```
kubectl logs pod-name
```
<h1>Health Check Probes (Liveness)</h1>

Monitora disponibilidade de aplicações <b>(live)</b>

## Monitorando PODs
```
watch -n1 kubectl get pods
```

<h1>Health Check Probes (Readiness)</h1>

Recurso para só liberar tráfego para a aplicação quando a mesma estiver <b>pronta</b>

![Badge Importante](http://img.shields.io/static/v1?label=IMPORTANTE&message=apply&color=orange&style=for-the-badge)
<br>
Obs.: Liveness e Readiness podem entrar em conflito dependendo das configurações. É necessário avaliar.