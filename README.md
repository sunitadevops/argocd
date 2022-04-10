Steps to install KIND Cluster:#####################

curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.12.0/kind-linux-amd64

chmod +x ./kind

mv ./kind /bin/kind

Steps to install Kubectl ###########################

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl

mv ./kubectl /bin/kubectl


yum install docker -y

systemctl start docker

systemctl enable docker


kind create cluster

#######Creating kind config###########

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker

#########Apply kind config cmd############333

kind create cluster --config config.yaml

Step-1: Create git-creds

kubectl --namespace argocd create secret generic git-creds --from-literal=username=satyamskic --from-literal=password=ghp_VS57GgNMCE9xxxxxxxxxxxxxxxxxx


Step-2: Create namespace and Apply argocd pods

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl config set-context --current --namespace=argocd

Step-3:Expose service to NodePort

kubectl edit svc argocd-server -n argocd

Step-4:Create image-updater pods

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/argocd-image-updater/stable/manifests/install.yaml

Step-5:Check logs

kubectl --namespace argocd logs --selector app.kubernetes.io/name=argocd-image-updater --follow

kubectl -n argocd rollout restart deployment argocd-image-updater
