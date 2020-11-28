# K8S for Trutas - Dia 1

## Sumário
- [Kubernetes Essentials - Turma 1 - Dia 1](#kubernetes-essentials---turma-1---dia-1)
  - [Sumário](#sumário)
  - [Referências](#referências)
  - [O que é o Kubernetes](#o-que-é-o-kubernetes)
  - [Componentes](#componentes)
  - [Ambientes de estudo](#ambientes-de-estudo)
  - [Kubectl](#kubectl)
  - [Instalação - Sem alta disponibilidade](#instalação---sem-alta-disponibilidade)
    - [Pre-requisitos](#pre-requisitos)
    - [CNI - the Container Network Interface / POD Network](#cni---the-container-network-interface--pod-network)
    - [Instalação](#instalação)
  - [Exercício 1](#exercício-1)
  - [Exercício 2](#exercício-2)
  - [Exercício 3](#exercício-3)

## Referências

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
  - [Kubernetes Concepts](https://kubernetes.io/docs/concepts/)
  - [Kubernetes Tutorial](https://kubernetes.io/docs/tutorials/)

## O que é o Kubernetes

- <https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/>

## Componentes

- <https://kubernetes.io/docs/concepts/overview/components/>

![Kubernetes Components](../images/components-of-kubernetes.png)

## Ambientes de estudo

- [Minukube](https://kubernetes.io/docs/setup/learning-environment/minikube/)
- [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [Microk8s](https://microk8s.io/)

## Kubectl

- <https://kubernetes.io/docs/tasks/tools/install-kubectl/>
- <https://kubernetes.io/docs/reference/kubectl/overview/>
- <https://kubernetes.io/docs/reference/kubectl/kubectl/>

## Instalação - Sem alta disponibilidade

### Pre-requisitos

- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/>

### CNI - the Container Network Interface / POD Network

- <https://github.com/containernetworking/cni>
- <https://kubernetes.io/docs/concepts/cluster-administration/addons/>

### Instalação

- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/>
- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/>

## Exercício 1

Objetivo: Executar os pre-requisitos e instalar o pacotes do docker e dokubernetes

- Analisar e executar via servidor bastion os 3 Playbooks do diretório **playbooks-non-ha**

## Exercício 2

Objetivo: Inicializar o Kubernetes Control Plane

0. Logar no servidor do control plane(cp-truta-XX)

1. "Baixar" as images obrigatórias do Kubernetes
   
```
kubeadm config images pull
```

2. Inicializar o master node / control plane
   
```
kubeadm init --pod-network-cidr=10.244.0.0/16
```

OBS: Copiar o comando ```kubeadm join --discovery-token ...```

3. Copiar o arquivo de configuração para o home do usuário

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

4. Verificar o status do cluster kubernetes

```
kubectl get node -o wide
```

```
kubectl get pods --all-namespaces -o wide
```

5. Realizar o deploy do **plugin CNI Flannel** ou **Setup Pod Network**
   
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

6. Verificar o status do cluster kubernetes

```
kubectl get node -o wide
```

```
kubectl get pods --all-namespaces -o wide
```

## Exercício 3 

Objetivo: Adicionar os nodes ao cluster Kubernetes

0. Logar nos servidores node-truta-XX e executar o comando copiado no item 2 do exercício anterior

```
kubeadm join --discovery-token ...
```

1. Verificar o status do cluster kubernetes

```
kubectl get node -o wide
```

```
kubectl get pods --all-namespaces -o wide
```
