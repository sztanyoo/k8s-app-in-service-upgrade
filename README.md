# Create k8s cluster

https://kind.sigs.k8s.io/docs/user/quick-start/

kind create cluster --name issu --image kindest/node:v1.23.6 --config kind.yaml

helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace --values nginx-values.yaml


# Build & publish example application

docker build -t  sztanyoo/issu:1 -f Dockerfile.1 .
docker build -t  sztanyoo/issu:2 -f Dockerfile.2 .
docker push sztanyoo/issu:1
docker push sztanyoo/issu:2

# Install example application
helm upgrade --install example1 ./exampleapp

# Upgrade application

watch kubectl get pods -o=custom-columns="NAME:.metadata.name,READY:.status.containerStatuses[0].ready,STATUS:.status.phase,IMAGE:.spec.containers[0].image,STARTED:.status.startTime"

Change values.yaml image tags

helm upgrade --install example1 ./exampleapp


# Cleanup

kind delete cluster --name issu