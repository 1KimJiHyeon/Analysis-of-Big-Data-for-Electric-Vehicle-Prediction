# 20Survey Project 

Table of Contents
---------------------

 * Requirements
 * Installation
 * Configuration

---

Environments
------------

| Node.js | npm |nvm|
| ------ | ------ |-----|
| v12.18.2 | v6.14.5 |v0.35.1|

| mysql | sequelize |
|-----|-----|
|v8.0.20|v6.3|

---

## Installation

- 아래 모든 `코드`는 시작하기 전에 필요합니다.

### Clone

-  `https://gitlab.com/billiamchoi4u/20survey`에서 gitclone을  받습니다.

### Setup
#### Install Node Packages
- 아래 코드를 터미널에 입력하여 npm을 사용하여 필요한 node package를 설치합니다

```shell
$ npm install
```
#### Configuration Setting 
- config 폴더 안에 config-sample.json을 config.json으로 이름을 변경 후 저장합니다.

- 개발 환경에 맞게 database config 및 redis config를 아래 Database config와 Redis config를 따라 변경합니다. (config.json의 YOUR-ROOT-PASSWORD를 자신의 mysql 루트 계정의 비밀번호로 변경)

- mysql 루트 계정에 비밀번호가 존재하지 않으면 "YOUR-ROOT-PASSWORD"를 null로 변경합니다.
(디폴트 redisHost는 127.0.0.1이며 redisPort는 6379)

- 만약 개발환경에 따라 redisHost나 Port를 변경하려면 config.json의 redisHost, redisPort를 변경하면 됩니다.

#### Database Setting
- 아래 코드를 터미널에 입력하여 config.json에 나와있는데로 mysql에 데이터베이스를 생성합니다.
```shell
$ npx sequelize db:create
```
- database 를 migrate 하기 위하여 아래 코드를 터미널에 입력합니다.
```shell
$ npm run migrate
```
- database seed를 삽입하기 위하여 아래 코드를 터미널에 입력합니다.
```shell
$ npm run seed:insert
```
#### Start Redis Server (For MAC)
- redis server를 실행시키기 위하여 아래 코드를 터미널에 입력합니다. 
```shell
$ brew install redis$ brew services start redis
```

### Start Server
#### Database Delete Seed
- database seed를 삭제할 경우 아래 코드를 터미널에 입력합니다.
```shell
$ npm run seed:delete
```

---

## 인수인계 사항
### 데모 시연
- 일정 : 11월 20일
- 방법 : 온라인 줌 회의 (클라이언트 쪽에선 유주영씨 외 교수님 2~3분 동시 참가 예정)
#### - 시나리오
1. 주어진 문항에 따라 설문지를 미리 만들어놓은 후,
2. 데모시연 때 학생 계정으로 로그인하여 설문 문항을 작성하고 제출하는 과정
3. 교수 계정으로 로그인시 자신의 강좌에 한해서만 설문 결과를 설문 결과 페이지에서 확인 가능한 것 (엑셀 파일 다운은 X)
4. admin 계정에서 설문 결과를 각 강좌별로 엑셀 파일로 내려 받을 수 있는 것까지 시연. ex) 구글 폼
#### - 특이사항
1. 설문지 요구사항
   * 설문 문항은 총 30문항으로 구성 (또래지명 8문항, Likert 12문항, Grit 12문항)
   * 설문 문항은 클라이언트에서 넘겨 준 Demodata.xlsx 내용 그대로 작성하며, Visual Groups 항목에 - 표시가 되어 있는 부분은 생략. (Demodata.xlsx : https://drive.google.com/drive/u/0/folders/1x4sbcZt_H2_1yKX30N9N6ynnb0eQWp8d)
   * Grit 문항의 경우 reverse 여부를 반드시 반영하여야 하고, 역산 값이 제대로 들어가는 부분을 데모시연 시 클라이언트에게 따로 확인 해 달라는 요청 있음. (역산이 반영 된 문항은 점수가 거꾸로 반영되어 들어간다는 것을 실시간으로 DB에서 직접 확인시켜 달라는 요청)
   * 또래지명 보기는 매트릭스 형태의 UI로 구성하고, 권고사항은 한 페이지에 3X3으로 총 9명의 이름이 보이게 하는 것이나, 꼭 3x3으로 구성 될 필요는 없고 가독성이 좋으면 됨. 유저 입장에서 한 페이지에 너무 많은 이름이 보이면 선택이 불편해지고 설문 결과의 신뢰도가 낮아지기에 가독성을 유의 해 달라는 클라이언트의 요청사항이 있었음.
   * 또래지명 문항의 보기는 매트릭스 형태의 UI, 페이지네이션, 검색창 기능 구현하고 다른 문항들과 페이지를 다르게 나눌 것
   * 설문지에 지문 (각각의 설문 문항보다 상위 항목)이 들어갈 수 있도록 구성 해 달라는 요청이 있어, 현재 DB에 ‘section’ 테이블을 추가한 뒤 ERD에서 forms 테이블과 questions 테이블 사이에 위치시켜야 함. 또한 DB 스키마 변경에 관련 된 라우터, 컨트롤러 등 코드 수정도 같이 이루어져야 함.
   * Likert 문항은 보기가 1에서 5까지.
   * Demodata.xlsx 에 표기 된 ‘설문’ sheet 기준으로 설문지 단계에서는 Section 과 Question 이 노출되어야 하며, 설문 결과 페이지 (결과 엑셀 파일X) 에서는 Description 과 VisualGroup 이 함께 묶여서 보여야 함.
   * 설문 항목에 성별 입력란(객관식)과, 학점입력란 (주관식) 추가 필요. 학점입력란의 경우엔 관련 DB 테이블 추가 필요.

## Elastic Beanstalk Deploy 방법
### EB CLI를 사용하여 CodeCommit 리포지토리 생성
- 프로젝트 폴더에서 eb init를 실행합니다. eb init를 사용하여 이전에 프로젝트를 구성한 경우에도 이를 다시 실행하여 CodeCommit를 구성할 수 있습니다.

```shell
~/my-app$ eb init
```

- Create new Repository(새 리포지토리 생성)를 선택합니다.

```shell
Select a repository
1) my-repo
2) [ Create new Repository ]
(default is 2): 2
```

- 리포지토리 이름을 입력하거나 입력을 눌러 기본 이름을 적용합니다.

```shell
Enter Repository Name
(default is "codecommit-origin"): my-app
Successfully created repository: my-app
```

- 커밋에 대해 기존 브랜치를 선택하거나, EB CLI를 사용하여 새 브랜치를 만듭니다.

```shell
Enter Branch Name
***** Must have at least one commit to create a new branch with CodeCommit *****
(default is "master"): ENTER
Successfully created branch: master
```

### CodeCommit 리포지토리에서 배포
#### EB CLI 리포지토리에서 CodeCommit를 구성하면 EB CLI는 리포지토리의 내용을 사용하여 소스 번들을 생성합니다. eb deploy 또는 eb create를 실행하면 EB CLI는 새 커밋을 푸시하고 브랜치의 HEAD 개정을 사용하여 환경의 EC2 인스턴스에 배포하는 아카이브를 생성합니다.

- eb create를 사용하여 새 환경을 생성합니다.

```shell
~/my-app$ eb create my-app-env
Starting environment deployment via CodeCommit
--- Waiting for application versions to be pre-processed ---
Finished processing application version app-ac1ea-161010_201918
Setting up default branch
Environment details for: my-app-env
  Application name: my-app
  Region: us-east-2
  Deployed Version: app-ac1ea-161010_201918
  Environment ID: e-pm5mvvkfnd
  Platform: 64bit Amazon Linux 2016.03 v2.1.6 running Java 8
  Tier: WebServer-Standard
  CNAME: UNKNOWN
  Updated: 2016-10-10 20:20:29.725000+00:00
Printing Status:
INFO: createEnvironment is starting.
...
```

- 새 로컬 커밋이 있는 경우 eb deploy를 사용하여 커밋을 푸시하고 환경에 배포합니다.

```shell
~/my-app$ eb deploy
Starting environment deployment via CodeCommit
INFO: Environment update is starting.
INFO: Deploying new version to instance(s).
INFO: New application version was deployed to running EC2 instances.
INFO: Environment update completed successfully.
```