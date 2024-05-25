# Install docker - Master
```bash
curl https://releases.rancher.com/install-docker/24.0.7.sh | sh
```
# Install Runcher - Master
```bash
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  rancher/rancher:latest \
  --acme-domain rancher.devopsbr.com.br
```
# Install Kubectl - Master  
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
mkdir ~/.kube
vi ~/.kube/config
```
# Install Metallb
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.4/config/manifests/metallb-native.yaml
```
# Install ARGOCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
# Install longhorn

Realizar a instalação nos Nodes
```bash
sudo apt-get update
sudo apt-get install open-iscsi
sudo apt-get install nfs-common
sudo systemctl enable --now iscsid
```
Instalar longhorn atraves do helm do rancher
# Install CertManager
```bash
kubectl create namespace cert-manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.4/cert-manager.yaml
kubectl get pods --namespace cert-manager
```
# Install Postgres-operator
## First, clone the repository and change to the directory
```bash
git clone https://github.com/zalando/postgres-operator.git
cd postgres-operator
```
## apply the manifests in the following order
```bash
kubectl create -f /manifests/configmap.yaml  # configuration
kubectl create -f /manifests/operator-service-account-rbac.yaml  # identity and permissions
kubectl create -f /manifests/postgres-operator.yaml  # deployment
#kubectl create -f /manifests/api-service.yaml  # operator API to be used by UI
```