# K8S for Trutas - Dia 2

## Kubernetes - High Availability - Stacked etcd

![Kubernetes Components](../images/kubeadm-ha-topology-stacked-etcd.svg)

- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/>
- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/>

### Executando os playbooks para:

<https://github.com/jailton/k8s-4-trutas/tree/main/ansible>

- Executar os pré-requistos
- Instalar o HAProxy
- Instalar o Docker
- Instalar o Kubeadm, Kubelet e Kubectl

### Ajustar o haproxy.cfg

1. Acessar o servidor do haproxy:

```
ssh root@lb-truta-X0
```

2. Editar o arquivo de configuração do HAProxy:

```
vi /etc/haproxy/haproxy.cfg
```

3. Adicionar no final do arquivo:

```    
    server cp-truta-X1 IP_CONTROL_PLANE_X1:6443 check fall 3 rise 2
    server cp-truta-X1 IP_CONTROL_PLANE_X1:6443 check fall 3 rise 2
    server cp-truta-X1 IP_CONTROL_PLANE_X1:6443 check fall 3 rise 2
```

4. Restart do haproxy

```
systemctl restart haproxy
```