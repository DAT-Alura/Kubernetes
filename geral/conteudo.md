## Historia do Kubernetes

O Kubernetes nasceu dentro do Google e foi doado em 2014 para Cloud Native Computing Foundation (CNCF) como projeto Open Source.

Como Kubernetes foi criado dentro da Google, ele se baseia na experiencia de outros dois sistemas famosos dentro da infraestrutura do Google, o Bork e Omega.

A primeira versão foi lançada em 2015 e desde lá a popularidade aumentou bastante, tanto que temos hoje em dia implementações de Kubernetes nos maiores provedores de nuvem como:

Amazon EKS
Azure Kubernetes Service
IBM Kubernetes Service
Google Kubernetes Engine
Red Hat OpenShift Container Platform
entre outros.

O Kubernetes foi escrito em Go e todo o código fonte está disponível no github:

[Kubernetes](https://github.com/kubernetes/kubernetes)

Talvez você já tenha estranhado o nome "Kubernetes" que vem da língua grega e significa timoneiro (o tripulante que navega o barco). Como o nome e a pronuncia é um pouco diferente, se popularizou a abreviação: K8s

## Comandos principais: Minikube

O Minikube implementa um cluster local do Kubernetes para Linux, Mac e Windows. O objetivo principal é ser uma ótima ferramenta para o desenvolvimento local de aplicativos Kubernetes para experimentar, aprender, rodar testes e usar na integração contínua.

Abaixo, temos os principais comandos do Minikube.

Para iniciar o Minikube:

```minikube start```

Para iniciar o Minikube com uma versão especifica do Kubernetes:

```minikube start --kubernetes-version="v1.16"```

Para acessar o dashboard do Kubernetes em execução:

```minikube dashboard```

Para ver o status do Minikube:

```minikube status```

Para parar o cluster:

```minikube stop```

Para se conectar pelo SSH com o nó master do cluster:

```minikube ssh```

E para remover o cluster:

```minikube delete```

E para remover todos os clusters e perfis:

```minikube delete --all```

## Comandos principais: kubectl

Para listar os pods:

```kubectl get pods```

Pbm funcionar para deployments e services, por exemplo::

```kubectl get services```

Para detalhes de um pod:

```kubectl describe pod <nome-pod>```

P comando describe tbm funciona para deployment e service, por exemplo::

```kubectl describe service <nome>```

Para criar pod, deployment ou service a partir de um arquivo yml:

```kubectl create -f <nome-arquivo-yml>```

Para remover pod, deployment ou service a partir de um arquivo yml:

```kubectl delete -f <nome-arquivo-yml>```

Para remover um pod:

```kubectl delete pod <nome-pod>```

Para remover um deployment:

```kubectl delete deployment <nome-deployment>```

Para remover um service:

```kubectl delete service <nome-service>```
