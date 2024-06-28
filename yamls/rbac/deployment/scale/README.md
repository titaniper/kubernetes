```
kubectl apply -f namespace.yaml
kubectl config set-context --current --namespace=rbac-test
kubectl apply -f ./yamls/rbac/deployment/scale/nginx-deployment.yaml
kubectl apply -f secret.yaml
kubectl apply -f service-account.yaml 


# Deployment 등 권한 주기 위해 ClusterRole도 설정


kubectl apply -f scale-role.yaml     
kubectl apply -f scale-role-binding.yaml
```

- ServiceAccount 토큰 가져오기
```
SECRET_NAME=$(kubectl get sa rbac-user -n rbac-test -o jsonpath="{.secrets[0].name}")
echo $SECRET_NAME
TOKEN=$(kubectl get secret $SECRET_NAME -n rbac-test -o jsonpath="{.data.token}" | base64 --decode)
echo $TOKEN
kubectl config set-credentials rbac-user --token=$TOKEN


or

SECRET_NAME=$(kubectl -n rbac-test get secret | grep rbac-user | awk '{print $1}')
TOKEN=$(kubectl -n rbac-test get secret $SECRET_NAME -o jsonpath="{.data.token}" | base64 --decode)
CLUSTER_NAME=$(kubectl config get-contexts $(kubectl config current-context) --no-headers | awk '{print $3}')
CLUSTER_ENDPOINT=$(kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}')
CERTIFICATE_AUTHORITY=$(kubectl config view --minify -o jsonpath='{.clusters[0].cluster.certificate-authority}')
kubectl config set-credentials rbac-user --token=$TOKEN
kubectl config set-context rbac-user-context --cluster=$CLUSTER_NAME --namespace=rbac-test --user=rbac-user
kubectl config use-context rbac-user-context




```



Context 생성 및 설정
```
kubectl config set-context rbac-test --cluster=docker-desktop --namespace=rbac-test --user=rbac-user
kubectl config use-context rbac-test
```

권한 테스트
```
kubectl scale deployment  nginx-deployment --replicas=2 -n rbac-test

kubectl patch -n rbac-test deployment nginx-deployment -p '{"spec": {"template": {"spec": {"containers": [{"name": "nginx", "image": "nginx:1.16.0"}]}}}}'
kubectl config current-context
kubectl config view --minify | grep namespace:

kubectl config set-credentials rbac-user \
    --client-certificate=<path-to-client-certificate> \
    --client-key=<path-to-client-key>

kubectl config use-context rbac-test


client-certificate-data,  client-key-data 추천
```


-- token: BLSfstUpjHCPn+KW8h2rOjGcnHfTbH6Q91LhWZeC+pw=





돌아가기
```
CURRENT_CONTEXT=$(kubectl config current-context)
kubectl config set-context rbac-test --cluster=docker-desktop --namespace=rbac-test --user=rbac-user
kubectl config use-context rbac-test
kubectl config use-context $CURRENT_CONTEXT
kubectl config view
kubectl config view --minify --context=rbac-test

kubectl config view --minify --context=docker-desktop
kubectl config use-context docker-desktop


kubectl delete -f
```


권한 출력
```
kubectl auth can-i --list --as=system:serviceaccount:rbac-test:rbac-user

kubectl get rolebinding --all-namespaces | grep rbac-user
kubectl delete rolebinding scale-rolebinding -n rbac-test
kubectl get rolebindings --all-namespaces

kubectl get roles --all-namespaces

kubectl get rolebinding -n rbac-test


kubectl get clusterrolebinding | grep rbac-user
kubectl delete clusterrolebinding rbac-user-clusterrolebinding

kubectl get role --all-namespaces | grep rbac-user
kubectl delete role scale-role -n rbac-test

kubectl get clusterrole | grep rbac-user
kubectl delete clusterrole rbac-user-clusterrole

kubectl get clusterrolebinding
kubectl get clusterrolebindings
kubectl get clusterroles

kubectl cache delete

kubectl config unset users.rbac-user.client-certificate-data
kubectl config unset users.rbac-user.client-key-data

```


권한 물어보기
```
kubectl auth can-i get pods
kubectl auth can-i get deployments
kubectl auth can-i update deployments
kubectl auth can-i create deployments.apps --as=system:serviceaccount:rbac-test:rbac-user
kubectl auth can-i list pods --as=rbac-user -n rbac-test
kubectl auth can-i update deployments/scale --as=system:serviceaccount:rbac-test:rbac-user -n rbac-test



kubectl get serviceaccount

```



권한 삭제
```
kubectl get rolebindings --all-namespaces | grep rbac-user
kubectl get clusterrolebindings | grep rbac-user
kubectl get roles --all-namespaces | grep rbac-user
kubectl get clusterroles | grep rbac-user

kubectl delete role scale-role -n rbac-test
kubectl delete rolebinding scale-rolebinding -n rbac-test
kubectl delete clusterrole rbac-user-clusterrole
kubectl delete clusterrolebinding rbac-user-clusterrolebinding

kubectl get rolebindings -n rbac-test
kubectl delete rolebinding <rolebinding-name> -n rbac-test

kubectl get clusterrolebindings
kubectl delete clusterrolebinding rbac-test-clusterrolebinding
kubectl delete clusterrolebinding rbac-test-rolebinding
```

컨텍스트 확인
```
kubectl config current-context
kubectl config get-contexts
```

Update와 Patch 차이
```
# 성공
kubectl set image deployment/nginx-deployment nginx=nginx:1.16 -n rbac-test


# 성공
kubectl patch deployment nginx-deployment -p '{"spec": {"replicas": 5}}' -n rbac-test


kubectl apply -f nginx-deployment.yaml 

```