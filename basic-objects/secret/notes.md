# Secret

## What It Is

Secret은 민감한 데이터를 Kubernetes에 저장하기 위한 리소스다.
비밀번호, 토큰, 인증 정보 같은 값을 담을 때 사용한다.

## What To Remember

- 사용 방법은 ConfigMap과 비슷하지만 목적은 민감 정보 저장이다.
- YAML에서는 값이 base64로 표현될 수 있다.
- Secret도 env와 volume 방식으로 Pod에 전달할 수 있다.

## Core Commands

```bash
kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=s3cr3t
kubectl get secret db-secret
kubectl describe secret db-secret
```

## Command Guide

- `kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=s3cr3t`
  민감한 키-값 데이터를 가진 Secret을 만든다. 로그인 정보나 토큰류를 저장할 때 사용한다.
- `kubectl get secret db-secret`
  Secret 리소스 존재 여부를 확인한다. 기본 출력에서는 실제 값은 바로 보이지 않는다.
- `kubectl describe secret db-secret`
  Secret 타입과 키 이름을 확인한다. 민감값 자체보다 구조 확인용으로 자주 쓴다.

YAML 뼈대 생성:

```bash
kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=s3cr3t --dry-run=client -o yaml > secret.yaml
kubectl apply -f secret.yaml
```

- `kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=s3cr3t --dry-run=client -o yaml > secret.yaml`
  Secret YAML 초안을 만든다. 결과 YAML에서는 값이 base64 형태로 표현될 수 있다.
- `kubectl apply -f secret.yaml`
  Secret 정의를 적용한다.

## Checkpoints

- Secret 생성 명령을 빠르게 쓸 수 있는가
- base64 표현과 실제 민감 정보 저장 목적을 구분할 수 있는가
- Pod에서 env와 volume 둘 다 사용할 수 있는가
