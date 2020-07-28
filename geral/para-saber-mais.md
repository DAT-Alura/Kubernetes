# Para saber mais

## Infos com kubectl

O comando onipresente para descobrir algo no seu cluster é o kubectl get <nome-do-recurso>

Um recurso é um pod, deployment, node entre vários outros que veremos ainda.

Por exemplo, você pode listar os nodes do seu cluster usando:

```kubectl get nodes```

Como você está usando Minikube deve aparecer apenas um Master node.

Caso queira entender melhor aquele recurso, use o comando explain:

```kubectl explain node```

Isso também funciona para outros recursos como pod (e outros recursos com deployment ou service):

```kubectl explain pod```

Informações sobre os pods, service, deployments etc recebemos pelo comando kubectl get ..., por exemplo:

``` Kubernetes
kubectl get pods
kubectl get deployments
kubectl get services
```

e

``` Kubernetes
kubectl get pod <nome-do-pod>
kubectl get deployment <nome-do-deployment>
kubectl get service <nome-do-service>
```

Usando o flag '-o wide' recebemos mais infos:

```kubectl get pods -o wide```

Informações mais detalhadas conseguimos pelo comando describe:

``` Kubernetes
## detalhes sobre todos os pods
kubectl describe pods

## detalhes de um pod especifico
kubectl describe pod <nome-do-pod>
```

Por fim, podemos ver os logs de um Pod com o comando:

```kubectl logs <nome-pod-name>```

## Replication Controller

Na documentação oficial, mas também em artigos sobre Kubernetes, você vai ver o uso de um Replication Controller para controlar e garantir a alta disponibilidade dos Pods. Como vimos na aula, isso é a mesma responsabilidade de um Deployment.

Então o que eu devo usar: Deployment ou Replication Controller?

O Deployment substitui o antigo Replication Controller e deve ter preferência no uso. Para ser correto, um Deployment usa um Replica Set por baixo dos panos que substituí o antigo Replication Controller.

Além disso, o Deployment possui vantagens e facilita a atualização (update) e consegue desfazer (rollback) um deploy de Pods, tudo isso de forma declarativa.

Resumindo, use sempre Deployments a favor dos antigos Replication Controller.

## Expose

Como podemos criar deployment sem YML também podemos criar um service na linha de comando. Nesse caso devemos usar o comando expose:

``` Kubernetes
kubectl expose deployment \
  nginx-deploy \
  --name=nginx-service \
  --type=LoadBalancer \
  --port=8080 \
  --target-port=80
```

O comando acima só funciona se existir um deployment com o nome nginx-deploy e vai expor um serviço nginx-service no porta 8080. O target-port é a porta do pod/container.

## Deploy sem YML

Vimos no vídeo como criar um deployment usando o arquivo de configuração YML que vai na linha da Infraestrutura como Código. No exemplo abaixo, usamos a imagem nginx:

``` YML
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  template:
    metadata:
      labels:
        name: nginx
    spec:
      replicas: 3
      containers:
        - name: container-kubernetes
          image: nginx
```

Também é possível criar o deployment a partir da linha de comando, sem a necessidade de definir um arquivo de YML. Nesse caso, precisamos de dois comandos para definir o deployment e 3 replicas:

``` kubectl
kubectl create deployment nginx-deploy --image=nginx
kubectl scale --replicas=3 deployment/nginx-deploy
```

Para deploys em produção, o arquivo de configuração YML é indispensável, pois agrupa as configurações em um único lugar e é possível deployar e atualizar com único comando, além de ser possível versionar no repositório.
