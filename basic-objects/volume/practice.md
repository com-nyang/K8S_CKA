# Volume Practice

## Problem 1

`emptyDir`를 사용하는 Pod `volume-pod`를 생성하라.

<details>
<summary>정답</summary>

```yaml
# volume-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - name: app
    image: nginx:stable
    volumeMounts:
    - name: data-vol
      mountPath: /data
  volumes:
  - name: data-vol
    emptyDir: {}
```

```bash
kubectl apply -f volume-pod.yaml
```

</details>

## Problem 2

컨테이너 안에서 `/data/hello.txt` 파일을 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl exec volume-pod -- sh -c "echo 'hello' > /data/hello.txt"
```

확인:

```bash
kubectl exec volume-pod -- cat /data/hello.txt
# hello
```

</details>

## Problem 3

같은 Pod 안에서 파일이 유지되는지 확인하라.

<details>
<summary>정답</summary>

`emptyDir`는 Pod가 살아있는 동안 유지된다. 컨테이너가 재시작돼도 같은 Pod면 데이터가 남아있다.

```bash
kubectl exec volume-pod -- ls /data
# hello.txt
```

사이드카 컨테이너가 있다면 해당 컨테이너에서도 동일 파일에 접근 가능:

```bash
kubectl exec volume-pod -c <sidecar-container> -- cat /data/hello.txt
```

</details>

## Problem 4

Pod를 삭제한 뒤 같은 파일이 남아 있는지 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl delete pod volume-pod
kubectl apply -f volume-pod.yaml
kubectl exec volume-pod -- ls /data
# (비어 있음 — hello.txt 없음)
```

**결론**: `emptyDir`는 Pod 생명주기와 함께하며, Pod가 삭제되면 데이터도 사라진다.  
데이터를 영구적으로 유지하려면 `PersistentVolume`을 사용해야 한다.

</details>

## Problem 5

ConfigMap 또는 Secret을 volume으로 마운트하는 Pod를 하나 더 만들어라.

<details>
<summary>정답</summary>

ConfigMap을 volume으로 마운트하는 예시 (`app-config` ConfigMap이 있다고 가정):

```yaml
# cm-vol-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-vol-pod
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
kubectl apply -f cm-vol-pod.yaml
kubectl exec cm-vol-pod -- ls /etc/config
kubectl exec cm-vol-pod -- cat /etc/config/APP_COLOR
```

Secret을 volume으로 마운트하는 예시 (`db-secret` Secret이 있다고 가정):

```yaml
volumes:
- name: secret-vol
  secret:
    secretName: db-secret
```

</details>
