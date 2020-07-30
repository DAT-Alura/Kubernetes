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

## Juntando configs

No vídeo, você viu que deployamos a aplicação criando vários arquivos YML, cada definindo o seu recurso e configuração. Por exemplo, para expor o serviço aplicação sistema criamos 4 arquivos YML.

Isso pode ficar confuso na medida que a quantidade de arquivos cresce, e por isso é possível juntá-los. Podemos juntar, por exemplo, todas as configurações relacionadas com o sistema:

``` yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: permissao-imagens
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: permissao-sessao
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
---

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: aplicacao-sistema-statefulset
spec:
  serviceName: aplicacao-sistema-statefulset
  selector:
    matchLabels:
      name: aplicacao-sistema-pod-statefulset
  template:
    metadata:
      labels:
        name: aplicacao-sistema-pod-statefulset
    spec:
      containers:
        - name: container-aplicacao-sistema-statefulset
          image: jnlucas/noticia-alura:v2
          ports:
            - containerPort: 80
          volumeMounts:
            - name: imagens
              mountPath: /var/www/html/uploads
            - name: sessoes
              mountPath: /tmp
      volumes:
        - name: imagens
          persistentVolumeClaim:
            claimName: permissao-imagens
        - name: sessoes
          persistentVolumeClaim:
            claimName: permissao-sessao

---

apiVersion: v1
kind: Service
metadata:
  name: servico-aplicacao-sistema-statefulset
spec:
  type: LoadBalancer
  ports:
    - name: http
      port: 80
      nodePort: 31822
  selector:
    name: aplicacao-sistema-pod-statefulset
```

Salvando as configurações em um arquivo deploy-sistema.yml podemos aplicá-lo pelo comando:

```kubectl create -f deploy-sistema.yml```

## kubectl edit

Nos bastidores, tudo é possível definir e controlar através do arquivo YAML. Isso fornece maior controle e é mais fácil de fazer alterações e atualizar o cluster.

As definições de um objeto do Kubernetes já carregados podem ser editadas ao vivo pelo kubectl usando:

```kubectl edit deployment aplicacao-noticia-deployment```

Isso abrirá a configuração do deployment no formato YAML usando VIM. Nessa configuração aparecem todos os dados desse objeto, também os valores padrões que não foram definidos explicitamente.

Ao salvar e sair, as alterações solicitadas serão aplicadas.

## Docker Swarm

Aprendemos que o Kubernetes usa a Docker Engine para rodar os containers dentro do cluster. Isso não é obrigatório mas é o mais comum. Ou seja, já que usamos Docker para rodar os containers, será que existe uma solução do Docker mesmo para gerenciar e orquestrar containers? Um cluster nativo do Docker?

Existe sim e se chama Docker Swarm. Ou seja, Docker Swarm e Kubernetes são alternativas ou concorrentes, ambos são orquestradores de container.

Kubernetes vs Docker Swarm
A comparação entre eles é algo complexo e talvez injusta, mas em geral o Docker Swarm tende ser mais simples e leve do que o Kubernetes. Isto é pois o Docker Swarm aproveita vários comandos e configurações do Docker. Aprendendo o Docker você também já conhece uma parte do Docker Swarm.

O Kubernetes por sua vez tem uma curva de aprendizagem mais íngreme, define outras ferramentas como o kubectl. No entanto o Kubernetes possui uma comunidade enorme e muitas ferramentas ao redor, alem do amplo uso pelos provedores de cloud (AWS, Azure, Google). Além disso, o Kubernetes é baseado na experiencia do Google, é um projeto opensource do Google e isso ajudou muito na popularidade da ferramenta.
