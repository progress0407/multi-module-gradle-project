# multi-module-gradle-project

## 모듈 설명

- `app`: SpringApplication 실행 파일 위치

- `api`: controller 파일 위치

- `domain`: service, entity, repository 위치

- `common`: MSA간 사용되는 공통 코드, DTO 위치

- `client`: FeignClient, RabbitMQ 위치

## 프로젝트 구조도

- `root`
  - `app`
    - `app-a`
    - `app-b`
    - `app-c`
    - ...
  - `api`
    - `api-a`
    - `api-b`
    - ...
  - `domain`
    - ...
  - `common`
    - ...
  - `client`
    - ...

## 의존 관계도

`app` -> `api` -> `domain` -> `common` -> `client`


## 문제 상황

단순한 1뎁스 모듈 구조가 아닌, 2뎁스 이상의 구조여서 그런지는 불분명하다. 

확실한 증상은 현재 구조에서 아래 이상 증상들이 발견된다..

### 1. java.lang.NoClassDefFoundError 에러

분명히 하위 모듈의 java파일을 정상 import 했음에도 불구하고, 실행시에 이와 같은 에러 발생
 
<img src="https://github.com/progress0407/progress0407/assets/66164361/09457a57-2684-454f-af48-59e411cff4a1" width="600px">

### 2. app 패키지에 SpringBootTest 작성시에 에러
 classLoader가 파일을 찾을 수 없는 에러 발생

### 3. 혼자서 계속되는 gradle

(git 브랜치 생성 후부터 발견된 에러) 빌드/실행을 하지 않았음에도 불구하고 계속 gradle이 빌드를 시도함

### 4. 빌드 캐싱 문제

특정 모듈의 `build` 파일을 지우지 않으면 정상적으로 빌드되지 않음

2뎁스(예: `cleint-a`)에 위치한 모듈이 아닌 1뎁스의 build 디렉터리(예: `client`)를 지워야 정상 동작

이 현상은 필자의 1뎁스 멀티 모듈 프로젝트에서는 발견되지 않은 문제...