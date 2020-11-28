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


### Inicializando o cluster

1. Control Plane X1

Executar o init no control plane X1:
```
kubeadm init --pod-network-cidr=10.88.0.0/16 --control-plane-endpoint "HAPROXY_IP:6443" --upload-certs
```

2. Deploy do CNI Plugin

- <https://docs.projectcalico.org/getting-started/kubernetes/self-managed-onprem/onpremises>

Executar o deploy do Calico:

```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

3. Executar o ```kubeadm join``` nos servidores do control plane X2 e X3:

```
  kubeadm join HAPROXY_IP:6443 --token ... \
    --discovery-token-ca-cert-hash sha256:... \
    --control-plane --certificate-key ...
```
