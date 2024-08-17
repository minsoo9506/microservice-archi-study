# BM
# Ch 15. Organization Structures
## Loosely Coupled Organizations
- Loosely Coupled Organizations 이 결과를 잘 낸다.
  - power and accountability are decentralized

## Conway’s Law
### Evidence
#### Loosely and tightly coupled organizations
- (실험으로 확인) the more loosely coupled organizations actually created more modular, less coupled systems, whereas the more tightly coupled organization’s software was less modularized.
#### Netflix and Amazon
- 8-10 명 규모의 작고 독립적인 팀 구성
- 각 팀이 본인들의 service 를 잘 전담, 독립적 운영

## Team Size
- 9명 이상이 되면 productivity 가 떨어진다.

## Understanding Conway’s Law

## Small Teams, Large Organization

## On Autonomy
- 자율성 가지는게 좋다.

## Strong Versus Collective Ownership
- two primary forms of code ownership
  - Strong ownership
    - ms 마다 담당하는 팀이 있고 해당 ms 와 관련한 작업을 하기 위해서는 해당 팀의 컨펌이 필요하다.
  - Collective ownership
    - 아무 팀이나 ms 에 변화를 줄 수 있다. 조심해서 작업하면.

### Strong Ownership
- full control of what code changes are made
- 전담을 하고 독립적이고 autonomy 도 강하게 갖는다. 물론 책임감도 함께.
- Amazon 이 이런 스타일이라고 한다.

### Collective Ownership
- requires a high degree of coordination among individuals and among the teams those individuals are in
  - increased degree of coupling at an organizational level
- 개발자 인원이 적고 한 팀이면 몰라도 그 수가 늘어나면 collective ownership 은 ms 아키텍처의 장점을 활용하기 어렵다.

## Enabling Teams
## Shared Microservices
## Internal Open Source
- 내부 오픈소스도 많이 사용한다. 이를 유지하는 것은 일반 오픈소스와 비슷하게 유지보수 된다.
- 이때 core committer 들이 있고 이들이 중심적인 역할을 해야한다.

## Pluggable, Modular Microservices
## The Orphaned Service
## Case Study: realestate.com.au
## Geogrphical Distrubution
## Conway's Law in Reverse
## People

# FSA
# Ch 22. Making Teams Effective
## Team Boundaries
- One of the roles of a software architect is to create and communicate the constraints, or the box, in which developers can implement the architecture.
## Architect Personalities
### Control Freak
- 너무 디테일한 부분까지 컨트롤하려고 한다.
- 상황에 따라 tight 할 필요도 있지만 비효율적이지 않도록 유의해야한다.
### Armchair Architect
- 반대로 너무 loose 하고 실무에서 멀어진 상태
### Effective Architect
## How Much Control?
- 5가지 기준에 따라 얼마다 control 할지 파악할 수 있다.
  - Team familiarity
    - 팀원끼리 잘 알수록 less control
  - Team size
    - 팀 크기가 클수록 more control
  - Overall experience
    - 주니어가 많을수록 more control
  - Project complexity
    - 복잡할수록 more control
  - Project duration
    - 프로젝트 기간이 길수록 more control
## Team Warning Signs
- Process loss
  - process loss = group potential - actual productivity
  - 팀의 크기가 커질수록 프로젝트 진행속도는 느려진다.
  - process loss 의 indicator 중 하나는 frequent merge conflicts when pushing code to a repository 이다.
  - 팀 사이즈를 정해야할 때, 고려하면 좋다.
- Pluralistic ignorance
  - 팀 사이즈가 너무 크면 각 개인들이 의견을 내기보다 그냥 따라가게 된다.
  - 그래서 아키텍터가 평소에 팀원들을 잘 살펴야하고 편하게 의견을 낼 수 있도록 해야한다.
- Diffusion of responsibility
  - 책임이 누구에게 있는지 모르게 되는 경우 팀 사이즈가 커서 그럴 수 있다. 
## Leveraging Checklists
- checklist 를 사용하는 것은 아주 좋다.
- 하지만 개발자들이 비행기 조종사는 아니므로 knowing when to leverage checklists and when not to 가 중요하다.
- 개발자를 위한 효율적인 checklist 3가지
  - Developer Code Comopletion Checklist
  - Unit and Functional Testing Checklist
  - Software Release Checklist
### Developer Code Comopletion Checklist
- 예시
  - Coding and formatting standards not included in automated tools
  - Frequently overlooked items (such as absorbed exceptions)
  - Project-specific standards
  - Special team instructions or procedures
### Unit and Functional Testing Checklist
- 예시
  - Special characters in text and numeric fields
  - Minimum and maximum value ranges
  - Unusual and extreme test cases
  - Missing fields
### Software Release Checklist
- 가장 조심해야한다.
- 예시
  - Configuration changes in servers or external configuration servers
  - Third-party libraries added to the project (JAR, DLL, etc.)
  - Database updates and corresponding database migration scripts
## Providing Guidance