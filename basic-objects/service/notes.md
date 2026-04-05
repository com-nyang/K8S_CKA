# Service

## What It Is

Service는 Pod 집합에 대해 안정적인 접근 경로를 제공하는 리소스다.
Pod IP는 바뀔 수 있기 때문에 Service로 접근 대상을 고정한다.

## What To Remember

- `selector`로 어떤 Pod를 연결할지 결정한다.
- 시험 초반에는 `ClusterIP`와 `NodePort`를 우선 익히면 된다.
- Service가 연결되지 않을 때는 라벨과 셀렉터가 가장 먼저 확인 대상이다.

## Core Commands

```bash
kubectl expose deployment web --port=80 --target-port=80 --name=web-svc
kubectl get svc
kubectl describe svc web-svc
kubectl get endpoints web-svc
```

## Command Guide

- `kubectl expose deployment web --port=80 --target-port=80 --name=web-svc`
  `web` Deployment를 대상으로 Service를 만든다. `port`는 Service 포트, `target-port`는 Pod 컨테이너 포트다.
- `kubectl get svc`
  Service 목록과 타입, 클러스터 IP를 확인한다.
- `kubectl describe svc web-svc`
  Service 상세 정보와 selector를 본다. 연결 대상이 맞는지 확인할 때 중요하다.
- `kubectl get endpoints web-svc`
  Service가 실제로 연결한 Pod IP 목록을 본다. 비어 있으면 selector 또는 Pod 상태를 먼저 의심한다.

YAML 뼈대 생성:

```bash
kubectl expose deployment web --port=80 --target-port=80 --name=web-svc --dry-run=client -o yaml > svc.yaml
kubectl apply -f svc.yaml
```

- `kubectl expose deployment web --port=80 --target-port=80 --name=web-svc --dry-run=client -o yaml > svc.yaml`
  Service YAML 초안을 생성한다. 타입이나 selector를 더 손볼 때 편하다.
- `kubectl apply -f svc.yaml`
  Service 정의를 클러스터에 적용한다.

## Checkpoints

- Service와 Pod를 selector로 연결할 수 있는가
- `describe svc`와 `get endpoints`로 연결 상태를 확인할 수 있는가
- 셀렉터 불일치 문제를 빠르게 찾을 수 있는가
