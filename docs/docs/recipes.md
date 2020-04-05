---
title: Recipes
tableOfContentsDepth: 2
---

<!-- Basic template for a Gatsby recipe:

## Task to accomplish.
1-2 sentences about it. The more concise and focused, the better!

### Prerequisites
- System/version requirements
- Everything necessary to set up the task
- Including setting up accounts at other sites, like Netlify
- See [docs templates](/docs/docs-templates/) for formatting tips

### Directions
Step-by-step directions. Each step should be repeatable and to-the-point. Anything not critical to the task should be omitted.

#### Live example (optional)
A live example may not be possible depending on the nature of the recipe, in which case it is fine to omit.

### Additional resources
- Tutorials
- Docs pages
- Plugin READMEs
- etc.

See [docs templates](/docs/docs-templates/) in the contributing docs for more help.
-->

[튜토리얼](/tutorial/)과 [문서](/docs) 사이의 무언가를 원하시나요? 여기 Gatsby 방식으로 만들 수 있게 도와주는 가이드 레시피가 있습니다.

## [1. 페이지와 레이아웃](/docs/recipes/pages-layouts)

Gatsby 웹사이트에 페이지를 추가하고, 페이지의 공통 요소를 위해 레이아웃을 사용하세요.

- [프로젝트 구조](/docs/recipes/pages-layouts#project-structure)
- [자동으로 페이지 생성하기](/docs/recipes/pages-layouts#creating-pages-automatically)
- [페이지 간 연결하기](/docs/recipes/pages-layouts#linking-between-pages)
- [레이아웃 컴포넌트 생성하기](/docs/recipes/pages-layouts#creating-a-layout-component)
- [createPage를 사용하여 프로그래밍으로 페이지 생성하기](/docs/recipes/pages-layouts#creating-pages-programmatically-with-createpage)

## [2. CSS로 스타일링](/docs/recipes/styling-css)

웹 사이트에 스타일을 추가하는 정말 많은 방법이 있습니다. Gatsby는 공식 및 커뮤니티 플러그인을 통해 거의 모든 옵션을 지원합니다.

- [Layout 컴포넌트 없이 전역 CSS 파일들 사용하기](/docs/recipes/styling-css#using-global-css-files-without-a-layout-component)
- [Layout 컴포넌트에서 전역 스타일 사용하기](/docs/recipes/styling-css#using-global-styles-in-a-layout-component)
- [Styled Components 사용하기](/docs/recipes/styling-css#using-styled-components)
- [CSS Modules 사용하기](/docs/recipes/styling-css#using-css-modules)
- [Sass/SCSS 사용하기](/docs/recipes/styling-css#using-sassscss)
- [로컬 폰트 파일 적용하기](/docs/recipes/styling-css#adding-a-local-font)
- [Emotion 사용하기](/docs/recipes/styling-css#using-emotion)
- [Google Fonts 사용하기](/docs/recipes/styling-css#using-google-fonts)

## [3. 스타터로 작업하기](/docs/recipes/working-with-starters)

[스타터](/docs/starters/)는 공식적으로 또는 커뮤니티에서 유지 보수되는 개츠비 사이트의 보일러 플레이트입니다.

- [스타터 사용하기](/docs/recipes/working-with-starters#using-a-starter)

## [4. 테마로 작업하기](/docs/recipes/working-with-themes)

Gatsby 테마는 웹사이트의 look-and-feel을 중앙에서 관리 할 수 있습니다. 테마를 매끄럽게 업데이트하고, 여러 테마들을 조합하며, 호환되는 테마를 다른 테마로 교체 할 수도 있습니다.

- [테마를 사용하여 새 사이트 생성하기](/docs/recipes/working-with-themes#creating-a-new-site-using-a-theme)
- [테마 스타터를 사용하여 새 사이트 생성하기](/docs/recipes/working-with-themes#creating-a-new-site-using-a-theme-starter)
- [새 테마 만들기](/docs/recipes/working-with-themes#building-a-new-theme)

## [5. 데이터 소싱하기](/docs/recipes/sourcing-data)

파일 시스템이나 데이터베이스와 같은 여러 위치의 데이터를 Gatsby 사이트로 가져옵니다.

- [GraphQL에 데이터 추가하기](/docs/recipes/sourcing-data#adding-data-to-graphql)
- [GraphQL을 사용하여 블로그 게시물 및 페이지를 위한 마크다운 데이터 소싱하기](/docs/recipes/sourcing-data#sourcing-markdown-data-for-blog-posts-and-pages-with-graphql)
- [WordPress로부터 소싱하기](/docs/recipes/sourcing-data#sourcing-from-wordpress)
- [Contentful 데이터 소싱하기](/docs/recipes/sourcing-data#sourcing-data-from-contentful)
- [외부 데이터 소싱으로 GraphQL없이 페이지 만들기](/docs/recipes/sourcing-data#pulling-data-from-an-external-source-and-creating-pages-without-graphql)
- [Drupal로 부터 콘텐츠 소싱하기](/docs/recipes/sourcing-data#sourcing-content-from-drupal)

## [6. 데이터 쿼리하기](/docs/recipes/querying-data)

Gatsby에서는 GraphQL 인터페이스만으로 모든 소스들의 데이터에 접근 할 수 있습니다.

- [페이지 쿼리로 데이터 쿼리하기](/docs/recipes/querying-data#querying-data-with-a-page-query)
- [StaticQuery 컴포넌트를 사용하여 데이터 쿼리하기](/docs/recipes/querying-data#querying-data-with-the-staticquery-component)
- [useStaticQuery 훅을 사용하여 데이터 쿼리하기](/docs/recipes/querying-data/#querying-data-with-the-usestaticquery-hook)
- [GraphQL 결과 제한하기](/docs/recipes/querying-data#limiting-with-graphql)
- [GraphQL 결과 정렬하기](/docs/recipes/querying-data#sorting-with-graphql)
- [GraphQL 결과 필터링하기](/docs/recipes/querying-data#filtering-with-graphql)
- [GraphQL 쿼리 별칭](/docs/recipes/querying-data#graphql-query-aliases)
- [GraphQL 쿼리 프래그먼트](/docs/recipes/querying-data#graphql-query-fragments)
- [fetch로 클라이언트 사이드에서 데이터 쿼리하기](/docs/recipes/querying-data#querying-data-client-side-with-fetch)

## [7. 이미지 작업](/docs/recipes/working-with-images)

정적 리소스로써 이미지를 액세스하거나 강력한 플러그인을 통해 이미지 최적화 프로세스를 자동화 하세요.

- [웹팩을 사용하여 컴포넌트에서 이미지 임포트하기](/docs/recipes/working-with-images#import-an-image-into-a-component-with-webpack)
- [static 폴더의 이미지 참조하기](/docs/recipes/working-with-images#reference-an-image-from-the-static-folder)
- [gatsby-image를 사용하여 로컬 이미지 최적화 및 쿼리하기](/docs/recipes/working-with-images#optimizing-and-querying-local-images-with-gatsby-image)
- [gatsby-image를 사용하여 post frontmatter의 이미지 최적화 및 쿼리하기](/docs/recipes/working-with-images#optimizing-and-querying-images-in-post-frontmatter-with-gatsby-image)

## [8. 데이터 변환](/docs/recipes/transforming-data)

Gatsby에서 데이터 변환은 플러그인 중심입니다. Transformer 플러그인은 소스 플러그인을 사용하여 가져온 데이터를 더 유용한 것으로 가공합니다 (예: JSON을 JavaScript 객체 등으로).

- [마크다운을 HTML로 변환](/docs/recipes/transforming-data#transforming-markdown-into-html)
- [GraphQL을 사용하여 이미지를 그레이 스케일로 변환하기](/docs/recipes/transforming-data#transforming-images-into-grayscale-using-graphql)

## [9. 사이트 배포](/docs/recipes/deploying-your-site)

쇼타임! 사이트가 마음에 드시나요? 배포 할 준비가 되었습니다!

- [배포 준비](/docs/recipes/deploying-your-site#preparing-for-deployment)
- [Netlify에 배포](/docs/recipes/deploying-your-site#deploying-to-netlify)
- [ZEIT Now에 배포](/docs/recipes/deploying-your-site#deploying-to-zeit-now)
