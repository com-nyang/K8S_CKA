# Basic Objects

CKA 초반에는 아래 리소스를 손으로 직접 만들고 수정하는 데 익숙해지는 것이 중요하다.
이 디렉터리는 기본 오브젝트를 하나씩 나눠서 학습하기 위한 문서 모음이다.

## Order

1. [00-setup.md](./00-setup.md)
2. [pod/notes.md](./pod/notes.md)
3. [deployment/notes.md](./deployment/notes.md)
4. [service/notes.md](./service/notes.md)
5. [configmap/notes.md](./configmap/notes.md)
6. [secret/notes.md](./secret/notes.md)
7. [volume/notes.md](./volume/notes.md)
8. [namespace/notes.md](./namespace/notes.md)

## Practice Sets

- [pod/practice.md](./pod/practice.md)
- [deployment/practice.md](./deployment/practice.md)
- [service/practice.md](./service/practice.md)
- [configmap/practice.md](./configmap/practice.md)
- [secret/practice.md](./secret/practice.md)
- [volume/practice.md](./volume/practice.md)
- [namespace/practice.md](./namespace/practice.md)

## How To Study

- 각 문서의 예제를 직접 실행한다.
- `notes.md`를 먼저 보고 `practice.md`는 답을 보지 않고 풀어본다.
- `--dry-run=client -o yaml`로 YAML 뼈대를 먼저 만든다.
- 생성, 수정, 조회, 삭제를 모두 한 번씩 해본다.
- 마지막의 practice 섹션은 직접 답을 만들고 검증한다.

## Assumption

로컬 실습 환경은 `kind` 기준으로 생각하면 충분하다.
다만 문서의 대부분은 일반적인 Kubernetes 환경에서도 그대로 통한다.
