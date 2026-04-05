# ConfigMap

## What It Is

ConfigMap은 설정 데이터를 키-값 형태로 저장하는 리소스다.
환경 변수나 파일 형태로 Pod에 주입할 수 있다.

## What To Remember

- 민감하지 않은 설정값을 담는다.
- 환경 변수로 넣을 수도 있고 파일로 마운트할 수도 있다.
- Pod에서 실제로 어떻게 보이는지 확인하는 연습이 중요하다.

## Core Commands

```bash
kubectl create configmap app-config --from-literal=APP_COLOR=blue
kubectl get configmap app-config -o yaml
kubectl describe configmap app-config
```

## Command Guide

- `kubectl create configmap app-config --from-literal=APP_COLOR=blue`
  `APP_COLOR=blue` 설정값을 가진 ConfigMap을 만든다. 간단한 설정 데이터를 빠르게 넣을 때 쓴다.
- `kubectl get configmap app-config -o yaml`
  ConfigMap 내용을 YAML 형태로 확인한다. 어떤 키와 값이 들어갔는지 보기 좋다.
- `kubectl describe configmap app-config`
  사람이 읽기 쉬운 형식으로 ConfigMap 내용을 확인한다.

YAML 뼈대 생성:

```bash
kubectl create configmap app-config --from-literal=APP_COLOR=blue --dry-run=client -o yaml > configmap.yaml
kubectl apply -f configmap.yaml
```

- `kubectl create configmap app-config --from-literal=APP_COLOR=blue --dry-run=client -o yaml > configmap.yaml`
  실제 생성 없이 YAML 초안을 만든다. 여러 키를 추가하거나 메타데이터를 수정하기 좋다.
- `kubectl apply -f configmap.yaml`
  ConfigMap 정의를 적용한다.

## Checkpoints

- ConfigMap을 env와 volume 두 방식으로 넣을 수 있는가
- Pod 내부에서 실제 값이 어떻게 보이는지 확인할 수 있는가
- 설정 데이터가 민감 정보가 아니라는 점을 구분할 수 있는가
