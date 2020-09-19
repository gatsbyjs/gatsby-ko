---
title: 정적 사이트 캐싱
---

매우 빠른 웹 사이트를 만드는 중요한 부분은 적절한 HTTP 캐싱을 설정하는 것입니다. HTTP 캐싱을 통해 브라우저는 웹 사이트의 리소스를 캐시할 수 있으므로 사용자가 사이트에 재 방문했을 때 웹 사이트의 극히 일부만 다운로드해야 합니다.

리소스는 유형별로 다르게 캐시됩니다. `public/`에 빌드된 여러 유형의 파일을 캐시하는 방법을 살펴보겠습니다.

## HTML

HTML 파일은 브라우저에서 캐시하면 안 됩니다. Gatsby 사이트를 재구성할 때 HTML 파일의 내용을 업데이트하는 경우가 많습니다. 따라서 브라우저가 HTML 파일의 최신 버전을 다운로드해야 하는 경우 모든 요청을 확인하도록 지시해야 합니다.

`cache-control` 헤더는 `cache-control: public, max-age=0, must-revalidate`<sup>1</sup>이어야 합니다.

## 페이지 데이터

HTML 파일과 마찬가지로 `public/page-data` 디렉토리에 있는 JSON 파일은 브라우저에서 캐시하면 안 됩니다. 실제로 사이트를 다시 배포하지 않고도 이러한 파일을 업데이트할 수 있습니다. 이 때문에 브라우저에서 응용 프로그램의 데이터가 변경되었는지 여부를 모든 요청을 확인하도록 지시해야 합니다.

`cache-control` 헤더는 `cache-control: public, max-age=0, must-revalidate`<sup>1</sup>이어야 합니다.

## 앱 데이터

The new `app-data.json` file which contains the build hash for the current deployment of the site should also share the same `cache-control` header as `page-data.json`. This is to ensure that the version of the site loaded in the browser is always synchronised with the currently deployed version.현재 사이트의 빌드 해시가 포함된 새로운 `app-data.json` 파일도 `page-data.json`과 동일한 `cache-control` 헤더를 공유해야 합니다. 이는 브라우저에 로드된 사이트의 버전이 현재 배포된 버전과 항상 동기화되도록 하기 위한 것입니다.

`cache-control` 헤더는 `cache-control: public, max-age=0, must-revalidate`<sup>1</sup>이어야 합니다.

## 정적 파일

`static/`안의 모든 파일은 영구적으로 캐시되어야 합니다. 이 디렉터리에 있는 파일의 경우, Gatsby는 파일 내용과 직접 연결된 경로를 만듭니다. 파일 내용이 변경되면 파일 경로도 변경됩니다. 이러한 경로는 `reactnext-gatsby-performance.001-a3e9d70183ff294e097c4319d0f8cff6-0b1ba.png`와 같이 이상하게 보이지만, 해당 경로는 요청될 때 항상 동일한 파일이 반환되기 때문에 Gatsby가 영구히 캐시할 수 있습니다.

`cache-control` 헤더는 `cache-control: public, max-age=31536000, immutable`이어야 합니다.

## JavaScript와 CSS

_WebPack에서 생성한_ JavaScript 및 CSS 파일도 영구적으로 캐시해야 합니다. 정적 파일과 마찬가지로 Gatsby는 파일 내용에 따라 JS & CSS 파일 이름(해시가 적용된!)을 만듭니다. 파일 내용이 변경되면 파일 해시가 변경되므로 _웹 팩에서 생성된_ 이러한 파일은 캐시에 안전합니다.

`cache-control` 헤더는 `cache-control: public, max-age=31536000, immutable`이어야 합니다.

유일한 예외는 `/sw.js` 파일인데, 각 로드에 대해 새로운 버전의 사이트를 사용할 수 있는지 확인하기 위해 매번 재검증이 필요합니다. 이 파일은 `gatsby-plugin-offline` 및 기타 서비스 워커 플러그인에 의해 생성되어 오프라인으로 콘텐츠를 서비스합니다. 캐시 컨트롤 헤더는 "cache-control: public, max-age=0, revalidate(필수 확인)"이어야 합니다.`cache-control` 헤더는 `cache-control: public, max-age=0, must-revalidate`<sup>1</sup>이어야 합니다.

## 서로 다른 호스팅에서 캐싱을 설정하는 경우

캐싱을 설정하는 방법은 사이트를 호스트하는 방법에 따라 다릅니다. 캐싱 헤더의 생성을 자동화하기 위해 호스트당 Gatsby 플러그인을 만들 것을 권장합니다.

다음 플러그인이 생성되었습니다:

- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify/)
- [gatsby-plugin-s3](https://github.com/jariz/gatsby-plugin-s3)

Now를 사용하여 배포하는 경우, [Now documentation](https://zeit.co/guides/deploying-gatsby-with-now#bonus:-cache-your-gatsby-assets)의 지침을 따르세요.

---

<sup>1</sup> 'max-age=0, must-revalidate' 대신 'no-cache'를 사용할 수 있습니다. 이름이 의미하는 바는 아니지만 'no-cache'는 캐시가 먼저 캐시의 신선도를 검증하고 캐시된 콘텐츠를 제공할 수 있도록 허용합니다. <sup>[2][3] </sup> 어느 경우든 클리이언트는 매 요청마다 오리진 서버를 왕복해야 합니다. 그러나 ETags 또는 Last-Modified를 올바르게 사용하고 있고 캐시된 복사본이 여전히 유효하면(예: 파일이 캐시된 이후 원본 서버에서 변경되지 않음) 에셋을 다운로드하지 않아도 됩니다.

[2]: https://tools.ietf.org/html/rfc7234#section-5.2.2.1
[3]: https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#no-cache_and_no-store
