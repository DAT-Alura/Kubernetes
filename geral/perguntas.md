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
