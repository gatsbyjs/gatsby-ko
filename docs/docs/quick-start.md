---
title: 빠른 시작
---

이 빠른 시작은 중급부터 고급 개발자를 위한 가이드입니다. Gatsby에 대한 좀 더 쉬운 소개가 필요하면 [튜토리얼을 확인하세요](/tutorial/)!

## Gatsby CLI 사용하기

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**주의**: 이 영상에서는 npm 패키지를 설치 없이 실행해주는 도구인 `npx`를 사용합니다. `npx gatsby new`
명령은 gatsby-cli를 설치한 후 `gatsby new` 명령을 실행하는 것과 동일합니다.

<<<<<<< HEAD
### Gatsby CLI 설치
=======
### Install the Gatsby CLI
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
npm install -g gatsby-cli
```

<<<<<<< HEAD
### 새로운 프로젝트 생성
=======
> The above command installs Gatsby CLI globally on your machine.

### Create a new site
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby new gatsby-site
```

<<<<<<< HEAD
### 프로젝트 디렉토리로 이동
=======
### Change directories into site folder
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
cd gatsby-site
```

<<<<<<< HEAD
### 개발 서버 실행
=======
### Start development server
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby develop
```

<<<<<<< HEAD
Gatsby는 기본 설정으로 `localhost:8000`을 통해 접근할 수 있는 hot-reloading 개발 서버를 시작할 것입니다.
=======
Gatsby will start a hot-reloading development environment accessible by default at `http://localhost:8000`.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

`src/pages` 안의 자바스크립트 파일을 수정해 보세요. 변경 사항은 브라우저에서 실시간으로 갱신될 것입니다.

<<<<<<< HEAD
### production 빌드 생성
=======
### Create a production build
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby build
```

Gatsby는 정적 HTML 및 경로별 JavaScript 코드 번들을 생성하며 사이트에 최적화된 production 빌드를 수행합니다.

<<<<<<< HEAD
### 로컬 환경에서 production 빌드 후 테스트
=======
### Serve the production build locally
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby serve
```

Gatsby는 빌드된 사이트를 테스트하기 위해 로컬 HTML 서버를 시작합니다. 이 명령을 실행 하기 전 `gatsby build`를 먼저 수행 하는 것을 잊지마세요.

### CLI 명령어에 대한 가이드 접근

`gatsby --help`를 실행하면 CLI 명령어에 대한 자세한 문서를 볼 수 있습니다.

특정 명령어를 위해서는 `gatsby COMMAND_NAME --help`를 실행하세요. 예. `gatsby new --help`

Gatsby CLI에 대한 더 자세한 정보는 [CLI 레퍼런스](/docs/gatsby-cli/) 문서를 확인하세요.
