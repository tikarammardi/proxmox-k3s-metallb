
# Configuring K3S On Proxmox with metal LB

## On master k3s node machine
```
curl -sfL https://get.k3s.io | sh -s - server --disable servicelb --write-kubeconfig-mode 644
```
## disablinsg servicelb since we will be using metallb
## kubeconfig mode to 644 so that we don't have to type sudo all the time while using kubectl command

## Copy kubeconfig file to local system

```bash
scp tikaram@192.168.1.36:/etc/rancher/k3s/k3s.yaml k3s.yaml
```
## Get k3stoken form master node
```
sudo cat /var/lib/rancher/k3s/server/node-token
```

## Install k3s on agent node machine and join the node

```
curl -sfL https://get.k3s.io | K3S_URL=https://192.168.1.36:6443 K3S_TOKEN=K10806c12b125c56fa6de6d72b3e43dd540b7c85e723e0661eff471055232827::server:556cc033e378f3cd65606581314688 sh -
```

## Deploy metallb-config.yaml file

```
kubectl apply -f metallb-config.yaml
```

## expose service as LoadBalancer, metallb will allocate ip addr from the pool available

