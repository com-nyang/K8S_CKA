# ConfigMap Practice

## Problem 1

`APP_COLOR=blue` 값을 가진 ConfigMap `app-config`를 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl create configmap app-config --from-literal=APP_COLOR=blue
```

확인:

```bash
kubectl get configmap app-config -o yaml
```

</details>

## Problem 2

Pod 하나를 만들고 `app-config` 값을 환경 변수로 주입하라.

<details>
<summary>정답</summary>

```yaml
# pod-configmap-env.yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-env-pod
spec:
  containers:
  - name: app
    image: nginx:stable
    envFrom:
    - configMapRef:
        name: app-config
```

```bash
kubectl apply -f pod-configmap-env.yaml
```

특정 키만 주입할 경우:

```yaml
env:
- name: APP_COLOR
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_COLOR
```

</details>

## Problem 3

컨테이너 안에서 환경 변수 값이 실제로 들어왔는지 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl exec configmap-env-pod -- env | grep APP_COLOR
# APP_COLOR=blue
```

</details>

## Problem 4

같은 ConfigMap을 volume으로 마운트하라.

<details>
<summary>정답</summary>

```yaml
# pod-configmap-vol.yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-vol-pod
spec:
  containers:
  - name: app
    image: nginx:stable
    volumeMounts:
    - name: config-vol
      mountPath: /etc/config
  volumes:
  - name: config-vol
    configMap:
      name: app-config
```

```bash
kubectl apply -f pod-configmap-vol.yaml
```

</details>

## Problem 5

컨테이너 안에서 ConfigMap이 파일로 어떻게 보이는지 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl exec configmap-vol-pod -- ls /etc/config
# APP_COLOR

kubectl exec configmap-vol-pod -- cat /etc/config/APP_COLOR
# blue
```

ConfigMap의 각 키가 파일명이 되고, 값이 파일 내용이 된다.

</details>
