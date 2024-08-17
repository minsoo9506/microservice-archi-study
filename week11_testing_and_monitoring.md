# BM
# 7. Build
## A Brief Introduction to Continous Integration
- core goal is to keep everyone in sync with each other
  - newly checked-in code properly integrates with existing code
### Are You Really Doing CI?
- Jenkins 같은 CI 툴을 사용한다고해서 CI 하고 있다고 확신할 수 없다.
  - Do you check in to mainline once per day?
  - Do you have a suite of tests to validate your changes?
  - When the build is broken, is it the #1 priority of the team to fix it?
### Branching Models
- Integrate early, and integrate often. Avoid the use of long-lived branches for feature development, and consider trunk-based development instead.
- short-lived branches; small, readable patches and automatic testing of changes make everyone more productive.

## Build Pipelines and Continous Delivery
- Continuous Delivery Versus Continuous Deployment
  - Continuous Delivery : each check-in is treated as a release candidate, and we can assess the quality of each release candidate to decide if it’s ready to be deployed.
  - Continuous Deployment 는 이를 자동화한 개념

### Tooling
### Trade-Offs and Environments
### Artifact Creation

## Mapping Source Code and Builds to Microservices
### One Giant Repo, One Giant Build
- 피해야 한다.
- 작은 일부분만 수정되어도 전체가 테스트, 빌드 과정을 거쳐야해서 시간도 오래걸린다.
- 다른 service 는 배포되지 않아야 하는 경우가 있을 수 있고 이를 다 고려해야 한다.
- 내 코드로 인해 빌드가 실패하면 이를 fix 할 때까지 다른 service 는 개발하기 어렵다.

### Pattern: One Repository per Microservice (Multirepo)
- 위처럼 하나로 전부 하는 경우 발생하는 단점들이 사라진다.
- 하지만 이또한 단점이 없는 것은 아니다.
- 여러 repo 를 개발해야하고 서로 dependency 가 걸려있는 것도 주의해야한다.

### Pattern: Monorepo
- 구글에서도 쓰는 방법 (https://cacm.acm.org/research/why-google-stores-billions-of-lines-of-code-in-a-single-repository/)
- 처음 시작으로는 폴더로 service 를 구분하는 것으로 시작할 수 있다.
- 언제 사용?
  - 잘하는 빅테크 회사
  - 10~20 정도 개발자가 있는 작은 회사: 책임&권한의 범위를 비교적 잘 나눌 수 있는 경우
  - 그 중간은 성장통을 겪을 수 있음

### Which Approach Would I Use?
- 상황에 따라 다르니 잘 고려하길

# Deployment

## From Logical to Physical
### Multiple Instances
- 여러개의 instance 를 통해 load 를 잘 버티고 robust 하게 해준다.
- 다른 datacenter 에 있는 것들도 고려해야 한다.

### The Database
### Environments

## Principles of Microservice Deployment
### Isolated Execution
### Focus on Automation
### Infrastructure as Code (IAC)
### Zero-Downtime Deployment
### Desired State Management

## Deployment Options
### Physical Machines
### Virtual Machines
### Containers
### Application Containers
### Platform as a Service (PaaS)
### Function as a Service (FaaS)

## Which Deployment Option Is Right for You?

## Kubernetes and Container Orchestration
- k8s 는 크게 2가지: node, control plane
### The Case for Container Orchestration
### A Simplified View of Kubernetes Concepts
### Multitenancy and Federation
### The Cloud Native Computing Federation
### Platforms and Portablility
### Helm, Operators, and CRDs, Oh My!
### And Knative
### The Future
### Should You Use It?

## Progressive Delivery
- high-performing companies deploy more frequently and at the same time have much lower change failure rates.
### Separating Deployment from Release
### On to Progressive Delivery
### Feature Toggles
- hide deployed functionality behind a toggle that can be used to switch functionality off or on
### Canary Release
- limited subset of our customers see new functionality
### Parallel Run
- run two different implementations of the same functionality side by side, and send a request to the functionality to both implementations