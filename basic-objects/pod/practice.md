# Pod Practice

## Problem 1

`nginx:stable` 이미지를 사용하는 Pod `web`을 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl run web --image=nginx:stable
```

</details>

## Problem 2

Pod `web`에 `app=web` 라벨을 설정하라.

<details>
<summary>정답</summary>

```bash
kubectl label pod web app=web
```

이미 생성할 때부터 라벨을 붙이려면:

```bash
kubectl run web --image=nginx:stable --labels=app=web
```

</details>

## Problem 3

현재 실행 중인 Pod `web`의 IP를 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl get pod web -o wide
```

또는

```bash
kubectl describe pod web | grep IP
```

</details>

## Problem 4

존재하지 않는 이미지 `nginx:notfound`를 사용하는 Pod `broken-web`을 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl run broken-web --image=nginx:notfound
```

</details>

## Problem 5

`broken-web` Pod가 정상적으로 뜨지 않는 원인을 `kubectl describe`로 확인하고 설명하라.

<details>
<summary>정답</summary>

```bash
kubectl describe pod broken-web
```

Events 섹션에서 `ErrImagePull` 또는 `ImagePullBackOff` 오류를 확인할 수 있다.  
원인: 지정한 이미지 태그 `nginx:notfound`가 레지스트리에 존재하지 않아 이미지를 pull하지 못함.

```bash
kubectl get pod broken-web
# STATUS: ErrImagePull 또는 ImagePullBackOff
```

</details>

## Problem 6

`kubectl run`이 아니라 YAML 파일을 생성해서 같은 Pod `web-yaml`을 만들어라.

<details>
<summary>정답</summary>

dry-run으로 YAML 생성 후 적용:

```bash
kubectl run web-yaml --image=nginx:stable --dry-run=client -o yaml > web-yaml.yaml
kubectl apply -f web-yaml.yaml
```

또는 직접 YAML 작성:

```yaml
# web-yaml.yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-yaml
spec:
  containers:
  - name: web-yaml
    image: nginx:stable
```

```bash
kubectl apply -f web-yaml.yaml
```

</details>

## Problem 7

실습이 끝난 뒤 `web`, `broken-web`, `web-yaml` Pod를 모두 삭제하라.

<details>
<summary>정답</summary>

```bash
kubectl delete pod web broken-web web-yaml
```

또는 한 번에:

```bash
kubectl delete pod web broken-web web-yaml --grace-period=0 --force
```

</details>
