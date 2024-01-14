## Basics
```sh
kind create cluster --config=kubecluster.yml

docker pull traefik/whoami:v1.10.1
kind load docker-image traefik/whoami:v1.10.1

kind load docker-image nginx:1.25.3-bookworm
kubectl exec -it pods/nginx -- bash

kind load docker-image nicolaka/netshoot:1.0.0
kubectl run -it --rm netshoot --image=nicolaka/netshoot:1.0.0 --restart=Never -- bash

curl app-service.default.svc.cluster.local

docker exec -i kind-control-plane sh -c 'crictl images'

kind delete cluster
```

## Ingress
```sh
docker pull ghcr.io/projectcontour/contour:v1.27.0
docker pull envoyproxy/envoy:v1.28.0
kind load docker-image ghcr.io/projectcontour/contour:v1.27.0
kind load docker-image envoyproxy/envoy:v1.28.0

kubectl apply -f https://projectcontour.io/quickstart/contour.yaml

kubectl patch daemonsets -n projectcontour envoy -p '{"spec":{"template":{"spec":{"nodeSelector":{"ingress-ready":"true"},"tolerations":[{"key":"node-role.kubernetes.io/control-plane","operator":"Equal","effect":"NoSchedule"},{"key":"node-role.kubernetes.io/master","operator":"Equal","effect":"NoSchedule"}]}}}}'
```

## LoadBalancer
```sh
docker pull quay.io/metallb/controller:v0.13.7
docker pull quay.io/metallb/speaker:v0.13.7
kind load docker-image quay.io/metallb/controller:v0.13.7
kind load docker-image quay.io/metallb/speaker:v0.13.7

kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.7/config/manifests/metallb-native.yaml

docker network inspect -f '{{.IPAM.Config}}' kind
kubectl apply -f https://kind.sigs.k8s.io/examples/loadbalancer/metallb-config.yaml
```

## Calico
```sh
docker pull calico/cni:v3.27.0
docker pull calico/node:v3.27.0
docker pull calico/kube-controllers:v3.27.0

kind load docker-image calico/cni:v3.27.0
kind load docker-image calico/node:v3.27.0
kind load docker-image calico/kube-controllers:v3.27.0

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/calico.yaml
```
