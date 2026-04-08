# Deployment Practice

## Problem 1

`nginx:1.25` 이미지를 사용하는 Deployment `web`을 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl create deployment web --image=nginx:1.25
```

</details>

## Problem 2

Deployment `web`의 replica 수를 3으로 변경하라.

<details>
<summary>정답</summary>

```bash
kubectl scale deployment web --replicas=3
```

또는 YAML 편집:

```bash
kubectl edit deployment web
# spec.replicas: 3
```

</details>

## Problem 3

Deployment `web`의 Pod 템플릿 라벨에 `app=web`을 설정하라.

<details>
<summary>정답</summary>

```bash
kubectl edit deployment web
```

`spec.template.metadata.labels` 아래에 `app: web` 추가:

```yaml
spec:
  template:
    metadata:
      labels:
        app: web
```

</details>

## Problem 4

Deployment `web`의 이미지를 `nginx:1.26`으로 변경하라.

<details>
<summary>정답</summary>

```bash
kubectl set image deployment/web nginx=nginx:1.26
```

컨테이너 이름이 다를 경우 먼저 확인:

```bash
kubectl get deployment web -o jsonpath='{.spec.template.spec.containers[0].name}'
```

</details>

## Problem 5

롤아웃 상태를 확인하고, 변경 이력을 조회하라.

<details>
<summary>정답</summary>

롤아웃 상태 확인:

```bash
kubectl rollout status deployment/web
```

변경 이력 조회:

```bash
kubectl rollout history deployment/web
```

특정 revision 상세 조회:

```bash
kubectl rollout history deployment/web --revision=2
```

</details>

## Problem 6

이전 버전으로 롤백하라.

<details>
<summary>정답</summary>

바로 이전 버전으로 롤백:

```bash
kubectl rollout undo deployment/web
```

특정 revision으로 롤백:

```bash
kubectl rollout undo deployment/web --to-revision=1
```

</details>

## Problem 7

`kubectl create deployment ... --dry-run=client -o yaml` 방식으로 `api` Deployment YAML을 만들고 적용하라.

<details>
<summary>정답</summary>

```bash
kubectl create deployment api --image=nginx:stable --dry-run=client -o yaml > api-deployment.yaml
kubectl apply -f api-deployment.yaml
```

</details>

## Problem 8

Deployment `web`이 관리하는 Pod 목록을 라벨 셀렉터로 조회하라.

<details>
<summary>정답</summary>

Deployment의 selector 확인:

```bash
kubectl get deployment web -o jsonpath='{.spec.selector.matchLabels}'
```

라벨로 Pod 조회:

```bash
kubectl get pods -l app=web
```

</details>

## Problem 9

Deployment `web`의 이미지를 존재하지 않는 태그로 바꿔 롤아웃 실패를 재현하고, 원인을 확인한 뒤 정상 태그로 복구하라.

<details>
<summary>정답</summary>

잘못된 이미지로 변경:

```bash
kubectl set image deployment/web nginx=nginx:doesnotexist
```

롤아웃 상태 확인 (멈춰 있음):

```bash
kubectl rollout status deployment/web
```

Pod 상태 및 원인 확인:

```bash
kubectl get pods
kubectl describe pod <pod-name>
# Events에서 ErrImagePull / ImagePullBackOff 확인
```

정상 태그로 복구:

```bash
kubectl rollout undo deployment/web
# 또는
kubectl set image deployment/web nginx=nginx:1.26
```

</details>
