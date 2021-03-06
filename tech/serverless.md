[comment]: # (This presentation was made with markdown-slides)
[comment]: # (This is a CommonMark compliant comment. It will not be included in the presentation.)
[comment]: # (Compile this presentation with the command below)
[comment]: # (mdslides serverless.md --include serverless-resource)

[comment]: # (Set the theme:)
[comment]: # (THEME = white)
[comment]: # (CODE_THEME = base16/zenburn)
[comment]: # (The list of themes is at https://revealjs.com/themes/)
[comment]: # (The list of code themes is at https://highlightjs.org/)

[comment]: # "You can also use quotes instead of parenthesis"
[comment]: # "THEME = white"
[comment]: # "font-size = 32px"

[comment]: # (Pass optional settings to reveal.js:)
[comment]: # (controls: true)
[comment]: # (keyboard: true)
[comment]: # (markdown: { smartypants: true })
[comment]: # (hash: false)
[comment]: # (respondToHashChanges: false)
[comment]: # (Other settings are documented at https://revealjs.com/config/)

Harry | Shopl&Company

# Serverless
## AWS Lambda 관리

[comment]: # (!!! data-background-video="sentry-resource/video.mp4", data-background-video-loop data-background-video-muted data-background-opacity="0.2")

![AWS Lambda](https://incentius.com/img/tech/cloud/lambda.png)

AWS Lambda는 이벤트에 대한 응답으로 코드를 실행하고 자동으로 기본 컴퓨팅 리소스를 관리하는 이벤트 기반 **서버리스 컴퓨팅 플랫폼**

[comment]: # (!!!)

### Serverless 아키텍처란?

![Serverless](https://acg-wordpress-content-production.s3.us-west-2.amazonaws.com/app/uploads/2020/12/1_jKQenYyeQLoqnZcpjPGexA.gif)

개발자가 기본 인프라를 유지 관리할 필요 없이 애플리케이션과 서비스를 구축하고 실행할 수 있도록 하는 소프트웨어 설계 접근 방식

[comment]: # (|||)

- 서버를 관리하려면 상당한 노력과 리소스가 필요
- 팀은 서버 하드웨어를 유지 관리하고 소프트웨어 및 보안 업그레이드를 관리하며 오류 발생 시 백업을 생성해야 함
- 개발자는 서버리스 아키텍처를 채택하여 이러한 작업을 타사 공급업체에 위임하고 코드 작성에 집중할 수 있다.

[comment]: # (|||)

### Serverless 아키텍처는 어떻게 작동하는가?

1. 개발자가 기능을 작성함: 이 기능은 종종 애플리케이션 코드 내에서 특정 역할이나 목적을 수행합니다.
2. 개발자가 이벤트를 정의: 이벤트는 클라우드 서비스 공급자가 기능을 실행하도록 트리거하는 것. HTTP 요청이 일반적인 종류의 이벤트
3. 이벤트가 트리거 됨: 정의된 이벤트가 HTTP 요청인 경우 클릭 또는 기타 동등한 작업으로 사용자가 이벤트를 트리거한다.
4. 함수가 실행됨: 클라우드 서비스 공급자는 함수의 인스턴스가 실행 중인 서버에서 이미 실행 중인지 여부를 결정합니다. 그렇지 않은 경우 새 서버를 시작하여 기능을 실행합니다.
5. 결과가 클라이언트로 전송됨: 사용자는 애플리케이션 내에서 실행된 기능의 출력을 봅니다.

[comment]: # (|||)

### 컨테이너와 비교

||컨테이너|서버리스|
|------|---|---|
|호스팅|최신 Linux, 일부 Windows|대부분 클라우드 서비스|
|로컬실행|손쉽게 실행|클라우드 외부 실행 어려움|
|비용|오픈소스|사용량 기준으로 요금 부과|
|상태유지|기본적으로 상태 비저장|
|유효성|장기간 작동 가능|일반적으로 짧은 시간동안 작동하고 이벤트 처리 즉시 종료됨|

[comment]: # (|||)

### Serverless 아카텍처의 장점

- 확장성: 대부분의 서버리스 함수 인스턴스는 트래픽 변동에 따라 자동으로 생성되거나 소멸
- 호스팅 솔루션의 비용 친화성: 호출 양에 따라 요금을 청구하므로 사용되지 않는 시간에 대해 비용을 지불할 필요 없음
- 민첩성: 빠른 개발, 짧은 릴리스 주기
- 재해 대응 시간: 서버리스 시스템에서 정전 또는 고장으로부터의 복구는 사실상 즉각적으로 이루어짐
- 생산성 : 엔지니어는 서버를 관리할 필요 없이 간단히 코드에만 집중할 수 있음

[comment]: # (|||)

### Serverless 아카텍처의 단점

- 제어 부족 : 소프트웨어 스택에 대한 제어를 클라우드 제공업체에 의존해야 함
- 아키텍처 복잡성: 기능이 얼마나 세분화되어야 하는지에 대한 결정을 검토, 개발 및 테스트하는 데 시간이 걸 수 있다
- 콜드 스타트 : 비활성된 함수를 호출하면 서버 구동에서부터 코드 실행까지 시간이 걸림 
- 테스팅 : 통합 테스트는 서버리스 환경에서 수행하기 어렵다
- Vendor Lock-In : 클라우드 제공업체로부터 벗어날 수 없다

[comment]: # (!!!)

### Serverless에 적합한 작업

- 배치 작업
- 비동기 처리: 비디오 트랜스코딩 등 비하인드 작업 수행에 적합
- 단위 처리의 리소스가 큰 작업: 리포트 파일 생성 및 다운로드
- 동시에 수많은 업무를 분산해야 할 때: 많은 클라이언트를 대상으로 푸시 메시지 발송
- MSA RESTful API: 기능별 마이크로서비스를 구현하는데 적합

[comment]: # (!!!)

![Serverless Framework](https://miro.medium.com/max/877/1*BdKEE3815BcMklgX9jTjIw.png)

[comment]: # (!!!)

### Serverless Framework?

AWS Lambda와 같은 서비스를 사용하여 서버리스 애플리케이션을 빠르게 구축 및 배포하는 데 도움을 주는 오픈소스 프로젝트

[comment]: # (!!!)

### sls 작동과정 (Java)

1. 컴파일 및 업로드할 파일(jar)을 패키징하여 준비합니다.
2. AWS CloudFormation 템플릿을 생성하고 
3. 해당 템플릿에서 필요한 모든 관련 AWS 리소스가 생성됩니다.
4. 코드를 AWS에 업로드하고 스택 생성을 시작
5. AWS에서 서버리스 애플리케이션 생성이 마무리됨

[comment]: # (!!!)

### Serverless Framework 설치에서 배포까지

```sh
$ npm install -g serverless
$ serverless 
$ cd my-service-name 
$ sls --template-url=https://github.com/serverless/examples/tree/v3/...
$ serverless deploy
$ sls invoke -f hello --log
$ serverless remove
```

[공식문서](https://www.serverless.com/framework/docs/getting-started)

[comment]: # (!!!)

### shopl-fn 레파지토리

- 사내 AWS Lambda 프로젝트가 늘어나고 있음
- 단일 레파지토리 내에서 관리하기 위한 목적 
- Serverless Framework를 사용해 배포 및 인프라 관리 단순화
- 모든 프로젝트에 대해 동일한 배포 프로세스 적용

[comment]: # (!!!)

### 환율 조회 및 DB 입력 사례 연구

![환율 아키텍처](serverless-resource/exchangerate.png) <!-- .element: style="height:500px;" -->
[컨플루언스 문서](https://shoplworks.atlassian.net/wiki/spaces/S/pages/467402753)

[comment]: # (!!!)

### shopl-fn 배포 

```sh
$ sls deploy -s sss 
$ sls deploy function -f sqsReceiver  -s sss 
$ sls remove -s sss
```
