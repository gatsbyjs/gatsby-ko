---
title: CSS-in-JS를 사용하여 스타일 향상시키기
overview: true
---

CSS-in-JS는 외부 CSS 파일이 아닌 JavaScript로 스타일을 작성하여 컴포넌트의 스타일을 쉽게 범위화하고, 사용하지않는 코드를 제거하며, 성능을 향상시키고, 동적으로 스타일을 적용하는 등의 접근 방식을 말합니다.

CSS-in-JS는 CSS와 JavaScript 사이의 간격을 메웁니다:

1. **컴포넌트**: 모든 것이 구성요소라는 React의 개념과 잘 통합되어 컴포넌트로 사이트를 스타일링할 수 있습니다.
2. **범위**: 이것은 첫 번째 부가 요소입니다. [CSS 모듈](/docs/css-modules/)과 마찬가지로 CSS-in-JS는 기본적으로 컴포넌트에 범위가 지정됩니다.
3. **동적**: JavaScript 변수를 통합하여 컴포넌트 상태에 따라 동적으로 사이트를 스타일링합니다
4. **보너스**: 대부분의 CSS-in-JS 라이브러리는 선택한 라이브러리에 따라 캐싱, 자동 벤더 접두사, 중요 CSS를 적시에 로드하고 다른 많은 기능을 구현하는 데 도움이 되는 고유한 클래스 이름을 생성합니다.

CSS-in-JS는 Gatsby에서는 필요하지 않지만 위에 나열된 이유로 JavaScript 개발자들 사이에서 매우 인기가 있습니다. 자세한 내용은 Max Stoiber의 (CSS-in-JS 라이브러리 [styled-class](/docs/styled-components/)를 만든이) 기사 [_JavaScript에서 CSS를 쓰는 이유_](https://mxstbr.com/thoughts/css-in-js/)를 읽어보세요. 그러나 CSS-in-JS에 의존하지 않는 것이 더 포괄적인 프런트엔드 스킬 세트를 장려할 수 있으므로 CSS-in-JS가 필요한지도 고려해야 합니다. 또한 JSX나 CSS로부터 스타일을 포트하는 것이 어렵습니다.

_이 기능은 React 또는 Gatsby의 일부가 아니며, 많은 [써드파티 CSS-in-JS 라이브러리](https://github.com/MicheleBertoli/css-in-js#css-in-js)를 사용해야 합니다._

> 안정적인 CSS 클래스를 CSS-in-JS와 함께 JSX 마크업에 추가하면 접근성을 위해 [User Stylesheets](https://www.viget.com/articles/inline-styles-user-style-sheets-and-accessibility/)를 쉽게 포함할 수 있습니다. [Styled Components](/docs/styled-components#enabling-user-stylesheets-with-a-stable-class-name) 예제를 참조하세요.

JavaScript가 로드될 때까지 스타일이 적용되지 않으므로 스타일을 추출하는 플러그인은 스타일이 적용되지않은 컨텐츠의 플래시를 방지(FOUC)하는 데 필요합니다. 이를 충족하기 위해 모든 CSS-in-JS 라이브러리에는 스타일을 추출하여 빌드하는 동안 HTML에 삽입하는 Gatsby 플러그인이 있으며, 이는 FOUC를 방지합니다.

이 섹션에는 각 라이브러리를 사용하여 글로벌 스타일을 설정하는 방법을 비롯하여 가장 인기 있는 CSS-in-JS 라이브러리를 사용하여 사이트를 스타일링하는 방법에 대한 가이드가 포함되어 있습니다.

<GuideList slug={props.slug} />
