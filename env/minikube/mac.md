minikube

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64
sudo install minikube-darwin-arm64 /usr/local/bin/minikube

or 

brew install minikube
```

```
minikube start 
minikube dashboard & disown
minikube dashboard --url

-- 모니터링 가능하도록 구성
minikube addons enable metrics-server

minikube start --cpus=4 --memory=8192

minikube ssh

sudo cat /etc/kubernetes/manifests/kube-apiserver.yaml

```

드라이버를 docker로 실행하면 NodePort 등 사용할 수 없다.

```
minikube delete

brew install qemu
minikube start --driver=qemu
minikube addons enable ingress
```
