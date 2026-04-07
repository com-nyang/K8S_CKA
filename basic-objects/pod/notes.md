# Pod

## What It Is

Pod는 Kubernetes에서 배포 가능한 가장 작은 단위다.
하나 이상의 컨테이너가 같은 네트워크와 스토리지를 공유할 때 Pod로 묶인다.

## What To Remember

- Pod는 보통 직접 운영하기보다 Deployment 같은 상위 리소스로 관리한다.
- 단일 테스트나 디버깅에는 Pod를 직접 만드는 일이 자주 있다.
- `kubectl run`과 YAML 생성 둘 다 익숙해야 한다.

## Core Commands

```bash
kubectl run web --image=nginx
kubectl get pod
kubectl describe pod web
kubectl logs web
kubectl delete pod web
```

## Command Guide

- `kubectl run web --image=nginx`
  `web`이라는 이름의 Pod를 빠르게 생성한다. 단일 테스트용 Pod를 바로 띄울 때 가장 빠르다.
- `kubectl get pod`
  현재 Namespace의 Pod 목록과 상태를 본다. 생성 직후 `Running`, 실패 시 `ErrImagePull` 같은 상태를 확인할 때 쓴다.
- `kubectl describe pod web`
  Pod의 상세 정보와 이벤트를 본다. 스케줄링 실패, 이미지 풀 실패, 볼륨 문제 같은 원인을 찾을 때 핵심이다.
- `kubectl logs web`
  컨테이너 표준 출력 로그를 확인한다. 애플리케이션이 왜 실패하는지 볼 때 쓴다.
- `kubectl delete pod web`
  Pod를 삭제한다. 테스트 리소스를 정리하거나 다시 생성되게 할 때 사용한다.

YAML 뼈대 생성:

```bash
kubectl run web --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl apply -f pod.yaml
```

- `kubectl run web --image=nginx --dry-run=client -o yaml > pod.yaml`
  실제 생성은 하지 않고 Pod YAML 초안을 만든다. 시험에서는 YAML을 처음부터 다 치는 대신 이 방식이 빠르다.
- `kubectl apply -f pod.yaml`
  파일에 있는 정의를 클러스터에 적용한다. 생성과 수정에 모두 쓸 수 있다.

## Checkpoints

- `get`, `describe`, `logs`, `delete`를 빠르게 쓸 수 있는가
- YAML로 만든 Pod를 수정해 다시 적용할 수 있는가
- 실패한 Pod 상태를 보고 원인을 찾을 수 있는가

## Next Step

Pod를 직접 다룰 수 있게 되면 다음은 Deployment로 넘어가는 것이 맞다.
운영 환경에서는 개별 Pod보다 Deployment로 Pod 개수 유지, 이미지 업데이트, 롤백을 관리하는 경우가 훨씬 많다.

## Pod vs ReplicaSet vs Deployment

- Pod는 컨테이너를 실행하는 가장 작은 단위다.
- ReplicaSet은 원하는 개수의 Pod가 계속 유지되도록 맞춘다.
- Deployment는 ReplicaSet을 관리하면서 스케일링, 이미지 변경, 롤링 업데이트, 롤백까지 담당한다.

관계:

`Deployment -> ReplicaSet -> Pod`

시험 기준으로는 아래처럼 구분하면 된다.

- Pod
  단일 컨테이너 테스트, 로그 확인, 장애 분석에 자주 쓴다.
- ReplicaSet
  Pod 개수 유지 개념을 이해할 때 중요하지만 직접 만들 일은 많지 않다.
- Deployment
  실제 운영 워크로드와 CKA 실습에서 가장 자주 다루는 리소스다.

한 줄 요약:

- Pod는 실행 단위
- ReplicaSet은 개수 유지
- Deployment는 운영 관리
