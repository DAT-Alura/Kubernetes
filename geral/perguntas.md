# Perguntas

## Aula 1

1 - O que é verdade sobre um Pod dentro do Kubernetes?
- __Um Pod é a menor unidade de deploy no Kubernetes.__
> Alternativa correta! Um Pod é a menor unidade de deploy!
- __Um Pod tem no mínimo um container associado.__
> Alternativa correta! Um Pod normalmente tem apenas um container associado, mas existe a possibilidade de definir outros containers no mesmo Pod, desde que varia a porta de rede entre os containers isso funciona.
- Quando um Pod morre, ele reinicia sozinho.
- __Um Pod tem um IP.__
> Alternativa correta! Um Pod possui exatamente um IP!

2 - Já conhecemos dois protagonistas desse curso, o minikube e kubectl. Sobre eles, quais afirmações abaixo são verdadeiras?
- kubectl é um orquestrador de container parecido com docker-compose.
- minikube é um container engine e serve principalmente para rodar containers Docker.
- __minikube é uma implementação do Kubernetes.__
> Correto, o minikube é uma implementação de simples uso do Kubernetes. Existem outras como Google GKE, Amazon EKS ou Azure Kubernetes Service.
- __kubectl é a interface de linha de comando para gerenciar Kubernetes.__
> Correto, tanto que já usamos o kubectl para criar um POD!

## Aula 2

1 - Quais das características abaixo aplicam para o recurso Deployment?
- __É responsável por definir a quantidade de replicas do Pod__
> Correto, podemos definir através dashboard, pelo kubectl ou na arquivo yml a quantidade de replicas.
- É responsável por definir a porta de rede do recurso.
- É responsável por distribuir (balancear) a carga.
- __É responsável por garantir que o Pod está disponível (rodando).__
> Correto, um Pod criado SEM deployment não garante disponibilidade.

2 - O que aprendemos sobre serviços nessa aula?
- __Oferecem Load-Balancing__
> Alternativa correta! Por padrão usam Round-robin.
- __Possuem um IP estável__
> Alternativa correta! Correto, isso é a principal tarefa do serviço. Ele possui um IP que não muda e dá acesso a um ou mais Pods.
- Reiniciam Pods que morreram
- Rodam a imagem associada

3 - Recentemente verificamos que o número de usuários do portal de notícias cresceu muito devido a uma ação da equipe de marketing da Alura, e com isso precisamos aumentar a capacidade de processamento do nosso ambiente para atender a essa demanda. Para isso, estamos utilizando o Kubernetes, para orquestrar todos os Pods que criamos em nosso ambiente.
Como um "pod" é a menor unidade dentro de um cluster Kubernetes, o usuário final não tem acesso direto ao conteúdo desse Pod, pois o IP que ele ganha dentro do cluster é um IP interno.
Em nosso ambiente local, utilizando o Minikube, o que é preciso para que um Pod tenha seu conteúdo disponível aos usuários?

- __A__
Podemos colocar um controlador do tipo Deployment para monitorar o estado do nosso Pod, e para garantir o acesso com um IP externo, podemos criar um serviço do tipo LoadBalancer:
``` YML
#service loadBalancer
apiVersion: v1
kind: Service
metadata:
  name: servico-aplicacao-noticia
spec:
  type: LoadBalancer
...

#deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aplicacao-noticia-deployment
spec:
...
```
> Correto! Criando um serviço do tipo LoadBalancer, além de garantir o acesso, o serviço é responsável por dividir a carga nos containers.

- B
Da mesma forma que acessamos um container docker, podemos acessar diretamente um Pod. Essa é a única maneira de acessar o Pod, e podemos obter o endereço ip com o comando describe: ```kubectl describe pods```

- C
Podemos criar apenas um serviço do tipo LoadBalancer, pois ele tem acesso direto ao Pod:
``` YML
#service loadBalance
apiVersion: v1
kind: Service
metadata:
  name: servico-aplicacao-noticia
spec:
  type: LoadBalancer
```