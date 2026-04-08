# Namespace Practice

## Problem 1

`dev` Namespace를 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl create namespace dev
```

확인:

```bash
kubectl get namespace dev
```

</details>

## Problem 2

`dev` Namespace 안에 Pod `web`을 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl run web --image=nginx:stable -n dev
```

확인:

```bash
kubectl get pod web -n dev
```

</details>

## Problem 3

현재 컨텍스트의 기본 Namespace를 `dev`로 바꿔라.

<details>
<summary>정답</summary>

```bash
kubectl config set-context --current --namespace=dev
```

</details>

## Problem 4

현재 어떤 Namespace를 기본으로 보고 있는지 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl config view --minify | grep namespace
```

또는

```bash
kubectl config get-contexts
# NAMESPACE 컬럼 확인
```

</details>

## Problem 5

기본 Namespace를 다시 `default`로 되돌려라.

<details>
<summary>정답</summary>

```bash
kubectl config set-context --current --namespace=default
```

</details>

## Problem 6

`dev` Namespace에 있는 리소스만 조회하라.

<details>
<summary>정답</summary>

특정 Namespace 지정:

```bash
kubectl get all -n dev
```

Pod만 조회:

```bash
kubectl get pods -n dev
```

모든 Namespace 조회:

```bash
kubectl get pods --all-namespaces
# 또는
kubectl get pods -A
```

</details>
