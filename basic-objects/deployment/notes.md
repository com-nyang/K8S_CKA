# Deployment

## What It Is

Deployment는 여러 개의 Pod를 안정적으로 관리하기 위한 리소스다.
이미지 업데이트, replica 수 조정, 롤링 업데이트, 롤백에 자주 사용된다.

## What To Remember

- 실제 운영 워크로드는 Pod보다 Deployment로 관리하는 경우가 많다.
- Deployment는 ReplicaSet을 만들고, ReplicaSet이 Pod를 유지한다.
- 이미지 변경과 replica 조정은 시험에서 자주 나온다.

## Core Commands

```bash
kubectl create deployment web --image=nginx:1.25
kubectl scale deployment web --replicas=3
kubectl set image deployment/web nginx=nginx:1.26
kubectl rollout status deployment/web
kubectl rollout history deployment/web
kubectl rollout undo deployment/web
```

## Command Guide

- `kubectl create deployment web --image=nginx:1.25`
  `web` Deployment를 생성한다. Pod를 직접 만들지 않고 관리형 워크로드를 시작할 때 쓴다.
- `kubectl scale deployment web --replicas=3`
  원하는 Pod 개수를 3개로 맞춘다. 부하 대응이나 시험 문제에서 replica 변경 요구가 나오면 바로 쓴다.
- `kubectl set image deployment/web nginx=nginx:1.26`
  Deployment 안 컨테이너 이미지를 새 버전으로 바꾼다. 롤링 업데이트가 시작된다.
- `kubectl rollout status deployment/web`
  업데이트가 정상 완료됐는지 확인한다. 이미지 변경 뒤 바로 확인하는 습관이 중요하다.
- `kubectl rollout history deployment/web`
  Deployment 변경 이력을 본다. 어떤 revision이 있었는지 확인할 때 사용한다.
- `kubectl rollout undo deployment/web`
  마지막 변경을 이전 버전으로 되돌린다. 잘못된 이미지나 설정을 롤백할 때 쓴다.

YAML 뼈대 생성:

```bash
kubectl create deployment web --image=nginx:1.25 --dry-run=client -o yaml > deploy.yaml
kubectl apply -f deploy.yaml
```

- `kubectl create deployment web --image=nginx:1.25 --dry-run=client -o yaml > deploy.yaml`
  Deployment YAML 초안을 생성한다. 이후 replica, label, resource 같은 필드를 붙이기 좋다.
- `kubectl apply -f deploy.yaml`
  YAML 파일 기준으로 Deployment를 생성하거나 수정한다.

예제 YAML:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
```

- Pod에서는 컨테이너 정의만 보면 됐지만, Deployment에서는 `selector`, `template`, `replicas`를 함께 본다.
- `selector.matchLabels`와 `template.metadata.labels`는 반드시 맞아야 한다.
- `replicas`를 조정하면 Deployment가 원하는 개수만큼 Pod를 유지한다.

## Rolling Update

롤링 업데이트는 기존 Pod를 한 번에 모두 바꾸지 않고, 새 버전 Pod로 조금씩 순차 교체하는 방식이다.
서비스 중단 가능성을 줄이기 위해 Deployment에서 기본적으로 자주 사용하는 배포 전략이다.

흐름:

1. Deployment 이미지나 Pod 템플릿을 변경한다.
2. 새 ReplicaSet이 만들어진다.
3. 새 Pod가 조금씩 올라온다.
4. 기존 Pod가 조금씩 내려간다.
5. 최종적으로 모든 Pod가 새 버전으로 바뀐다.

관련 명령:

```bash
kubectl set image deployment/web nginx=nginx:1.26
kubectl rollout status deployment/web
kubectl rollout history deployment/web
kubectl rollout undo deployment/web
```

- `kubectl set image deployment/web nginx=nginx:1.26`
  Deployment의 컨테이너 이미지를 바꿔 롤링 업데이트를 시작한다.
- `kubectl rollout status deployment/web`
  업데이트 진행 상태가 정상인지 확인한다.
- `kubectl rollout history deployment/web`
  어떤 버전 변경이 있었는지 revision 이력을 확인한다.
- `kubectl rollout undo deployment/web`
  업데이트에 문제가 있으면 이전 버전으로 롤백한다.

## Checkpoints

- Deployment와 Pod의 관계를 설명할 수 있는가
- replica 변경을 빠르게 할 수 있는가
- 이미지 업데이트와 롤백 절차를 손으로 수행할 수 있는가
