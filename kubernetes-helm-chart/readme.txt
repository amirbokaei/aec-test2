
# start minikube cluster
minikube start \
  --cpus 3 \
  --memory 10000

# enable ingress addons (becase can't map 22/80/443 in internal ingress)
minikube addons enable ingress

# add gitlab helm chart
helm repo add gitlab https://charts.gitlab.io/
helm repo update

# installing
helm upgrade --install gitlab gitlab/gitlab \
   --timeout 600s   \
   --set global.hosts.domain=$(minikube ip).nip.io \
   --set global.hosts.externalIP=$(minikube ip) \
   -f values-gitlab.yaml -n gitlab --create-namespace


# get root password
kubectl get secret gitlab-gitlab-initial-root-password -ojsonpath='{.data.password}' -n gitlab | base64 --decode ; echo


# gitlab installed on https://gitlab.192.168.49.2.nip.io/ get middel ip from `minikube ip` command
