# Service Practice

## Problem 1

`app=web` 라벨을 가진 Pod를 대상으로 Service `web-svc`를 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl expose deployment web --name=web-svc --port=80 --selector=app=web
```

또는 YAML로 직접 생성:

```yaml
# web-svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: web-svc
spec:
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
```

```bash
kubectl apply -f web-svc.yaml
```

</details>

## Problem 2

Service `web-svc`의 타입이 무엇인지 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl get service web-svc
# TYPE 컬럼 확인 → ClusterIP (기본값)
```

또는

```bash
kubectl describe service web-svc | grep Type
```

</details>

## Problem 3

Service `web-svc`의 엔드포인트를 조회하라.

<details>
<summary>정답</summary>

```bash
kubectl get endpoints web-svc
```

또는

```bash
kubectl describe service web-svc | grep Endpoints
```

</details>

## Problem 4

Pod 라벨 또는 Service selector를 일부러 틀리게 수정해 Service가 Pod를 찾지 못하게 만들어라.

<details>
<summary>정답</summary>

Service의 selector를 잘못된 값으로 변경:

```bash
kubectl edit service web-svc
# spec.selector.app: wrong-label 로 변경
```

또는 Pod의 라벨을 제거:

```bash
kubectl label pod <pod-name> app-
```

</details>

## Problem 5

왜 엔드포인트가 비어 있는지 확인하고 다시 정상 연결되게 수정하라.

<details>
<summary>정답</summary>

엔드포인트 확인:

```bash
kubectl get endpoints web-svc
# ENDPOINTS 컬럼이 <none> 이면 매칭 실패
```

원인 파악: Service selector와 Pod 라벨 비교:

```bash
kubectl get service web-svc -o jsonpath='{.spec.selector}'
kubectl get pods --show-labels
```

수정 (selector 복원):

```bash
kubectl edit service web-svc
# spec.selector.app: web 으로 복원
```

</details>

## Problem 6

`NodePort` 타입 Service를 하나 더 만들어 포트를 외부에 노출하라.

<details>
<summary>정답</summary>

```bash
kubectl expose deployment web --name=web-nodeport --type=NodePort --port=80
```

또는 YAML:

```yaml
# web-nodeport.yaml
apiVersion: v1
kind: Service
metadata:
  name: web-nodeport
spec:
  type: NodePort
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080   # 30000-32767 범위에서 지정 (생략 시 자동 할당)
```

```bash
kubectl apply -f web-nodeport.yaml
```

노드 IP와 NodePort로 접근:

```bash
kubectl get service web-nodeport
# NODE_PORT 확인 후 http://<NodeIP>:<NodePort> 로 접근
```

</details>
