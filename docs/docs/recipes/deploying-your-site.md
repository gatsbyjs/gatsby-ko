---
title: "Recipes: Deploying Your Site"
tableOfContentsDepth: 1
---

쇼타임! 사이트가 마음에 드시나요? 배포 할 준비가 되었습니다!

## 배포 준비

### 사전준비

- [Gatsby 사이트](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli) 설치

### 수행 절차

1. 개발 서버가 실행 중이면 중지하세요 (대부분의 경우 명령줄에서 `Ctrl + C`)

2. 루트 디렉토리 (`/`)인 표준 사이트 경로에 대해서는 명령줄에서 Gatsby CLI를 사용하여 `gatsby build`를 실행하세요. 빌드 된 파일은 `public` 폴더에 생성됩니다.

```shell
gatsby build
```

3. `/` 이외의 사이트 경로 (예 :`/site-name/`)를 포함 시키려면 `gatsby-config.js`에 다음을 추가하고 `yourpathprefix`를 원하는 경로로 바꿔주세요:

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

이렇게 하는데에는 몇 가지 이유가 있습니다. 예를 들어 Gatsby로 만들어지지 않은 다른 사이트 도메인 위에 Gatsby로 구축 된 블로그를 호스팅 할 수 있습니다. 기본 사이트는 `example.com`으로 연결되며 Gatsby 사이트는 `example.com/blog` 와 같은 접두사가 붙은 경로로 서비스 될 수 있습니다.

4. `gatsby-config.js`에 경로 접두사가 설정되어 있으면 `--prefix-paths` 플래그와 함께 `gatsby build`를 실행하여 모든 Gatsby 사이트 URL과 `<Link>` 태그의 시작 부분에 접두사를 자동으로 추가하세요.

```shell
gatsby build --prefix-paths
```

5. `gatsby build`를 실행할 때 `gatsby develop`때와 사이트가 동일하게 보이는지 확인하세요. 사이트를 빌드 할 때 `gatsby serve`를 실행하면 완성 된 제품을 실제로 배포하기 전에 테스트 (그리고 필요한 경우 디버그) 할 수 있습니다.

```shell
gatsby build && gatsby serve
```

### 추가 정보

- [튜토리얼 파트1](/tutorial/part-one/#deploying-a-gatsby-site)에서 예제 사이트 구축 및 배포에 대해 알아보세요
- [성능 최정화](/docs/performance/)에 대해 배우세요
- [다른 배포 관련 주제](/docs/preparing-for-deployment/)에 대해 읽어보세요
- 특정 호스팅 플랫폼 및 배포 방법에 대해서는 [배포 문서](/docs/deploying-and-hosting/)를 확인하세요

## Netlify에 배포

[`netlify-cli`](https://www.netlify.com/docs/cli/)를 사용하여 명령줄 인터페이스를 벗어나지 않고 Gatsby 애플리케이션을 배포하세요.

### 사전준비

- 단일 `index.js` 컴포넌트가 있는 [Gatsby 사이트](/docs/quick-start)
- [netlify-cli](https://www.npmjs.com/package/netlify-cli) 패키지 설치
- [Gatsby CLI](/docs/gatsby-cli) 설치

### 수행 절차

1. `gatsby build`를 사용하여 gatsby 애플리케이션을 빌드하세요.

2. `netlify login`을 사용하여 Netlify에 로그인하세요.

3. `netlify init` 명령을 실행하세요. "신규 사이트 생성 및 구성"옵션을 선택하세요.

4. 원하는 경우 사용자 정의 웹 사이트 이름을 선택하거나 Enter 키를 눌러 임의의 웹 사이트 이름을 받으세요.

5. [팀](https://www.netlify.com/docs/teams/)을 선택하세요.

6. 배포 경로를 `public/`으로 변경하세요.

7. `netlify deploy --prod`를 사용하여 프로덕션에 배포하기 전에 모든 것이 잘 보이는지 확인하세요.

### 추가 정보

- [Netlify에서 호스팅](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

## ZEIT Now에 배포

[Now CLI](https://zeit.co/download)를 사용하여 명령줄 인터페이스를 벗어나지 않고 Gatsby 애플리케이션을 배포하세요.

### 사전준비

- [ZEIT Now](https://zeit.co/signup) 계정
- 단일 `index.js` 컴포넌트가 있는 [Gatsby 사이트](/docs/quick-start)
- [Now CLI](https://zeit.co/download) 패키지 설치
- [Gatsby CLI](/docs/gatsby-cli) 설치

### 수행 절차

1. `now login`을 사용하여 Now CLI에 로그인

2. 터미널에서 Gatsby.js 애플리케이션 디렉토리로 이동하세요.

3. `now`를 실행하여 배포하세요

### 추가 정보

- [ZEIT Now에 배포하기](/docs/deploying-to-zeit-now/)
