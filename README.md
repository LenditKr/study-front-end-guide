Grab Front End Guide
==

[![Front End Developer Desk](images/desk.png)](https://dribbble.com/shots/3577639-Isometric-Developer-Desk)

_Credits: [Illustration](https://dribbble.com/shots/3577639-Isometric-Developer-Desk) by [@yangheng](https://dribbble.com/yangheng)_

_This guide has been cross-posted on [Free Code Camp](https://medium.freecodecamp.com/grabs-front-end-guide-for-large-teams-484d4033cc41)._

[Grab](https://www.grab.com) is Southeast Asia (SEA)'s leading transportation platform and our mission is to drive SEA forward, leveraging on the latest technology and the talented people we have in the company. As of May 2017, we handle [2.3 million rides daily](https://www.bloomberg.com/news/videos/2017-05-11/tans-says-company-has-more-than-850-000-drivers-video) and we are growing and hiring at a rapid scale.

To keep up with Grab's phenomenal growth, our web team and web platforms have to grow as well. Fortunately, or unfortunately, at Grab, the web team has been [keeping up](https://blog.daftcode.pl/hype-driven-development-3469fc2e9b22) with the latest best practices and has incorporated the modern JavaScript ecosystem in our web apps.

The result of this is that our new hires or back end engineers, who are not necessarily well-acquainted with the modern JavaScript ecosystem, may feel overwhelmed by the barrage of new things that they have to learn just to complete their feature or bug fix in a web app. Front end development has never been so complex and exciting as it is today. New tools, libraries, frameworks and plugins emerge every other day and there is so much to learn. It is imperative that newcomers to the web team are guided to embrace this evolution of the front end, learn to navigate the ecosystem with ease, and get productive in shipping code to our users as fast as possible. We have come up with a study guide to introduce why we do what we do, and how we handle front end at scale.

This study guide is inspired by ["A Study Plan to Cure JavaScript Fatigue"](https://medium.freecodecamp.com/a-study-plan-to-cure-javascript-fatigue-8ad3a54f2eb1#.g9egaapps) and is mildly opinionated in the sense that we recommend certain libraries/frameworks to learn for each aspect of front end development, based on what is currently deemed most suitable at Grab. We explain why a certain library/framework/tool is chosen and provide links to learning resources to enable the reader to pick it up on their own. Alternative choices that may be better for other use cases are provided as well for reference and further self-exploration.

If you are familiar with front end development and have been consistently keeping up with the latest developments, this guide will probably not be that useful to you. It is targeted at newcomers to front end.

If your company is exploring a modern JavaScript stack as well, you may find this study plan useful to your company too! Feel free to adapt it to your needs. We will update this study plan periodically, according to our latest work and choices.

*- Grab Web Team*

**Pre-requisites**

- Good understanding of core programming concepts.
- Comfortable with basic command line actions and familiarity with source code version control systems such as Git.
- Experience in web development. Have built server-side rendered web apps using frameworks like Ruby on Rails, Django, Express, etc.
- Understanding of how the web works. Familiarity with web protocols and conventions like HTTP and RESTful APIs.

### Table of Contents

- [Single-page Apps (SPAs)](#single-page-apps-spas)
- [New-age JavaScript](#new-age-javascript)
- [User Interface](#user-interface---react)
- [State Management](#state-management---fluxredux)
- [Coding with Style](#coding-with-style---css-modules)
- [Maintainability](#maintainability)
  - [Testing](#testing---jest--enzyme)
  - [Linting JavaScript](#linting-javascript---eslint)
  - [Linting CSS](#linting-css---stylelint)
  - [Types](#types---flow)
- [Build System](#build-system---webpack)
- [Package Management](#package-management---yarn)
- [Continuous Integration](#continuous-integration)
- [Hosting](#hosting---amazon-s3)
- [Deployment](#deployment)

Certain topics can be skipped if you have prior experience in them.

## Single-page Apps (SPAs)

최근 웹 개발자들은 그들이 만드는 제품을 웹 사이트보다는 웹 앱이라는 이름으로 부르고 있습니다. 두 용어 간에 명확한 구분이 있는 것은 아니지만, 사용자들이 어떤 동작을 하고 그에 대한 응답을 받을 수 있게 하는 좀 더 동적이고 상호작용이 많은 것을 웹 앱이라고 부르는 경향이 있습니다. 전통적으로, 브라우저는 서버에서 HTML을 받아 화면에 그립니다. 사용자가 다른 URL로 이동하면, 전체 페이지의 새로고침이 필요하며 서버는 새로운 페이지를 위해 새로운 HTML을 전송합니다. 이것을 서버 사이드 렌더링이라고 부릅니다.

그러나 모던 SPA에서는 대신 클라이언트 사이드 렌더링을 사용합니다. 브라우저는 서버에서 전체 앱을 구성하는 데 필요한 스크립트(프레임워크, 라이브러리, 앱 코드)와 스타일시트를 포함한 초기 페이지를 로드합니다. 사용자가 다른 페이지로 이동할 때, 페이지는 새로고침되지 않습니다. 페이지의 URL은 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 을 통해 업데이트됩니다. 새로운 페이지를 위해 필요한 (보통 JSON 포맷으로 이루어진) 새로운 데이터는 브라우저가 서버에 보내는 [AJAX](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started) 요청을 통해 수신됩니다. 그 후 SPA는 수신된 데이터를 가지고, 초기 페이지를 로딩할 때 다운로드 받아 두었던 자바스크립트를 이용해 동적으로 페이지를 업데이트합니다. 이 모델은 네이티브 모바일 앱들이 동작하는 방식과 비슷합니다.

장점:

- 앱의 반응속도가 빠르고 사용자가 페이지 이동 중에 전체 페이지 새로고침으로 인한 번쩍임을 경험하지 않습니다.
- 페이지를 로드할 때마다 같은 asset이 매번 다운로드될 필요가 없기 때문에 서버에 더 적은 HTTP request를 하게 됩니다.
- 서버와 클라이언트의 관심사가 명확하게 분리됩니다; 서버 코드 수정 없이 다양한 플랫폼(모바일, 챗봇, 스마트워치 등)들을 위한 클라이언트를 쉽게 만들 수 있습니다. 또한 클라이언트와 서버의 기술 스택을 독립적으로 변경할 수 있습니다(API 규약만 잘 지켜진다면).

단점:

- 프레임워크, 앱 코드, 여러 페이지에서 필요한 asset 등으로 인해 초기 페이지 로딩이 무거워집니다.<sup><a href="#fn1" id="ref1">1</a></sup>
- 서버에서 모든 요청을 하나의 진입 포인트로 라우팅하고 이후 클라이언트에서 라우팅 역할을 가져갈 수 있도록 하는 추가 작업이 필요합니다.
- SPA는 자바스크립트를 통해 콘텐츠를 렌더링하지만, 모든 검색엔진이 크롤링 도중 자바스크립트를 실행하는 것은 아니기 때문에 일부 검색엔진에서는 빈 페이지를 보게 됩니다. 이는 의도치 않게 검색엔진 최적화 (SEO)에 악영향을 주게 됩니다. <sup><a href="#fn2" id="ref2">2</a></sup>. 그러나 대부분의 경우 앱을 만들 때 모든 콘텐츠가 검색엔진에 색인될 필요는 없으므로 SEO가 가장 중요한 요소는 아닙니다. 이 문제를 극복하기 위해서는 서버 사이드 렌더링을 하거나, 브라우저에서 자바스크립트를 렌더링해서 정적 HTML을 저장한 후 크롤러에게 제공하는 [Prerender](https://prerender.io/) 같은 서비스를 사용할 수도 있습니다.

전통적인 서버 사이드 렌더링 앱은 여전히 유효한 옵션이지만, 대규모 엔지니어링 팀에게는 클라이언트-서버의 명확한 분리가 더 좋습니다. 왜냐하면 클라이언트와 서버 코드가 독립적으로 개발/배포될 수 있기 때문입니다. 같은 API 서버를 사용하는 여러 클라이언트가 있을 경우 특히 유용합니다.

웹 개발자들이 이제 페이지보다는 앱을 만들고 있기 때문에, 클라이언트 측 자바스크립트의 구성이 점차 중요해지고 있습니다. 서버 사이드 렌더링된 페이지에서는, 각 페이지에 jQuery 스니펫을 넣어서 유저 상호작용을 추가하는 것이 일반적이었습니다. 그러나 대규모의 앱을 만들 때는 jQuery만으로는 충분하지 않습니다. 무엇보다도 jQuery는 DOM 조작을 위한 라이브러리이지 프레임워크는 아니므로, 앱을 위한 명확한 구조와 구성을 정의하지는 않습니다.

자바스크립트 프레임워크들은 DOM 위에 더 높은 수준의 추상화를 제공하기 위해 만들어졌으며, DOM 외부의 메모리에 상태를 유지할 수 있도록 해줍니다. 또한 프레임워크를 사용하면 앱을 만들기 위해 권장되는 개념과 모범 사례를 재사용할 수 있는 이점도 누릴 수 있습니다. 코드 기반에 익숙하지 않지만 프레임워크에 대한 경험이 있는 팀의 새로운 엔지니어는 코드가 익숙한 구조로 구성되어 있기 때문에 코드를 더 쉽게 이해할 수 있습니다. 인기 있는 프레임워크들은 많은 튜토리얼과 가이드가 있으며, 동료들과 커뮤니티의 지식과 경험을 활용하면 새로운 엔지니어들이 빠르게 작업할 수 있습니다.

#### Study Links

- [Single Page App: advantages and disadvantages](http://stackoverflow.com/questions/21862054/single-page-app-advantages-and-disadvantages)
- [The (R)Evolution of Web Development](http://blog.isquaredsoftware.com/presentations/2016-10-revolution-of-web-dev/)
- [Here's Why Client Side Rendering Won](https://medium.freecodecamp.com/heres-why-client-side-rendering-won-46a349fadb52)

## New-age JavaScript

자바스크립트 웹 앱 작성의 세계에 뛰어들기 전에, 웹의 언어인 자바스크립트(또는 ECMAScript)에 익숙해지는 것이 중요합니다. 자바스크립트는 [웹 서버](https://nodejs.org/en/)나 [네이티브 모바일 앱](https://facebook.github.io/react-native/), [데스크탑 앱](https://electron.atom.io/)까지도 만들 수 있는 끝내주게 다재다능한 언어입니다.

2015년 전까지는 ECMAScript 5.1이 가장 최신 버전이었습니다. 그러나 최근에 자바스크립트는 짧은 시간동안 갑자기 엄청난 발전을 이뤘습니다. 2015년에 (이전에 ECMAScript 6로 불리던)ECMAScript 2015가 나왔고, 코드를 깔끔하게 작성하게 해주는 여러가지 문법적인 구조가 소개되었습니다. 이에 대해 관심이 있으면 Auth0가 쓴 글 [history of JavaScript](https://auth0.com/blog/a-brief-history-of-javascript/)를 읽어보세요. 아직까지 모든 브라우저에서 ES2015 명세가 다 구현되지는 않았지만, [Babel](https://babeljs.io/)과 같은 도구들의 도움을 받으면 개발자가 ES2015로 코드를 작성해도 이를 ES5로 트랜스파일해서 브라우저에서 돌아가도록 할 수 있습니다.

사실 ES5와 ES2015에 둘 다 익숙해질 필요가 있습니다. ES2015가 아직 나온지 그리 오래 않았기 때문에 많은 오픈 소스 및 Node.js 앱들은 아직 ES5로 작성되고 있습니다. 아마 브라우저 콘솔에서 디버깅을 할 때도 ES2015 문법을 온전히 사용하지 못할 겁니다. 반면에 아래에서 소개할 모던 라이브러리들의 문서나 예제 코드들은 ES2015로 작성되었습니다. Grab에서는 [babel-preset-env](https://github.com/babel/babel-preset-env)를 사용하고 있는데, 이 사랑스러운 녀석은 우리가 최신 자바스크립트의 문법적 향상에 따른 생산성 증대를 누리도록 해 줍니다. 브라우저가 최신 ES표준 지원율을 나날이 높혀 나가는 가운데, `babel-preset-env`는 브라우저에서 지원하지 않는 기능들을 지능적으로 판단하여 이에 필요한 플러그인들을 알아서 골라 트랜스파일합니다. 혹시 표준 제안 중 stable 상태라 곧 브라우저에서 구현될 예정인 명세를 사용하는 것을 선호한다면 [babel-preset-stage-3](https://babeljs.io/docs/plugins/preset-stage-3/)가 더 적합할 수 있습니다.

하루 이틀 정도 시간을 내어 ES5를 복습하고 ES2015를 탐험해 보세요. ES2015에서 주로 많이 사용되는 기능은 "Arrows and Lexical This", "Classes", "Template Strings", "Destructuring", "Default/Rest/Spread operators", and "Importing and Exporting modules" 정도가 있습니다.

**예상소요시간: 3-4일** 다른 라이브러리들을 배우거나 앱을 직접 만들면서도 문법을 배우거나 살펴볼 수 있습니다.

#### Study Links

- [Learn ES5 on Codecademy](https://www.codecademy.com/learn/learn-javascript)
- [Learn ES2015 on Babel](https://babeljs.io/learn-es2015/)
- [ES6 Katas](http://es6katas.org/)
- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) (Advanced content, optional for beginners)
- [Answers to Front End Job Interview Questions — JavaScript](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#js-questions)

## User Interface - React

<img alt="React Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/react-logo.svg" width="256px" />

최근에 프론트엔드 생태계의 많은 주목을 끈 자바스크립트 프로젝트가 있다면 그건 [React](https://facebook.github.io/react/)일 것입니다. React는 페이스북의 똑똑한 개발자들이 개발한 오픈소스 라이브러리입니다. React에서 개발자들은 웹 인터페이스를 위한 Component들을 만들고 그것들을 함께 구성합니다. 

React는 급진적인 아이디어들을 가져와서 개발자들에게 [모범 사례들을 재고](https://www.youtube.com/watch?v=DgVS-zXgMTk)하도록 권장합니다. 수년 동안 웹 개발자들은 HTML, Javascript 그리고 CSS를 별도로 작성하는 것이 좋은 사례라고 배워왔습니다. React는 정반대로 당신의 HTML과 [CSS를 Javascript에 작성](https://speakerdeck.com/vjeux/react-css-in-js)하도록 권장합니다. 처음에는 미친 생각처럼 보일 수 있지만 시도하고 난 이후에는 처음 생각했던 것만큼 이상하지는 않습니다. 왜냐하면, 프론트엔트 개발 분야가 component 기반 개발 패러다임으로 전환하고 있기 때문입니다. React의 특징들은 아래와 같습니다:

- **서술적(Declarative)** - View에서 보고 싶은 것을 서술하고 그것을 갖기 위한 방법은 서술하지 않아도 됩니다. jQuery 시대에서 개발자들은 앱의 상태를 다음 단계로 변경하기 위해 DOM을 조작하는 일련의 단계를 따라야 했습니다. React에서는 단순히 component 내에 상태만 변경하면 view는 그 상태에 맞게 자동으로 변경됩니다. `render()` 함수의 markup을 보고 어떻게 component가 보일지 쉽게 알아낼 수 있습니다. 

- **기능적(Functional)** - View는 `props` 와 `state`의 순수함수입니다. 대부분의 경우 React component는 `props`(외부 parameter)와 `state`(내부 데이터)에 의해 정의됩니다. 같은 `props`와 `state`에서 언제나 같은 view가 생성됩니다. 순수 함수는 테스트하기 쉽고 기능적인 component들도 마찬가지입니다. React에서의 테스트는 Component의 인터페이스가 잘 정의되어 있고 component에 다른 `props`와 `state` 넣어 렌더링 결과를 비교할 수 있기 때문에 쉽습니다.

- **유지가능한(Maintainable)** - View를 component 기반으로 작성하면 재사용이 가능합니다. 우리는 component의 `propTypes`를 정의하는 것이 독자가 그 component를 사용하는데 필요한 것을 명확하게 알 수 있도록 React 코드를 self-documenting 한다는 점을 발견했습니다. 마지막으로 View와 로직은 component 내에 자체 포함되어 있으므로 다른 구성 요소에 영향을 받지 않으며 영향을 미치지도 않습니다. 따라서 같은 `props`가 component의 제공되는 한 대규모의 리팩토링 중에 component들을 쉽게 이동시킬 수 있습니다.

- **고성능(High Performance)** - React가 가상의 DOM([shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Shadow_DOM)과 혼동하지 마십시오)을 사용하고 변경된 상태가 있을 때 다시 모든 것을 렌더링한다고 들었을 것입니다. 왜 가상의 DOM이 필요할까요? 최신 JavaScript 엔진들은 빠르지만, DOM을 읽고 쓰는데 느립니다. React는 메모리에 DOM의 간단한 가상 표현을 유지합니다. 모든 것을 다시 렌더링하는 것은 오해의 소지가 있는 용어입니다. React에서 다시 렌더링한다는 것은 실제 DOM 자체가 아닌 DOM의 메모리 표현을 다시 렌더링하는 것을 의미합니다. component의 기본 데이터에 변경사항이 있을 때 새로운 가상표현이 생성되고 이전의 표현방식과의 차이를 비교합니다. 그 차이(최소한의 변경이 필요)는 실제 브라우저 DOM에 패치됩니다.

- **쉽게 학습 가능(Ease of Learning)** - React를 학습하는 것은 매우 간단합니다. React API는 [이것](https://angular.io/docs/ts/latest/api/)과 비교했을때 상대적으로 작습니다. 학습해야 하는 API의 수는 몇 가지 되지 않고 그것들은 쉽게 자주 변경되지 않습니다. React 커뮤니티는 가장 큰 커뮤니티 중 하나이며 활발한 도구 생태계, 오픈소스 UI component 및 수많은 온라인 자료를 통해 학습을 시작할 수 있습니다.

- **개발자 경험(Developer Experience)** - React에는 개발 경험을 향상하게 만드는 다수의 도구가 있습니다. [React Developer Tools](https://github.com/facebook/react-devtools)는 component를 검사하고 보고 `props`와 `state`를 조작할 수 있는 브라우저 확장 프로그램입니다. webpack과 함께 [Hot reloading](https://github.com/gaearon/react-hot-loader)은 새로고침 없이 코드의 변화를 브라우저에서 확인할 수 있도록 도와줍니다. 프론트 엔드 개발은 코드를 수정하고 저장하며 브라우저를 새로고침하는 작업을 많이 포함합니다. Hot reloading은 그 마지막 과정을 없애줍니다. 라이브러리의 업데이트가 있을 때 Facebook은 여러분의 코드를 새로운 API에 적용할 수 있도록 도와주는 [codemod scripts](https://github.com/reactjs/react-codemod)를 제공합니다. 이렇게 하면 업그레이드 프로세스가 상대적으로 고통 없이 진행됩니다. React의 개발 경험을 만드는데 헌신한 Facebook팀의 노고에 감사드립니다.<br> ![React Devtools Demo](images/react-devtools-demo.gif)

수년 동안 React보다 더 뛰어난 새로운 view 라이브러리가 등장했습니다. React는 가장 빠른 라이브러리는 아니지만 생태계, 전반적인 사용 경험 및 여러 장점을 보면 여전히 가장 큰 라이브러리 중 하나입니다. Facebook은 [기존 조정 알고리즘](https://github.com/acdlite/react-fiber-architecture)을 재작성하여 React를 더욱 신속하게 만들기 위해 노력하고 있습니다. React가 소개한 개념은 더 나은 코드 작성, 유지 보수가 쉬운 웹 앱 작성 방법과 함께 우리를 더 나은 개발자로 만들어주었고 그것에 감사드립니다. 

React 홈페이지에서 tic-tac-toe 게임을 제작하는 방법에 대한 [turorial](https://facebook.github.io/react/tutorial/tutorial.html)을 통해서 React가 무엇이고 이것으로 어떤일을 할 수 있는지 알아보기를 추천드립니다. 보다 깊이 있는 학습을 원하시면, Egghead 코스를 확인하십시오 [Build Your First Production Quality React App](https://egghead.io/courses/build-your-first-production-quality-react-app). React문서에서 다루지 않는 몇 가지 고급 개념과 실제 사용법을 다룹니다. Facebook이 제공하는 [Create React App](https://github.com/facebookincubator/create-react-app)은 최소의 구성으로 React 프로젝트를 실행할 수 있는 뼈대이며 새로운 React 프로젝트를 시작할 때 사용하기를 추천합니다.

React는 프레임워크가 아닌 라이브러리이며 view 아래 layer(앱 상태)를 처리하지 않습니다. 나중에 더 설명 드리겠습니다.

**예상소요시간: 3-4 days.** 해야 할 일 목록과 같은 단순한 프로젝트를 구축해보십시오. Hacker News는 순수한 React를 통해 만들어 집니다. React에 대한 이해가 천천히 진행되면서 아마도 몇가지 React로는 해결되지 않는 몇 가지 문제에 직면하게 되고 이는 다음 주제로 연결됩니다.

#### Study Links

- [React Official Tutorial](https://facebook.github.io/react/tutorial/tutorial.html)
- [Egghead Course - Build Your First Production Quality React App](https://egghead.io/courses/build-your-first-production-quality-react-app)
- [Simple React Development in 2017](https://hackernoon.com/simple-react-development-in-2017-113bd563691f)
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.5iexphyg5)

#### Alternatives

- [Angular](https://angular.io/)
- [Ember](https://www.emberjs.com/)
- [Vue](https://vuejs.org/)
- [Cycle](https://cycle.js.org/)

## State Management - Flux/Redux

<img alt="Redux Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/redux-logo.svg" width="256px" />

As your app grows bigger, you may find that the app structure becomes a little messy. Components throughout the app may have to share and display common data but there is no elegant way to handle that in React. After all, React is just the view layer, it does not dictate how you structure the other layers of your app, such as the model and the controller, in traditional MVC paradigms. In an effort to solve this, Facebook invented Flux, an app architecture that complements React's composable view components by utilizing a unidirectional data flow. Read more about how Flux works [here](https://facebook.github.io/flux/docs/in-depth-overview.html). In summary, the Flux pattern has the following characteristics:

- **Unidirectional data flow** - Makes the app more predictable as updates can be tracked easily.
- **Separation of concerns** - Each part in the Flux architecture has clear responsibilities and are highly decoupled.
- **Works well with declarative programming** - The store can send updates to the view without specifying how to transition views between states.

As Flux is not a framework per se, developers have tried to come up with many implementations of the Flux pattern. Eventually, a clear winner emerged, which was [Redux](http://redux.js.org/). Redux combines the ideas from Flux, [Command pattern](https://www.wikiwand.com/en/Command_pattern) and [Elm architecture](https://guide.elm-lang.org/architecture/) and is the de facto state management library developers use with React these days. Its core concepts are:

- App **state** is described by a single plain old JavaScript object (POJO).
- Dispatch an **action** (also a POJO) to modify the state.
- **Reducer** is a pure function that takes in current state and action to produce a new state.

The concepts sound simple, but they are really powerful as they enable apps to:

- Have their state rendered on the server, booted up on the client.
- Trace, log and backtrack changes in the whole app.
- Implement undo/redo functionality easily.

The creator of Redux, [Dan Abramov](https://github.com/gaearon), has taken great care in writing up detailed documentation for Redux, along with creating comprehensive video tutorials for learning [basic](https://egghead.io/courses/getting-started-with-redux) and [advanced](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) Redux. They are extremely helpful resources for learning Redux.

**Combining View and State**

While Redux does not necessarily have to be used with React, it is highly recommended as they play very well with each other. React and Redux have a lot of ideas and traits in common:

- **Functional composition paradigm** - React composes views (pure functions) while Redux composes pure reducers (also pure functions). Output is predictable given the same set of input.
- **Easy To Reason About** - You may have heard this term many times but what does it actually mean? We interpret it as having control and understanding over our code - Our code behaves in ways we expect it to, and when there are problems, we can find them easily. Through our experience, React and Redux makes debugging simpler. As the data flow is unidirectional, tracing the flow of data (server responses, user input events) is easier and it is straightforward to determine which layer the problem occurs in.
- **Layered Structure** - Each layer in the app / Flux architecture is a pure function, and has clear responsibilities. It is relatively easy to write tests for pure functions. You have to centralize changes to your app within the reducer, and the only way to trigger a change is to dispatch an action.
- **Development Experience** - A lot of effort has gone into creating tools to help in debugging and inspecting the app while development, such as [Redux DevTools](https://github.com/gaearon/redux-devtools). <br> ![Redux Devtools Demo](images/redux-devtools-demo.gif)

Your app will likely have to deal with async calls like making remote API requests. [redux-thunk](https://github.com/gaearon/redux-thunk) and [redux-saga](https://github.com/redux-saga/redux-saga) were created to solve those problems. They may take some time to understand as they require understanding of functional programming and generators. Our advice is to deal with it only when you need it.

[react-redux](https://github.com/reactjs/react-redux) is an official React binding for Redux and is very simple to learn.

**Estimated Duration: 4 days.** The egghead courses can be a little time-consuming but they are worth spending time on. After learning Redux, you can try incorporating it into the React projects you have built. Does Redux solve some of the state management issues you were struggling with in pure React?

#### Study Links

- [Flux Homepage](http://facebook.github.io/flux)
- [Redux Homepage](http://redux.js.org/)
- [Egghead Course - Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux)
- [Egghead Course - Build React Apps with Idiomatic Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)
- [React Redux Links](https://github.com/markerikson/react-redux-links)
- [You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)

#### Alternatives

- [MobX](https://github.com/mobxjs/mobx)

## Coding with Style - CSS Modules

<img alt="CSS Modules Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/css-modules-logo.svg" width="256px" />

CSS (Cascading Style Sheets) are rules to describe how your HTML elements look. Writing good CSS is hard. It usually takes many years of experience and frustration of shooting yourself in the foot before one is able to write maintainable and scalable CSS. CSS, having a global namespace, is fundamentally designed for web documents, and not really for web apps that favor a components architecture. Hence, experienced front end developers have designed methodologies to guide people on how to write organized CSS for complex projects, such as using [SMACSS](https://smacss.com/), [BEM](http://getbem.com/), [SUIT CSS](http://suitcss.github.io/), etc.

However, the encapsulation of styles that these methodologies bring about are artificially enforced by conventions and guidelines. They break the moment developers do not follow them.

As you might have realized by now, the front end ecosystem is saturated with tools, and unsurprisingly, tools have been invented to [partially solve some of the problems](https://speakerdeck.com/vjeux/react-css-in-js) with writing CSS at scale. "At scale" means that many developers are working on the same large project and touching the same stylesheets. There is no community-agreed approach on writing [CSS in JS](https://github.com/MicheleBertoli/css-in-js) at the moment, and we are hoping that one day a winner would emerge, just like Redux did, among all the Flux implementations. For now, we are banking on [CSS Modules](https://github.com/css-modules/css-modules). CSS modules is an improvement over existing CSS that aims to fix the problem of global namespace in CSS; it enables you to write styles that are local by default and encapsulated to your component. This feature is achieved via tooling. With CSS modules, large teams can write modular and reusable CSS without fear of conflict or overriding other parts of the app. However, at the end of the day, CSS modules are still being compiled into normal globally-namespaced CSS that browsers recognize, and it is still important to learn and understand how raw CSS works.

If you are a total beginner to CSS, Codecademy's [HTML & CSS course](https://www.codecademy.com/learn/learn-html-css) will be a good introduction to you. Next, read up on the [Sass preprocessor](http://sass-lang.com/), an extension of the CSS language which adds syntactic improvements and encourages style reusability. Study the CSS methodologies mentioned above, and lastly, CSS modules.

**Estimated Duration: 3-4 days.** Try styling up your app using the SMACSS/BEM approach and/or CSS modules.

#### Study Links

- [Learn HTML & CSS course on Codecademy](https://www.codecademy.com/learn/learn-html-css)
- [Intro to HTML/CSS on Khan Academy](https://www.khanacademy.org/computing/computer-programming/html-css)
- [SMACSS](https://smacss.com/)
- [BEM](http://getbem.com/introduction/)
- [SUIT CSS](http://suitcss.github.io/)
- [CSS Modules Specification](https://github.com/css-modules/css-modules)
- [Sass Homepage](http://sass-lang.com/)
- [Answers to Front End Job Interview Questions — HTML](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#html-questions)
- [Answers to Front End Job Interview Questions — CSS](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#css-questions)

#### Alternatives

- [JSS](https://github.com/cssinjs/jss)
- [Styled Components](https://github.com/styled-components/styled-components)

## Maintainability

Code is read more frequently than it is written. This is especially true at Grab, where the team size is large and we have multiple engineers working across multiple projects. We highly value readability, maintainability and stability of the code and there are a few ways to achieve that: "Extensive testing", "Consistent coding style" and "Typechecking". Also when you are in a team, sharing same practices becomes really important. Check out these [JavaScript Project Guidelines](https://github.com/wearehive/project-guidelines) for instance.

## Testing - Jest + Enzyme

<img alt="Jest Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/jest-logo.svg" width="164px" />

[Jest](http://facebook.github.io/jest/) is a testing library by Facebook that aims to make the process of testing pain-free. As with Facebook projects, it provides a great development experience out of the box. Tests can be run in parallel resulting in shorter duration. During watch mode, by default, only the tests for the changed files are run. One particular feature we like is "Snapshot Testing". Jest can save the generated output of your React component and Redux state and save it as serialized files, so you wouldn't have to manually come up with the expected output yourself. Jest also comes with built-in mocking, assertion and test coverage. One library to rule them all!

![Jest Demo](images/jest-demo.gif)

React comes with some testing utilities, but [Enzyme](http://airbnb.io/enzyme/) by Airbnb makes it easier to generate, assert, manipulate and traverse your React components' output with a jQuery-like API. It is recommended that Enzyme be used to test React components.

Jest and Enzyme makes writing front end tests fun and easy. When writing tests becomes enjoyable, developers write more tests. It also helps that React components and Redux actions/reducers are relatively easy to test because of clearly defined responsibilities and interfaces. For React components, we can test that given some `props`, the desired DOM is rendered, and that callbacks are fired upon certain simulated user interactions. For Redux reducers, we can test that given a prior state and an action, a resulting state is produced.

The documentation for Jest and Enzyme are pretty concise, and it should be sufficient to learn them by reading it.

**Estimated Duration: 2-3 days.** Try writing Jest + Enzyme tests for your React + Redux app!

#### Study Links

- [Jest Homepage](http://facebook.github.io/jest/)
- [Testing React Applications with Jest](https://auth0.com/blog/testing-react-applications-with-jest/)
- [Enzyme Homepage](http://airbnb.io/enzyme/)
- [Enzyme: JavaScript Testing utilities for React](https://medium.com/airbnb-engineering/enzyme-javascript-testing-utilities-for-react-a417e5e5090f)

#### Alternatives

- [AVA](https://github.com/avajs/ava)
- [Karma](https://karma-runner.github.io/)

## Linting JavaScript - ESLint

<img alt="ESLint Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/eslint-logo.svg" width="256px" />

A linter is a tool to statically analyze code and finds problems with them, potentially preventing bugs/runtime errors and at the same time, enforcing a coding style. Time is saved during pull request reviews when reviewers do not have to leave nitpicky comments on coding style. [ESLint](http://eslint.org/) is a tool for linting JavaScript code that is highly extensible and customizable. Teams can write their own lint rules to enforce their custom styles. At Grab, we use Airbnb's [`eslint-config-airbnb`](https://www.npmjs.com/package/eslint-config-airbnb) preset, that has already been configured with the common good coding style in the [Airbnb JavaScript style guide](https://github.com/airbnb/javascript).

For the most part, using ESLint is as simple as tweaking a configuration file in your project folder. There's nothing much to learn about ESLint if you're not writing new rules for it. Just be aware of the errors when they surface and Google it to find out the recommended style.

**Estimated Duration: 1/2 day.** Nothing much to learn here. Add ESLint to your project and fix the linting errors!

#### Study Links

- [ESLint Homepage](http://eslint.org/)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)

#### Alternatives

- [Prettier](https://github.com/prettier/prettier)
- [Standard](https://github.com/feross/standard)
- [JSHint](http://jshint.com/)

## Linting CSS - stylelint

<img alt="stylelint Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/stylelint-logo.svg" width="256px" />

As mentioned earlier, good CSS is notoriously hard to write. Usage of static analysis tools on CSS can help to maintain our CSS code quality and coding style. For linting CSS, we use stylelint. Like ESLint, stylelint is designed in a very modular fashion, allowing developers to turn rules on/off and write custom plugins for it. Besides CSS, stylelint is able to parse SCSS and has experimental support for Less, which lowers the barrier for most existing code bases to adopt it.

![stylelint Demo](images/stylelint-demo.png)

Once you have learnt ESLint, learning stylelint would be effortless considering their similarities. stylelint is currently being used by big companies like [Facebook](https://code.facebook.com/posts/879890885467584/improving-css-quality-at-facebook-and-beyond/), [Github](https://github.com/primer/stylelint-config-primer) and [Wordpress](https://github.com/WordPress-Coding-Standards/stylelint-config-wordpress).

One downside of stylelint is that the autofix feature is not fully mature yet, and is only able to fix for a limited number of rules. However, this issue should improve with time.

**Estimated Duration: 1/2 day.** Nothing much to learn here. Add stylelint to your project and fix the linting errors!

#### Study Links

- [stylelint Homepage](https://stylelint.io/)
- [Lint your CSS with stylelint](https://css-tricks.com/stylelint/)

#### Alternatives

- [Sass Lint](https://github.com/sasstools/sass-lint)
- [CSS Lint](http://csslint.net/)

## Types - Flow

<img alt="Flow Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/flow-logo.png" width="256px" />

Static typing brings about many benefits when writing apps. They can catch common bugs and errors in your code early. Types also serve as a form of documentation for your code and improves the readability of your code. As a code base grows larger, we see the importance of types as they gives us greater confidence when we do refactoring. It is also easier to onboard new members of the team to the project when it is clear what kind of values each object holds and what each function expects.

Adding types to your code comes with the trade-off of increased verbosity and a learning curve of the syntax. But this learning cost is paid upfront and amortized over time. In complex projects where the maintainability of the code matters and the people working on it change over time, adding types to the code brings about more benefits than disadvantages.

Recently, I had to fix a bug in a code base that I haven’t touched in months. It was thanks to types that I could easily refresh myself on what the code was doing, and gave me confidence in the fix I made.

The two biggest contenders in adding static types to JavaScript are [Flow](https://flow.org/) (by Facebook) and [TypeScript](https://www.typescriptlang.org/) (by Microsoft). As of date, there is no clear winner in the battle. For now, we have made the choice of using Flow. We find that Flow has a lower learning curve as compared to TypeScript and it requires relatively less effort to migrate an existing code base to Flow. Being built by Facebook, Flow has better integration with the React ecosystem out of the box. [James Kyle](https://twitter.com/thejameskyle), one of the authors of Flow, has [written](http://thejameskyle.com/adopting-flow-and-typescript.html) on a comparison between adopting Flow and TypeScript.

Anyway, it is not extremely difficult to move from Flow to TypeScript as the syntax and semantics are quite similar, and we will re-evaluate the situation in time to come. After all, using one is better than not using any at all.

Flow recently revamped their homepage and it's pretty neat now!

**Estimated Duration: 1 day.** Flow is pretty simple to learn as the type annotations feel like a natural extension of the JavaScript language. Add Flow annotations to your project and embrace the power of type systems.

#### Study Links

- [Flow Homepage](https://flow.org/)
- [TypeScript vs Flow](https://github.com/niieani/typescript-vs-flowtype)

#### Alternatives

- [TypeScript](https://www.typescriptlang.org/)

## Build System - webpack

<img alt="webpack Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/webpack-logo.svg" width="256px" />

This part will be kept short as setting up webpack can be a tedious process and might be a turn-off to developers who are already overwhelmed by the barrage of new things they have to learn for front end development. In a nutshell, [webpack](https://webpack.js.org/) is a module bundler that compiles a front end project and its dependencies into a final bundle to be served to users. Usually, projects will already have the webpack configuration set up and developers rarely have to change it. Having an understanding of webpack is still a good to have in the long run. It is due to webpack that features like hot reloading and CSS modules are made possible.

We have found the [webpack walkthrough](https://survivejs.com/webpack/foreword/) by SurviveJS to be the best resource on learning webpack. It is a good complement to the official documentation and we recommend following the walkthrough first and referring to the documentation later when the need for further customization arises.

**Estimated Duration: 2 days (Optional).**

#### Study Links

- [webpack Homepage](https://webpack.js.org/)
- [SurviveJS - Webpack: From apprentice to master](https://survivejs.com/webpack/foreword/)

#### Alternatives

- [Rollup](https://rollupjs.org/)
- [Browserify](http://browserify.org/)

## Package Management - Yarn

<img alt="Yarn Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/yarn-logo.png" width="256px" />

`node_modules` 디렉토리 안을 살펴보면 그 안에 포함된 수많은 디렉토리에 기겁할 겁니다. 각각의 babel 플러그인과 lodash 함수는 그 자체가 패키지입니다. 여러 개의 프로젝트가 있는 경우 이 패키지들은 각 프로젝트에 거의 유사하게 중복 저장됩니다. 새로운 프로젝트에서 `npm install` 실행을 할 때마다, 이 패키지가 다른 프로젝트에 존재하더라도 또 다시 다운로드하게 됩니다.

또 `npm install`를 통해 패키지를 설치했을 때 '비결정성' 문제가 있었습니다. 우리 CI 빌드 중 일부가 실패했었는데, 빌드하는 시점에 의존성들을 설치하는 과정에서 일부 'breaking changes'를 포함한 마이너 업데이트 패키지를 받아왔기 때문이었습니다. 라이브러리 저자들이 [semver](http://semver.org/)를 따르고, 엔지니어들이 API 규약을 항상 존중한다는 가정을 하지 않았다면 이런 일이 발생하지 않았을 겁니다.

[Yarn](https://yarnpkg.com/)는 이러한 문제들을 해결했습니다. '비결정성' 문제는 `yarn.lock` 파일을 통해 처리되며, 어떤 시스템에 설치해도 `node_modules` 내부의 파일 구조가 동일함을 보장합니다. Yarn은 전역 캐시 디렉토리를 활용해서 이미 다운로드한 적이 있는 패키지를 다시 다운받지 않아도 되게 합니다. 또한 오프라인으로 의존성을 설치할 수 있습니다!

가장 일반적인 Yarn 명령어는 [여기에서](https://yarnpkg.com/en/docs/usage) 찾을 수 있습니다. 다른 대부분의 명령어들은 `npm` 과 유사하므로 `npm` 버전을 대신 사용해도 됩니다. 우리가 좋아하는 명령어 중 하나는 `yarn upgrade-interactive`인데, 요즘처럼 자바스크립트 프로젝트가 의존성을 많이 가지고 있는 상황에서 의존성 업그레이드를 매우 수월하게 만들어 줍니다. 확인해 보세요!

npm@5.0.0이 [2017년 5월에 출시](https://github.com/npm/npm/releases/tag/v5.0.0)되었는데, Yarn이 해결하고자 한 많은 이슈들을 해결한 것으로 보입니다. 계속 주시하세요!

**예상소요시간: 2시간**

#### Study Links

- [Yarn Homepage](https://yarnpkg.com/)
- [Yarn: A new package manager for JavaScript](https://code.facebook.com/posts/1840075619545360)

#### Alternatives

- [Good old npm](https://github.com/npm/npm/releases/tag/v5.0.0)

## Continuous Integration

We use [Travis CI](https://travis-ci.com/) for our continuous integration (CI) pipeline. Travis is a highly popular CI on Github and its [build matrix](https://docs.travis-ci.com/user/customizing-the-build#Build-Matrix) feature is useful for repositories which contain multiple projects like Grab's. We configured Travis to do the following:

- Run linting for the project.
- Run unit tests for the project.
- If the tests pass:
  - Test coverage generated by Jest is uploaded to [Codecov](https://codecov.io/).
  - Generate a production bundle with webpack into a `build` directory.
  - `tar` the `build` directory as `<hash>.tar` and upload it to an S3 bucket which stores all our tar builds.
- Post a notification to Slack to inform about the Travis build result.

#### Study Links

- [Travis Homepage](https://travis-ci.com/)
- [Codecov Homepage](https://codecov.io/)

#### Alternatives

- [Jenkins](https://jenkins.io/)
- [CircleCI](https://circleci.com/)

## Hosting - Amazon S3

Traditionally, web servers that receive a request for a webpage will render the contents on the server, and return a HTML page with dynamic content meant for the requester. This is known as server-side rendering. As mentioned earlier in the section on Single-page Apps, modern web applications do not involve server-side rendering, and it is sufficient to use a web server that serves static asset files. Nginx and Apache are possible options and not much configuration is required to get things up and runnning. The caveat is that the web server will have to be configured to route all requests to a single entry point and allow client-side routing to take over. The flow for front end routing goes like this:

1. Web server receives a HTTP request for a particular route, for example `/user/john`.
1. Regardless of which route the server receives, serve up `index.html` from the static assets directory.
1. The `index.html` should contain scripts that load up a JavaScript framework/library that handles client-side routing.
1. The client-side routing library reads the current route, and communicates to the MVC (or equivalent where relevant) framework about the current route.
1. The MVC JavaScript framework renders the desired view based on the route, possibly after fetching data from an API if required. Example, load up `UsersController`, fetch user data for the username `john` as JSON, combine the data with the view, and render it on the page.

A good practice for serving static content is to use caching and putting them on a CDN. We use [Amazon Simple Storage Service (S3)](https://aws.amazon.com/s3/) because it can both host and act as a CDN for our static website content. We find that it is an affordable and reliable solution that meets our needs. S3 provides the option to "Use this bucket to host a website", which essentially directs the requests for all routes to the root of the bucket, which means we do not need our own web servers with special routing configurations.

An example of a web app that we host on S3 is [Hub](https://hub.grab.com/).

Other than hosting the website, we also use S3 to host the build `.tar` files generated from each successful Travis build.

#### Study Links

- [Amazon S3 Homepage](https://aws.amazon.com/s3/)
- [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)

#### Alternatives

- [Google Cloud Platform](https://cloud.google.com/storage/docs/hosting-static-website)
- [Now](https://zeit.co/now)

## Deployment

The last step in shipping the product to our users is deployment. We use [Ansible Tower](https://www.ansible.com/tower) which is a powerful automation software that enables us to deploy our builds easily.

As mentioned earlier, all our commits, upon successful build, are being uploaded to a central S3 bucket for builds. We follow semver for our releases and have commands to automatically generate release notes for the latest release. When it is time to release, we run a command to tag the latest commit and push to our code hosting environment. Travis will run the CI steps on that tagged commit and upload a tar file (such as `1.0.1.tar`) with the version to our S3 bucket for builds.

On Tower, we simply have to specify the name of the `.tar` we want to deploy to our hosting bucket, and Tower does the following:

1. Download the desired `.tar` file from our builds S3 bucket.
1. Extracts the contents and swap in the configuration file for specified environment.
1. Upload the contents to the hosting bucket.
1. Post a notification to Slack to inform about the successful deployment.

This whole process is done under 30 seconds and using Tower has made deployments and rollbacks easy. If we realize that a faulty deployment has occurred, we can simply find the previous stable tag and deploy it.

#### Study Links

- [Ansible Tower Homepage](https://www.ansible.com/tower)

### The Journey has Just Begun

Congratulations on making it this far! Front end development today is [hard](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f), but it is also more interesting than before. What we have covered so far will help any new engineer to Grab's web team to get up to speed with our technologies pretty quickly. There are many more things to be learnt, but building up a solid foundation in the essentials will aid in learning the rest of the technologies. This helpful [front end web developer roadmap](https://github.com/kamranahmedse/developer-roadmap#-front-end-roadmap) shows the alternative technologies available for each aspect.

We made our technical decisions based on what was important to a rapidly growing Grab Engineering team - maintainability and stability of the code base. These decisions may or may not apply to smaller teams and projects. Do evaluate what works best for you and your company.

As the front end ecosystem grows, we are actively exploring, experimenting and evaluating how new technologies can make us a more efficient team and improve our productivity. We hope that this post has given you insights into the front end technologies we use at Grab. If what we are doing interests you, [we are hiring](https://grab.careers)!

*Many thanks to [Joel Low](https://github.com/lowjoel), [Li Kai](https://github.com/li-kai) and [Tan Wei Seng](https://github.com/xming13) who reviewed drafts of this article.*

### More Reading

**General**

- [State of the JavaScript Landscape: A Map for Newcomers](http://www.infoq.com/articles/state-of-javascript-2016)
- [The Hitchhiker's guide to the modern front end development workflow](http://marcobotto.com/the-hitchhikers-guide-to-the-modern-front-end-development-workflow/)
- [How it feels to learn JavaScript in 2016](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f#.tmy8gzgvp)
- [Roadmap to becoming a web developer in 2017](https://github.com/kamranahmedse/developer-roadmap#-frontend-roadmap)
- [Modern JavaScript for Ancient Web Developers](https://trackchanges.postlight.com/modern-javascript-for-ancient-web-developers-58e7cae050f9)

**Other Study Plans**

- [A Study Plan To Cure JavaScript Fatigue](https://medium.freecodecamp.com/a-study-plan-to-cure-javascript-fatigue-8ad3a54f2eb1#.c0wnrrcwd)
- [JS Stack from Scratch](https://github.com/verekia/js-stack-from-scratch)
- [A Beginner’s JavaScript Study Plan](https://medium.freecodecamp.com/a-beginners-javascript-study-plan-27f1d698ea5e#.bgf49xno2)

### Footnotes

<p id="fn1">1. This can be solved via <a href="https://webpack.js.org/guides/code-splitting/">webpack code splitting</a>. <a href="#ref1" title="Jump back to footnote 1 in the text.">↩</a></p>

<p id="fn2">2. <a href="https://medium.com/@mjackson/universal-javascript-4761051b7ae9">Universal JS</a> to the rescue! <a href="#ref2" title="Jump back to footnote 1 in the text.">↩</a></p>
