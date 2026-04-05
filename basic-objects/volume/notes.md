# Volume

## What It Is

Volume은 컨테이너가 데이터를 저장하거나 공유할 수 있게 해주는 스토리지 구조다.
기본 오브젝트 단계에서는 먼저 Pod 내부의 `emptyDir`와 ConfigMap 또는 Secret 마운트를 익히는 것이 좋다.

## What To Remember

- 컨테이너 재시작과 Pod 재생성은 다른 상황이다.
- `emptyDir`는 Pod가 살아 있는 동안 유지된다.
- ConfigMap과 Secret도 volume으로 마운트할 수 있다.

## Core Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
    - name: app
      image: busybox
      command: ["sh", "-c", "sleep 3600"]
      volumeMounts:
        - name: cache
          mountPath: /data
  volumes:
    - name: cache
      emptyDir: {}
```

## Command Guide

- `volumes`
  Pod 수준에서 어떤 저장소를 쓸지 정의한다. `emptyDir`, `configMap`, `secret` 같은 타입이 여기 들어간다.
- `volumeMounts`
  위에서 정의한 volume을 컨테이너 내부 어느 경로에 붙일지 정한다.
- `emptyDir: {}`
  Pod가 살아 있는 동안 유지되는 임시 저장소를 만든다. Pod가 삭제되면 데이터도 같이 사라진다.
- `mountPath: /data`
  컨테이너 안에서 실제로 파일이 보일 경로를 뜻한다.

실습 때 자주 쓰는 확인 명령:

```bash
kubectl apply -f volume-pod.yaml
kubectl exec -it volume-pod -- sh
kubectl describe pod volume-pod
kubectl delete pod volume-pod
```

- `kubectl apply -f volume-pod.yaml`
  volume 설정이 들어간 Pod를 생성하거나 수정한다.
- `kubectl exec -it volume-pod -- sh`
  컨테이너 내부 쉘로 들어가 파일 생성과 확인을 한다.
- `kubectl describe pod volume-pod`
  Pod에 volume이 어떻게 붙었는지와 이벤트를 본다.
- `kubectl delete pod volume-pod`
  Pod를 지워 `emptyDir` 데이터가 같이 사라지는지 확인할 수 있다.

## Checkpoints

- `volumes`와 `volumeMounts`의 관계를 설명할 수 있는가
- `emptyDir`의 생명주기를 이해하는가
- 파일 기반 설정 주입 방식을 손으로 구성할 수 있는가
