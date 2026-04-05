# Namespace

## What It Is

Namespace는 클러스터 안에서 리소스를 논리적으로 구분하는 단위다.
같은 이름의 리소스도 서로 다른 Namespace에서는 공존할 수 있다.

## What To Remember

- 시험에서는 Namespace를 잘못 보고 다른 곳에 리소스를 만드는 실수가 자주 나온다.
- 항상 현재 컨텍스트와 Namespace를 확인하는 습관이 중요하다.
- 명령마다 `-n` 또는 `--namespace` 옵션을 빠르게 붙일 수 있어야 한다.

## Core Commands

```bash
kubectl create namespace dev
kubectl get ns
kubectl get pods -n dev
kubectl config set-context --current --namespace=dev
kubectl config view --minify | grep namespace
```

## Command Guide

- `kubectl create namespace dev`
  `dev` Namespace를 만든다. 리소스를 논리적으로 분리할 때 사용한다.
- `kubectl get ns`
  현재 클러스터의 Namespace 목록을 본다.
- `kubectl get pods -n dev`
  `dev` Namespace 안의 Pod만 조회한다. 시험에서는 `-n`을 빠르게 붙이는 습관이 중요하다.
- `kubectl config set-context --current --namespace=dev`
  현재 컨텍스트의 기본 Namespace를 `dev`로 바꾼다. 이후 매번 `-n dev`를 안 붙여도 된다.
- `kubectl config view --minify | grep namespace`
  지금 기본 Namespace가 무엇인지 확인한다. 작업 전후 점검용으로 유용하다.

YAML 뼈대 생성:

```bash
kubectl create namespace dev --dry-run=client -o yaml > namespace.yaml
kubectl apply -f namespace.yaml
```

- `kubectl create namespace dev --dry-run=client -o yaml > namespace.yaml`
  Namespace YAML 초안을 만든다.
- `kubectl apply -f namespace.yaml`
  파일 기준으로 Namespace를 생성한다.

## Checkpoints

- Namespace를 생성하고 전환할 수 있는가
- 리소스가 어느 Namespace에 있는지 빠르게 확인할 수 있는가
- 시험 중 Namespace 실수를 줄이기 위한 습관을 만들었는가
