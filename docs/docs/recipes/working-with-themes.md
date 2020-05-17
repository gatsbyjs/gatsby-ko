---
title: "Recipes: Working with Themes"
tableOfContentsDepth: 1
---

[Gatsby 테마](/docs/themes/what-are-gatsby-themes)는 Gatsby 구성(공유되는 기능, 데이터 소싱, 디자인)을 설치 가능한 패키지로 추상화합니다. 이는 구성 및 기능이 프로젝트에 직접 작성되지 않고 버저닝되고 중앙 관리되며 종속성으로 설치됨을 의미합니다. 테마를 매끄럽게 업데이트하고, 여러 테마들을 조합하며, 호환되는 테마를 다른 테마로 교체 할 수도 있습니다.

## 테마를 사용하여 새 사이트 생성하기

프로젝트에서 사용할 테마를 찾았습니까? 대박! 아래 단계에 따라 사용하도록 구성 할 수 있습니다.

> 더 많은 테마 옵션을 보려면 [테마 목록](https://www.npmjs.com/search?q=gatsby-theme)을 확인하세요.

### 사전준비

- [Gatsby CLI](/docs/gatsby-cli) 설치

### 수행 절차

1. Gatsby 사이트를 생성합니다.

```shell
gatsby new {your-project-name}
```

2. 디렉토리 변경 및 테마 설치

이 예에서 우리의 테마는`gatsby-theme-blog`입니다. 이것을 원하는 테마 이름으로 바꿔서 진행 할 수 있습니다.

```shell
cd {your-project-name}
npm install gatsby-theme-blog
```

3. `gatsby.config.js`에 테마를 추가하세요

사용 중인 테마의 README에 있는 지침에 따라 필요한 구성을 결정하세요.

```shell
module.exports = {
  plugins: [
    {
      resolve: `gatsby-theme-blog`,
      options: {
        /*
        - basePath defaults to `/`
        - contentPath defaults to `content/posts`
        - assetPath defaults to `content/assets`
        - mdx defaults to `true`
        */
        basePath: `/blog`,
      },
    },
  ],
}
```

4. `gatsby develop`를 실행하여 `http://localhost:8000/{basePath}`에서 테마를 확인하세요.

> 추가적인 테마 커스터마이징 방법을 알아 보려면 [Gatsby-theme-blog Documentation](https://www.npmjs.com/package/gatsby-theme-blog)를 확인하세요.

### 추가 정보

- 추가적인 테마 커스터마이징 방법을 알아 보려면 [Gatsby 테마 섀도잉에 대한 문서](https://www.gatsbyjs.org/docs/themes/shadowing/)를 확인하세요
- 프로젝트에서 [여러개의 테마를 사용](https://www.gatsbyjs.org/docs/themes/using-multiple-gatsby-themes/) 할 수도 있습니다.

## 테마 스타터를 사용하여 새 사이트 생성하기

테마가 구성된 스타터를 기반으로 사이트를 만드는 것은 테마가 **구성되지 않은** 스타터를 기반으로 사이트를 만드는 것과 동일한 프로세스를 따릅니다. 이 예에서는 [새 사이트를 만들기 위한 공식 Gatsby 블로그 스타터](https://github.com/gatsbyjs/gatsby-starter-blog-theme)를 사용할 수 있습니다.

### 사전준비

- [Gatsby CLI](/docs/gatsby-cli) 설치를 확인하세요

### 수행 절차

1. 블로그 테마 스타터를 기반으로 새 사이트를 생성하세요:

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. 새 사이트를 실행하세요:

```shell
cd {your-project-name}
gatsby develop
```

### 추가 정보

- [짧은 개념 가이드](/docs/themes/using-a-gatsby-theme) 또는 보다 상세한 [단계별 튜토리얼](/tutorial/using-a-theme)에서 기존 Gatsby 테마를 사용하는 방법을 배우세요.

## 새 테마 만들기

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

### 사전준비

- [Gatsby CLI](/docs/gatsby-cli) 설치
- [Yarn](https://yarnpkg.com/lang/en/docs/install) 설치

### 수행 절차

1. [Gatsby 테마 작업 환경 스타터](https://github.com/gatsbyjs/gatsby-starter-theme-workspace)를 사용하여 새 테마 워크스페이스를 생성하세요:

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. 워크스페이스 안의 example 사이트를 실행하세요:

```shell
yarn workspace example develop
```

### 추가 정보

- Gatsby 테마 작업 환경 스타터에 대한 [더 자세한 가이드](/docs/themes/building-themes/)를 확인하세요.
- [Egghead의 Gatsby 테마 작성 영상 강좌](https://egghead.io/courses/gatsby-theme-authoring) 또는 [영상 강좌의 문서 버전 튜토리얼](/tutorial/building-a-theme)에서 자신만의 테마 만드는 방법을 배우세요.
