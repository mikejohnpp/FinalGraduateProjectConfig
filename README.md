# Cài đặt nfs
### Phía server
```
sudo apt install nfs-server
mkdir /data
sudo chown -R nobody:nogroup /data
sudo chmod -R 777 /data/

vi /etc/exports

/data *(rw,sync,no_subtree_check,no_root_squash)

exportfs -rav
sudo systemctl restart nfs-kernel-server
```

## Phía client
```
sudo apt install nfs-common
```

# Cài đặt k3s
### Phía server
```
curl -fsL https://get.k3s.io | sh -s - --node-name control.k8s

cat /var/lib/rancher/k3s/server/node-token
```
### Phía node
```
curl -fsL https://get.k3s.io | K3S_URL=https://<control node ip>:6443 K3S_TOKEN=<cluster token> sh -s - --node-name worker-1.k8s
```

# Cài helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh

export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

# Cài đặt ArgoCd
```
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

kubectl create namespace argocd

helm install argocd argo/argo-cd \
  --namespace argocd \
  -f argocd/values.yaml

kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
```

# Port-forward argocd và mysql
```
kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:80
kubectl port-forward --address 0.0.0.0 svc/mysql-service 3306:3306
```

