# Secret Practice

## Problem 1

`username=admin`, `password=s3cr3t` 값을 가진 Secret `db-secret`을 생성하라.

<details>
<summary>정답</summary>

```bash
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=s3cr3t
```

확인 (값은 base64 인코딩됨):

```bash
kubectl get secret db-secret -o yaml
```

디코딩 확인:

```bash
kubectl get secret db-secret -o jsonpath='{.data.password}' | base64 -d
# s3cr3t
```

</details>

## Problem 2

Pod 하나를 만들고 Secret 값을 환경 변수로 주입하라.

<details>
<summary>정답</summary>

```yaml
# pod-secret-env.yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
  - name: app
    image: nginx:stable
    envFrom:
    - secretRef:
        name: db-secret
```

```bash
kubectl apply -f pod-secret-env.yaml
```

특정 키만 주입할 경우:

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

</details>

## Problem 3

컨테이너 안에서 값이 전달됐는지 확인하라.

<details>
<summary>정답</summary>

```bash
kubectl exec secret-env-pod -- env | grep -E "username|password"
# username=admin
# password=s3cr3t
```

컨테이너 내부에서는 base64 디코딩된 평문으로 보인다.

</details>

## Problem 4

같은 Secret을 volume으로 마운트하라.

<details>
<summary>정답</summary>

```yaml
# pod-secret-vol.yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-vol-pod
spec:
  containers:
  - name: app
    image: nginx:stable
    volumeMounts:
    - name: secret-vol
      mountPath: /etc/secret
      readOnly: true
  volumes:
  - name: secret-vol
    secret:
      secretName: db-secret
```

```bash
kubectl apply -f pod-secret-vol.yaml
kubectl exec secret-vol-pod -- cat /etc/secret/password
# s3cr3t
```

</details>

## Problem 5

Secret과 ConfigMap의 용도 차이를 짧게 설명하라.

<details>
<summary>정답</summary>

| 구분 | ConfigMap | Secret |
|------|-----------|--------|
| 용도 | 일반 설정값 (비밀이 아닌 데이터) | 민감 정보 (비밀번호, 토큰, 인증서 등) |
| 저장 형식 | 평문 | base64 인코딩 (etcd 암호화 가능) |
| 접근 제어 | 일반 RBAC | RBAC + 별도 암호화 정책 권장 |

**핵심 차이**: Secret은 민감한 데이터를 다루기 위한 것으로, 평문 저장을 피하고 접근 제어를 강화할 수 있다. 단, base64는 암호화가 아닌 인코딩이므로 etcd 암호화(EncryptionConfiguration)를 별도로 설정해야 진정한 보안이 된다.

</details>
