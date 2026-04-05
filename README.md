# K8S CKA Study Guide

CKA(Certified Kubernetes Administrator)는 개념 암기보다 실습 숙련도가 더 중요한 시험이다.
이 저장소는 CKA 학습 방향을 빠르게 잡고, 실습 위주로 공부하기 위한 개인 정리용 문서다.

## Study Principles

- 이론보다 `kubectl` 실습 시간을 더 많이 가져간다.
- 한 번 읽고 끝내지 않고, 같은 작업을 반복해서 손에 익힌다.
- YAML을 처음부터 전부 작성하지 말고 생성 후 수정하는 방식으로 속도를 올린다.
- 장애 상황을 직접 보고 원인을 찾는 연습을 반드시 포함한다.

## Recommended Order

1. Kubernetes 기본 개념 익히기
2. 시험 범위별 주제 학습
3. 실습으로 반복
4. `kubectl` 명령 숙련도 높이기
5. 문제 풀이와 모의고사로 마무리

## Core Topics

### 1. Basic Objects

먼저 아래 리소스를 직접 만들고 수정할 수 있어야 한다.

- Pod
- Deployment
- Service
- ConfigMap
- Secret
- Volume
- Namespace

학습 문서:

- [basic-objects/README.md](./basic-objects/README.md)

### 2. Exam Domains

CKA는 보통 다음 주제로 나눠서 공부하면 정리하기 쉽다.

- Cluster Architecture, Installation and Configuration
- Workloads and Scheduling
- Services and Networking
- Storage
- Troubleshooting

## Practice First

아래 작업은 문서만 읽지 말고 반드시 직접 수행한다.

- 파드 생성과 삭제
- Deployment 생성, 이미지 변경, replica 조정
- Service 생성과 포트 노출
- ConfigMap, Secret 연결
- Volume, PVC 연결
- Job, CronJob 실행
- Node 상태 확인
- 장애 상황에서 원인 찾기

문제 해결 시 자주 쓰게 되는 명령:

- `kubectl get`
- `kubectl describe`
- `kubectl logs`
- `kubectl exec`
- `kubectl edit`
- `kubectl apply -f`
- `kubectl expose`
- `kubectl run`
- `kubectl create ... --dry-run=client -o yaml`

## Recommended Daily Routine

매일 1시간에서 2시간 정도를 기준으로 아래처럼 나누는 것이 효율적이다.

- 20분: 개념 정리
- 40분: 직접 실습
- 20분: 문제 풀이와 복기

핵심은 정답 암기보다 "왜 이 명령과 리소스를 써야 하는가"를 설명할 수 있어야 한다는 점이다.

## 4-Week Plan

### Week 1

- Pod
- Deployment
- ReplicaSet
- Service
- ConfigMap
- Secret
- Volume
- Namespace

목표:

- 기본 리소스를 빠르게 생성하고 수정할 수 있다.
- YAML 뼈대를 생성한 뒤 필요한 필드만 수정하는 습관을 만든다.

### Week 2

- Scheduler
- Taints and Tolerations
- Node Selector / Affinity / Anti-Affinity
- Liveness Probe / Readiness Probe / Startup Probe
- Job
- CronJob

목표:

- 스케줄링 조건을 이해하고 원하는 노드에 워크로드를 배치할 수 있다.
- 애플리케이션 상태 점검 설정을 구분해서 적용할 수 있다.

### Week 3

- Cluster Networking
- DNS
- Ingress
- NetworkPolicy
- PersistentVolume / PersistentVolumeClaim
- StorageClass

목표:

- 서비스 통신 흐름을 이해하고 네트워크 접근 문제를 진단할 수 있다.
- 스토리지 리소스 연결과 동작 방식을 설명할 수 있다.

### Week 4

- Troubleshooting 집중
- 자주 틀리는 범위 복습
- 모의고사 반복

목표:

- 제한된 시간 안에 문제를 빠르게 읽고 필요한 리소스를 바로 수정할 수 있다.
- 장애 원인을 `describe`, `logs`, 이벤트, 리소스 정의에서 신속하게 찾을 수 있다.

## High-Value Habits

- `--dry-run=client -o yaml`로 템플릿을 먼저 만든다.
- 공식 문서에서 필요한 예시를 빠르게 찾는 연습을 한다.
- 실습 환경은 `kind`, `minikube`, 혹은 랩 환경 중 하나로 고정한다.
- 틀린 문제는 정답만 보지 말고 재현해서 다시 풀어본다.
- 시험은 시간 싸움이므로 짧고 정확한 명령 습관이 중요하다.

## Suggested Next Steps

바로 시작하려면 아래 순서가 현실적이다.

1. 로컬 실습 환경 하나 준비하기
2. Week 1 주제부터 손으로 직접 생성해 보기
3. 하루에 최소 3개 이상 리소스를 YAML과 명령어 둘 다로 다뤄 보기
4. 매주 마지막 날에 모의 문제를 풀고 약한 주제를 다시 정리하기

## One-Line Summary

CKA는 "많이 읽는 공부"보다 "빠르게 만들고, 고치고, 장애를 찾는 연습"이 합격에 더 가깝다.
