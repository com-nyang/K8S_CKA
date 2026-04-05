# Deployment Practice

## Problem 1

`nginx:1.25` 이미지를 사용하는 Deployment `web`을 생성하라.

## Problem 2

Deployment `web`의 replica 수를 3으로 변경하라.

## Problem 3

Deployment `web`의 Pod 템플릿 라벨에 `app=web`을 설정하라.

## Problem 4

Deployment `web`의 이미지를 `nginx:1.26`으로 변경하라.

## Problem 5

롤아웃 상태를 확인하고, 변경 이력을 조회하라.

## Problem 6

이전 버전으로 롤백하라.

## Problem 7

`kubectl create deployment ... --dry-run=client -o yaml` 방식으로 `api` Deployment YAML을 만들고 적용하라.
