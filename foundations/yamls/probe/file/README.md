호스트 네트워크 모드 사용:
이 방법은 보통 사용되지 않지만, 특정 상황에서는 유용할 수 있습니다. 호스트 네트워크 모드를 사용하면 파드가 호스트 머신의 네트워크 네임스페이스를 공유하게 됩니다. 이는 Docker가 아닌 단일 노드 클러스터에서만 가능하며, 다음과 같이 설정할 수 있습니다.

yaml
Copy code
apiVersion: v1
kind: Pod
metadata:
  name: streams-app
spec:
  hostNetwork: true
  containers:
  - name: streams-app
    image: devjyk/streams-app:1.0
    env:
    - name: KAFKA_BROKER
      value: "localhost:9092"
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
    readinessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
쿠버네티스 ExternalName 서비스 사용:
쿠버네티스 클러스터 외부의 서비스를 클러스터 내부에서 접근할 수 있도록 ExternalName 서비스를 설정할 수 있습니다. 이를 통해 외부의 서비스를 DNS 이름으로 접근할 수 있게 됩니다.

yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: kafka-external
spec:
  type: ExternalName
  externalName: "localhost"
쿠버네티스 포트 포워딩 사용:
이 방법은 개발 및 디버깅 목적으로 자주 사용됩니다. kubectl port-forward 명령어를 사용하여 호스트 머신의 포트를 쿠버네티스 클러스터 내부의 파드로 포워딩할 수 있습니다.

sh
Copy code
kubectl port-forward <pod-name> 9092:9092
이를 통해 파드에서 localhost:9092로 접근하면 실제로는 호스트 머신의 포트 9092로 연결됩니다.