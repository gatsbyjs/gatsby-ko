---
title: Get to Know Gatsby Building Blocks
typora-copy-images-to: ./
disableTableOfContents: true
---

[**지난 섹션**](/tutorial/part-zero/)에서, 필수 소프트웨어를 설치하고 [**“hello world” 스타터**](https://github.com/gatsbyjs/gatsby-starter-hello-world)를 사용하여 처음으로 Gatsby 사이트를 만들어보면서 여러분은 local 개발 환경을 준비해보았습니다. 이제 그 스타터에 의해 만들어진 코드를 더 자세히 살펴봅시다. 

## Gatsby 스타터를 이용하기

[**튜토리얼 파트 zero**](/tutorial/part-zero/)에서, 여러분은 “hello world” 스타터를 기반한 사이트를 다음과 같은 명령어를 사용하여 만들어 보았습니다:

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

새로운 Gatsby 사이트를 만들 때, 여러분은 다음 명령어 스트럭쳐를 사용해서 존재하는 Gatsby 스타터를 기반한 사이트를 만들 수 있습니다:

```shell
gatsby new [사이트_디렉토리_이름] [스타터_깃허브_리포_URL]
```

만약 마지막의 URL을 적지않는다면, Gatsby는 자동으로 [**default 스타터**](https://github.com/gatsbyjs/gatsby-starter-default)를 이용하여 만듭니다. 이전 튜토리얼 파트 0에서 만든 “Hello World” 사이트를 이번 튜토리얼에서도 사용하겠습니다.

### ✋ 코드 열어보기

여러분의 코드 에디터에서, “Hello World” 사이트를 위해 만들어진 코드를 열어보고 ‘hello-world’ 디렉토리 안에 있는 디렉토리와 파일들의 한번 살펴봅시다. 다음과 같을 것입니다:

![VS Code에서의 Hello World 프로젝트](01-hello-world-vscode.png)

_Note: 다시 말하지만, 비주얼 스튜디오 코드입니다. 다른 에디터를 사용하고 있다면 약간 다르게 보일 것입니다._

이제 홈페이지를 구동하는 코드를 살펴봅시다.

> 💡 이전 섹션에서 `gatsby develop` 이후에 개발 서버를 멈췄다면, 다시 작동 시켜주세요 — hello-world 사이트를 변경할 시간입니다!

## Gatsby 페이지에 익숙해지기

여러분의 코드 에디터에서 `/src` 디렉토리를 열어주세요. 그 안에 `/pages` 디렉토리가 있습니다.

`src/pages/index.js` 파일을 열어주세요. div와 “Hello world!”로 구성된 컴포넌트를 만드는 코드가 있습니다.

### ✋ “Hello World” 홈페이지를 변화시켜보기

1.  “Hello World!”를 “Hello Gatsby!”로 바꾸고 파일을 저장해봅시다. 여러분의 윈도우 창이 나란히 있다면, 여러분이 코드와 컨텐츠를 바꾸고 파일을 저장하면 곧바로 이가 반영되는 것을 볼 수 있을 것입니다.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

> 💡 Gatsby는 여러분의 개발 속도를 높이기 위해 **hot reloading**를 사용합니다. 기본적으로, 여러분이 Gatsby 개발 서버를 사용하는 동안, Gatsby 사이트의 파일들은 백그라운드에서 “감시”되어집니다 — 여러분이 파일을 저장하면 바뀐 것들이 곧바로 브라우저에 반영이 됩니다. 개발 서버를 새로 실행하거나 페이지를 새로고침을 할 필요가 없습니다 — 바뀐 것들이 그냥 반영됩니다.

2.  조금 더 눈에 띄게 바꿔봅시다. `src/pages/index.js` 의 코드를 아래 코드로 바꿔봅시다. 글자 색상이 보라색이고 글자 크기도 조금 더 커져있는 것을 볼수 있을 것 입니다.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

> 💡 튜토리얼 [**파트 2**](/tutorial/part-two/)에서 Gatsby 상에서의 스타일에 대해서 좀더 자세히 알아볼 것입니다.

3.  글자 크기 스타일을 지우고 “Hello Gatsby!” 글자를 1 레벨 헤더로 바꾸고 헤더 아래에 문장을 추가해봅시다.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
  {/* highlight-end */}
  </div>
)
```

![hot reloading과 변화](03-more-hot-reloading.png)

4.  이미치 추가하기. (Unsplash에서 랜덤 이미지).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

![이미지 추가](04-add-image.png)

### 잠시… 자바스크립트 안에서 HTML?

_만약 여러분이 React와 JSX에 이미 익숙하다면, 이 섹션을 건너뛰어도 됩니다._ React 프레임워크를 이전에 사용해보지 않았다면, 자바스크립트 함수에서 HTML이 있는 것에 대해서 의아해할 것입니다. 혹은 첫번째 줄에 있는 `react`를 불러오지만 어디에서도 사용하지 않는지 의아해 할 것입니다. 이 두 요소가 합쳐진 “HTML-in-JS”은 React를 위해 자바스크립트의 문법을 확장한 것으로 JSX라고 불립니다. React에 대한 경험이 없더라도 튜토리얼을 진행할 수 있지만, 궁금해하는 분을 위해 간단히 소개합니다…

`src/pages/index.js` 파일의 원본 내용을 참고합시다:

```jsx:title=src/pages/index.js
import React from "react"

export default () => <div>Hello world!</div>
```

순수한 자바스크립트에서는 아래와 같습니다:

```javascript:title=src/pages/index.js
import React from "react"

export default () => React.createElement("div", null, "Hello world!")
```
이제 `'react'`를 불러오고 사용하는 것을 볼 수 있습니다! 잠깐. 여러분은 JSX를 작성하고 있지, 순수한 HTML과 자바스크립트를 작성하지 않았습니다. 어떻게 브라우저는 그것들을 읽고 있는걸까요? 간단히 답변하자면: 브라우저는 이를 읽지 않습니다. Gatsby 사이트는 도구들과 함께 설치되며, 이것들은 여러분의 소스 코드를 브라우저가 읽을 수 있게 변화시켜주게 셋팅 되어있습니다.

## 컴포넌트 작성하기

여러분이 방금 편집하던 홈페이지는 페이지 컴포넌트를 정의함으로써 만들어졌습니다. “component”는 무엇일까요?

광범위하게 정의된, 컴포넌트는 여러분의 사이트의 빌딩 블록입니다; UI(유저 인터페이스)의 한 부분을 표현하는 코드 그 자체입니다.

Gatsby는 React를 기반으로 합니다. 우리가 **컴포넌트**의 사용법과 정의에 대한 이야기를 할 때, 이는 **React 컴포넌트**를 이야기합니다 — 이는 입력을 허용하고 UI 한 부분을 표현하는 React 엘리먼트를 반환하는 코드(보통 JSX로 작성됨)입니다.

(만약 여러분이 이미 개발자라면) 컴포넌트를 빌드하기 시작할 때에 큰 정신적 변화 중의 하나는, 여러분의 CSS, HTML, 자바스크립트가 긴밀하게 엮여서 종종 한 파일안에서 이들이 존재하는 것입니다.

간단해 보이는 변화지만, 이는 사이트를 만드는 것에 대해 여러분이 어떻게 생각하는지에 대한 깊은 의미를 가지고 있습니다.

커스텀 버튼을 만드는 예제를 봅시다. 이전에, 여러분은 CSS 클래스(아마 `.primary-button`)와 커스텀 스타일을 만들었고 이 스타일을 적용하고 싶을 때 사용했습니다. 예를 들어:
 
```html
<button class="primary-button">Click me</button>
```

컴포넌트 세계에서는, 여러분은 `PrimaryButton` 컴포넌트에 버튼 스타일을 같이 만들고 사이트에서는 다음과 같이 사용합니다:

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Click me</PrimaryButton>
```

Components become the base building blocks of your site. Instead of being limited to the building blocks the browser provides, e.g. `<button />`, you can easily create new building blocks that elegantly meet the needs of your projects.

### ✋ Using page components

Any React component defined in `src/pages/*.js` will automatically become a page. Let’s see this in action.

You already have a `src/pages/index.js` file that came with the “Hello World” starter. Let’s create an about page.

1.  Create a new file at `src/pages/about.js`, copy the following code into the new file, and save.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div style={{ color: `teal` }}>
    <h1>About Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
)
```

2.  Navigate to http://localhost:8000/about/.

![New about page](05-about-page.png)

Just by putting a React component in the `src/pages/about.js` file, you now have a page accessible at `/about`.

### ✋ Using sub-components

Let’s say the homepage and the about page both got quite large and you were rewriting a lot of things. You can use sub-components to break the UI into reusable pieces. Both of your pages have `<h1>` headers — create a component that will describe a `Header`.

1.  Create a new directory at `src/components` and a file within that directory called `header.js`.
2.  Add the following code to the new `src/components/header.js` file.

```jsx:title=src/components/header.js
import React from "react"

export default () => <h1>This is a header.</h1>
```

3.  Modify the `about.js` file to import the `Header` component. Replace the `h1` markup with `<Header />`:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header" // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Adding Header component](06-header-component.png)

In the browser, the “About Gatsby” header text should now be replaced with “This is a header.” But you don’t want the “About” page to say “This is a header.” You want it to say, “About Gatsby”.

4.  Head back to `src/components/header.js` and make the following change:

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  Head back to `src/pages/about.js` and make the following change:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Passing data to header](07-pass-data-header.png)

You should now see your “About Gatsby” header text again!

### What are “props”?

Earlier you defined React components as reusable pieces of code describing a UI. To make these reusable pieces dynamic you need to be able to supply them with different data. You do that with input called “props". Props are (appropriately enough) properties supplied to React components.

In `about.js` you passed a `headerText` prop with the value of `"About Gatsby"` to the imported `Header` sub-component:

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" />
```

Over in `header.js`, the header component expects to receive the `headerText` prop (because you’ve written it to expect that). So you can access it like so:

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 In JSX, you can embed any JavaScript expression by wrapping it with `{}`. This is how you can access the `headerText` property (or “prop!”) from the “props” object.

If you had passed another prop to your `<Header />` component, like so...

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" arbitraryPhrase="is arbitrary" />
```

...you would have been able to also access the `arbitraryPhrase` prop: `{props.arbitraryPhrase}`.

6.  To emphasize how this makes your components reusable, add an extra `<Header />` component to the about page, add the following code to the `src/pages/about.js` file, and save.

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" />
    <Header headerText="It's pretty cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Duplicate header to show reusability](08-duplicate-header.png)

And there you have it; A second header — without rewriting any code — by passing different data using props.

### Using layout components

Layout components are for sections of a site that you want to share across multiple pages. For example, Gatsby sites will commonly have a layout component with a shared header and footer. Other common things to add to layouts include a sidebar and/or a navigation menu.

You’ll explore layout components in [**part three**](/tutorial/part-three/).

## Linking between pages

You'll often want to link between pages — Let's look at routing in a Gatsby site.

### ✋ Using the `<Link />` component

1.  Open the index page component (`src/pages/index.js`), import the `<Link />` component from Gatsby, add a `<Link />` component above the header, and give it a `to` property with the value of `"/contact/"` for the pathname:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line
import Header from "../components/header"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Hello Gatsby!" />
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

When you click the new "Contact" link on the homepage, you should see...

![Gatsby dev 404 page](09-dev-404.png)

...the Gatsby development 404 page. Why? Because you're attempting to link to a page that doesn't exist yet.

2.  Now you'll have to create a page component for your new "Contact" page at `src/pages/contact.js` and have it link back to the homepage:

```jsx:title=src/pages/contact.js
import React from "react"
import { Link } from "gatsby"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Send us a message!</p>
  </div>
)
```

After you save the file, you should see the contact page and be able to link between it and the homepage.

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Sorry! You browser doesn't support this video.</p>
</video>

The Gatsby `<Link />` component is for linking between pages within your site. For external links to pages not handled by your Gatsby site, use the regular HTML `<a>` tag.

## Deploying a Gatsby site

Gatsby.js is a _modern site generator_, which means there are no servers to setup or complicated databases to deploy. Instead, the Gatsby `build` command produces a directory of static HTML and JavaScript files which you can deploy to a static site hosting service.

Try using [Surge](http://surge.sh/) for deploying your first Gatsby website. Surge is one of many "static site hosts" which make it possible to deploy Gatsby sites.

If you haven't previously installed &amp; set up Surge, open a new terminal window and install their command-line tool:

```shell
npm install --global surge

# Then create a (free) account with them
surge login
```

Next, build your site by running the following command in the terminal at the root of your site (tip: make sure you're running this command at the root of your site, in this case in the hello-world folder, which you can do by opening a new tab in the same window you used to run `gatsby develop`):

```shell
gatsby build
```

The build should take 15-30 seconds. Once the build is finished, it's interesting to take a look at the files that the `gatsby build` command just prepared to deploy.

Take a look at a list of the generated files by typing in the following terminal command into the root of your site, which will let you look at the `public` directory:

```shell
ls public
```

Then finally deploy your site by publishing the generated files to surge.sh.

```shell
surge public/
```

Once this finishes running, you should see in your terminal something like:

![Screenshot of publishing Gatsby site with Surge](surge-deployment.png)

Open the web address listed on the bottom line (`lowly-pain.surge.sh` in this
case) and you'll see your newly published site! Great work!

## ➡️ 다음에 할 것?

이번 섹션에서 여러분이 한 것들:

- Gatsby 스타터와, 이를 사용한 새로운 프로젝트를 만드는 법에 대한 학습
- JSX 문법에 대한 학습
- 컴포넌트에 대한 학습
- Gatsby 페이지 컴포넌트와 서브 컴포넌트에 대한 학습
- React “props”와 React 컴포넌트의 재사용에 대한 학습

이제 [**사이트에 스타일 추가하기**](/tutorial/part-two/)로 이동합시다!
