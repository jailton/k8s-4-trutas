# K8S for Trutas - Dia 2

## Kubernetes - High Availability - Stacked etcd

![Kubernetes Components](../images/kubeadm-ha-topology-stacked-etcd.svg)

- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/>
- <https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/>

## Inicializando o cluster

1. Control Plane X1

Executar o init no control plane X1:
```
kubeadm init --pod-network-cidr=10.88.0.0/16 --control-plane-endpoint "HAPROXY_IP:6443" --upload-certs
```

2. CNI Plugin

- <https://docs.projectcalico.org/getting-started/kubernetes/self-managed-onprem/onpremises>

Executar o deploy do Calico:
```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

3. Executar o kubeadm init nos servidores do control plane X2 e X3:

```
  kubeadm join HAPROXY_IP:6443 --token ... \
    --discovery-token-ca-cert-hash sha256:... \
    --control-plane --certificate-key ...
```