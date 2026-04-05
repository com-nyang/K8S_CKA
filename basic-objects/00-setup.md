# Practice Setup

## Goal

실습을 시작하기 전에 `kind`로 로컬 Kubernetes 클러스터를 만들고, `kubectl`이 정상 연결됐는지 확인한다.
이 문서의 목적은 Pod 문제를 풀기 전에 "내가 지금 어떤 클러스터와 어떤 Node를 보고 있는가"를 분명히 하는 것이다.

## Important Point

`kind`에서는 보통 Node를 직접 하나씩 만드는 것이 아니라 클러스터를 생성하면 Docker 컨테이너 형태의 Node가 함께 올라온다.
즉, 실습 시작 전 해야 하는 일은 "Node 생성"보다 "kind 클러스터 생성과 Node 확인"이다.

## Prerequisites

아래 도구가 로컬에 설치돼 있어야 한다.

- Docker
- `kubectl`
- `kind`

버전 확인:

```bash
docker --version
kubectl version --client
kind --version
```

## Create A Kind Cluster

가장 간단한 시작:

```bash
kind create cluster --name cka
```

클러스터가 만들어지면 `kubectl` 컨텍스트도 함께 잡힌다.

현재 컨텍스트 확인:

```bash
kubectl config current-context
```

## Check Nodes

클러스터 생성 후 가장 먼저 Node를 확인한다.

```bash
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node
```

정상이라면 보통 `cka-control-plane` 같은 이름의 Node가 `Ready` 상태로 보인다.

## Check Cluster Health

기본 시스템 Pod가 정상인지 확인한다.

```bash
kubectl get pods -A
kubectl cluster-info
kubectl get ns
```

최소한 `kube-system` 네임스페이스의 주요 Pod가 계속 오류 없이 올라와 있어야 한다.

## Recommended Alias And Habit

실습 속도를 높이려면 별칭과 자동완성을 미리 준비해두는 것이 좋다.

```bash
alias k=kubectl
complete -F __start_kubectl k
```

습관:

- 명령을 치기 전에 현재 컨텍스트를 확인한다.
- 문제를 풀기 전에 현재 Namespace를 확인한다.
- 리소스를 만든 직후 `get`과 `describe`로 상태를 본다.

## Namespace Check Before Practice

기본 Namespace 확인:

```bash
kubectl config view --minify | grep namespace
```

아무 값이 보이지 않으면 기본은 `default`다.

## Cleanup

실습을 다 끝낸 뒤 클러스터를 지우고 싶다면:

```bash
kind delete cluster --name cka
```

## Practice Start Checklist

아래가 모두 되면 Pod 실습으로 넘어가면 된다.

- `kind create cluster --name cka`를 실행했다.
- `kubectl get nodes` 결과에 `Ready` Node가 보인다.
- `kubectl get pods -A` 결과를 볼 수 있다.
- `kubectl config current-context`가 원하는 클러스터를 가리킨다.
- 현재 Namespace가 무엇인지 알고 있다.

## Next Step

환경 확인이 끝났으면 [pod/notes.md](./pod/notes.md)부터 시작한다.
