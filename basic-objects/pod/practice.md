# Pod Practice

## Problem 1

`nginx:stable` 이미지를 사용하는 Pod `web`을 생성하라.

## Problem 2

Pod `web`에 `app=web` 라벨을 설정하라.

## Problem 3

현재 실행 중인 Pod `web`의 IP를 확인하라.

## Problem 4

존재하지 않는 이미지 `nginx:notfound`를 사용하는 Pod `broken-web`을 생성하라.

## Problem 5

`broken-web` Pod가 정상적으로 뜨지 않는 원인을 `kubectl describe`로 확인하고 설명하라.

## Problem 6

`kubectl run`이 아니라 YAML 파일을 생성해서 같은 Pod `web-yaml`을 만들어라.

## Problem 7

실습이 끝난 뒤 `web`, `broken-web`, `web-yaml` Pod를 모두 삭제하라.
