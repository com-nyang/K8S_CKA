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
