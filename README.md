Grab 프론트엔드 가이드
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

## 상태 관리 - Flux/Redux

<img alt="Redux Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/redux-logo.svg" width="256px" />

앱이 커질수록, 앱구조가 조금 엉망이 될 수도 있습니다. 앱 전반의 컴포넌트는 공통 데이터를 공유하고 표시해야하지만 React에서 이를 처리 할 수 있는 우아한 방법이 없습니다. 결국 React는 뷰 레이어 일 뿐이며 모델 및 컨트롤러와 같이 기존 MVC 패러다임으로 앱의 다른 레이어를 구성하는 방법을 지정하지 않습니다. 이를 해결하기 위한 노력으로 페이스북은 단방향 데이터 흐름을 활용하여 React의 뷰 컴포넌트를 보완하는 앱 아키텍처인 Flux를 개발하였습니다. Flux가 어떻게 작동하는지 [이 링크](https://facebook.github.io/flux/docs/in-depth-overview.html)에서 더 자세히 일어보십시오. 요약하면, Flux 패턴은 다음과 같은 특성을 같습니다.

- **단방향 테이터 흐름** - 업데이트를 쉽게 추적할 수 있으므로 앱을 더 예측 가능하게 만듭니다.
- **관심사의 분리** - Flux 아키텍처의 각 부분은 명확한 책임을 가지고 있고 고도로 분리되어 있습니다.
- **선언적 프로그래밍과 잘 동작합니다** - 저장소는 상태간에 뷰를 전환하지 않고 뷰에 업데이트를 보낼 수 있습니다.

Flux는 본질적으로 프레임워크가 아니기 때문에 개발자는 Flux 패턴의 구현을 많이 찾아 내려고 노력했습니다. 결국 [Redux](http://redux.js.org/)가 분명한 승자가 되었습니다. Redux는 Flux, [Command 패턴](https://www.wikiwand.com/en/Command_pattern) 및 [Elm 아키텍처](https://guide.elm-lang.org/architecture/)의 아이디어를 결합하였고 요즘 React에서 개발자가 사용하는 사실상의 상태 관리 라이브러리 입니다. 핵심 개념은 다음과 같습니다.

- 앱 **상태**는 단일의 간단한 오래된 자바스크립트 객체(POJO)에 의해 설명됩니다.
- 상태 수정을 위한 액션(또한 POJO)를 전달합니다.
- **Reducer**는 현재 상태와 동작을 가져와서 새로운 상태를 생성하는 순수한 함수입니다.

개념은 간단하게 들리지만, 앱이 다음을 수행할 수 있게 해주므로 정말 강력합니다.

- 서버에서 렌더링 된 상태를 가지고 클라이언트에서 시작할 수 있습니다.
- 전체 앱에서 로그와 backtrack의 변경을 추적할 수 있습니다.
- '실행 취소'/'다시 실행' 기능을 쉽게 구현할 수 있습니다.

Redux의 창시자인 [Dan Abramov](https://github.com/gaearon)는 Redux에 대한 자세한 문서를 작성하였고, Redux를 배우기 위한 [기본](https://egghead.io/courses/getting-started-with-redux)과 [고급](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) 과정의 동영상 튜토리얼을 만드는데 큰 도움을 주었습니다. 그것들은 Redux 학습에 매우 유용한 자료입니다.

**뷰와 상태 결합**

Redux는 반드시 React와 함께 사용할 필요는 없지만, 서로 잘 어울리기 때문에 매우 추천됩니다. React와 Redux는 공통된 아이디어와 특징을 가지고 있습니다.

- **기능 구성 패러다임** - React는 뷰(순수 함수)를 작성하고 Redux는 순수 reducer(역시 순수 함수)를 작성합니다. 출력은 동일한 입력을 기준으로 예측이 가능합니다.
- **쉬운 추론** - 이 용어를 여러번 들었을 수도 있지만 실제로 의미하는 것이 무엇인지 아십니까? 우리는 코드를 제어하고 이해하는 것으로 해석합니다 - 우리의 코드는 예상대로 작동하고 문제가 발생하면 쉽게 찾을 수 있습니다. 우리의 경험으로 봤을때 React와 Redux는 디버깅을 더 간단하게 만듭니다. 데이터 흐름이 단방향이므로 데이터 흐름 (서버 응답, 사용자 입력 이벤트)을 추적하는 것이 더 쉽고 문제가 발생하는 계층을 쉽게 결정할 수 있습니다.
- **계층화된 구조** - 앱의 각 레이어에서 Flux 아키텍처는 순수 함수이고 분명한 책임이 있습니다. 순수 함수에 대한 테스트를 작성하는 것은 상대적으로 쉽습니다. reducer에 앱의 변경 사항을 집중시켜야하며 변경 사항을 트리거하는 유일한 방법은 액션을 전달하는 것입니다.
- **개발 경험** - [Redux DevTools](https://github.com/gaearon/redux-devtools)과 같이 개발 중에 앱을 디버깅하고 검사하는데 도움이 되는 많은 도구가 만들어졌습니다. <br> ![Redux Devtools Demo](images/redux-devtools-demo.gif)

당신의 앱은 원격 API 요청과 같은 비동기 호출을 처리해야할 가능성이 큽니다. [redux-thunk](https://github.com/gaearon/redux-thunk)와 [redux-saga](https://github.com/redux-saga/redux-saga)은 이러한 문제를 해결하기 위해 만들어졌습니다. 함수형 프로그래밍 및 생성기에 대한 이해가 필요하기 때문에 이해하는데 약간의 시간이 걸리 수 있습니다. 우리는 그것을 필요할 때만 사용하라고 충고합니다.

[react-redux](https://github.com/reactjs/react-redux)는 Redux가 바인딩된 공식적인 React고 배우기 매우 쉽습니다.

**예상 소요시간: 4일** Egghead 코스는 시간이 좀 걸리지만 시간을 투자할 가치가 있습니다. Redux를 배우고나면, 당신이 만든 React 프로젝트에 그것을 통합할 수 있습니다. 순수한 React에서 고생하고 있는 상태 관리 문제 일부를 Redux가 해결해줍니까?

#### Study Links

- [Flux Homepage](http://facebook.github.io/flux)
- [Redux Homepage](http://redux.js.org/)
- [Egghead Course - Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux)
- [Egghead Course - Build React Apps with Idiomatic Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)
- [React Redux Links](https://github.com/markerikson/react-redux-links)
- [You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)

#### Alternatives

- [MobX](https://github.com/mobxjs/mobx)

## 스타일이 살아있게 코딩하기 - CSS Modules

<img alt="CSS Modules 로고" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/css-modules-logo.svg" width="256px" />

CSS(Cascading Style Sheets)는 작성한 HTML 요소들을 어떻게 표시할지 서술하는 규칙입니다. CSS 잘 쓰기는 어렵습니다. 유지보수와 확장이 쉬운 CSS를 작성할 수 있게 되기 까지는 다년간의 경험과 내 발등을 찍는 좌절을 겪기 마련입니다. CSS는 웹문서용으로 설계되는 바람에 전역 네임스페이스를 갖습니다. 컴포넌트 구조의 웹앱과는 잘 맞지 않지요. 그래서 경험 많은 프론트엔드 개발자들은 복잡한 프로젝트에서 CSS를 정리하는 방법론들을 만들었습니다. 예를 들어 [SMACSS](https://smacss.com/), [BEM](http://getbem.com/), [SUIT CSS](http://suitcss.github.io/) 같은 것들이 있습니다.

하지만 이러한 방법론들이 제안하는 스타일 체계는 관습과 가이드라인에 의하여 인공적으로 주어진 것입니다. 개발자들이 관습을 어기는 순간 체계는 무너지고 맙니다.

지금쯤이면 눈치채셨겠지만, 프론트엔드 생태계는 넘쳐나는 도구들로 포화상태입니다. 당연히 대규모로 CSS 작업을 하면서 생기는 문제들을 [부분적으로나마 풀기위한 것들](https://speakerdeck.com/vjeux/react-css-in-js)도 많이 발명되었습니다. "대규모"라는 건 많은 개발자가 하나의 커다란 프로젝트에 참여하고, 같은 스타일시트를 고친다는 의미입니다. 지금은 [JS내에 CSS를 넣는 방법](https://github.com/MicheleBertoli/css-in-js)에 대하여 확립된 관습은 없습니다. 언젠가는 승자가 나오기를 기대해봅니다. Redux가 다른 Flux 구현체들을 물리친 것처럼요. 그랩은 현재로서는 [CSS Modules](https://github.com/css-modules/css-modules)를 선택했습니다. CSS Modules는 전역 네임스페이스 문제를 해결하기 위하여 기존의 CSS를 개선한 것입니다. 컴포넌트로 감싸서 내부에서만 유효한 스타일을 작성할 수 있습니다. 이런 기능은 도구체계를 이용하여 구현되었습니다. CSS Modules를 사용하면, 대규모 팀이 모듈화 되고 재사용하기 쉬운 CSS를 작성할 수 있습니다. 충돌과 부작용의 공포 없이 말이지요. 하지만 결국에는 CSS Modules도 브라우저가 인식할 수 있는 일반적인 전역 네임스페이스 CSS로 컴파일하게 됩니다. 기본 CSS가 어떻게 동작하는지 이해하는 것은 여전히 중요하겠지요.

만약 CSS가 처음이라면, Codecademy의 [HTML과 CSS 수업](https://www.codecademy.com/learn/learn-html-css)으로 시작하면 좋습니다. 그리고, [Sass전처리기](http://sass-lang.com/)를 공부하세요. 더 나은 문법을 제공하고 재사용을 쉽게 만들어주는 CSS언어의 확장인 언어입니다. 이런 CSS의 방법론을 먼저 공부한 후, 마지막으로 CSS Modules를 살펴보세요.

**예상소요시간: 3-4일.** 당신의 앱에 SMACSS/BEM, 그리고 CSS Modules를 적용해보세요.

#### Study Links

- [Codecademy의 HTML & CSS 코스](https://www.codecademy.com/learn/learn-html-css)
- [Khan Academy의 HTML/CSS 개론](https://www.khanacademy.org/computing/computer-programming/html-css)
- [SMACSS](https://smacss.com/)
- [BEM](http://getbem.com/introduction/)
- [SUIT CSS](http://suitcss.github.io/)
- [CSS Modules 문서](https://github.com/css-modules/css-modules)
- [Sass 홈페이지](http://sass-lang.com/)
- [프론트엔드 개발자 면접 질문들에 대한 답 — HTML](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#html-questions)
- [프론트엔드 개발자 면접 질문들에 대한 답 — CSS](https://github.com/yangshun/tech-interview-handbook/blob/master/front-end/interview-questions.md#css-questions)

#### Alternatives

- [JSS](https://github.com/cssinjs/jss)
- [Styled Components](https://github.com/styled-components/styled-components)

## 유지보수하기

코드는 한 번 작성하지만, 여러 번 읽게 됩니다. Grab에서는 특히 더 그렇습니다. 팀의 크기가 크고, 여러 엔지니어들이 다양한 프로젝트에 교차로 참여하니까요. 우리는 읽기 쉽고, 유지보수하기 쉬우며, 안정된 코드의 가치를 믿습니다. 이를 달성하기 위한 방법이 몇 가지 있지요. "아주 많은 테스트", "일관된 코드 형식", "타입 체크". 팀으로 일할 때는 동일한 관습을 공유하는 것이 매우 중요합니다. 예를 들어 [자바스크립트 프로젝트 가이드라인들](https://github.com/wearehive/project-guidelines)을 살펴보세요.

## 테스트 - Jest + Enzyme

<img alt="Jest Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/jest-logo.svg" width="164px" />

[Jest](http://facebook.github.io/jest/)는 고통없는 테스트 과정을 만드는 것을 목표로하는 페이스북의 테스트 라이브러리입니다. 페이스북 프로젝트와 마찬가지로, 그것은 뛰어난 개발 경험을 제공합니다. 테스트를 병렬로 실행할 수 있기 때문에 짧은 시간에 수행될 수 있습니다. 기본적으로 watch 모드에서는 변경된 파일에 대한 테스트만 실행됩니다. 우리가 좋아하는 기능 중 하나는 "스냅샷 테스팅"입니다. Jests는 React 구성요소 및 Redux 상태를 생성된 출력 파일을 저장하고, serialized된 파일들을 저장할 수 있으므로, 수동으로 예상 출력을  가져올 필요가 없습니다. Jest는 mocking, assertion 그리고 테스트 커버리지가 내장되어 있습니다. 하나의 라이브러리로 그것들 모두 지정 가능합니다.

![Jest Demo](images/jest-demo.gif)

React에는 몇 가지 테스트 유틸리티가 있지만, Airbnb의 [Enzyme] http://airbnb.io/enzyme/)을 사용하면 jQuery와 같은 API를 사용하여 React 구성요소의 출력을 생성하고, assert하고, 조작하고 traverse 할 수 있습니다. React 구성요소를 테스트하기 위해서는 Enzyme를 사용하는 것을 추천합니다.

Jest and Enzyme은 프론트 엔드 테스트를 재미있고 쉽게 작성합니다. 테스트를 작성하는 것이 즐거워지면 개발자는 더 많은 테스트를 작성합니다. 또한 React 구성 요소와 Redux 액션/리듀서는 명확하게 정의된 책임과 인터페이스로 인해 테스트하기가 쉽습니다. React 구성요소의 경우 일부 'props', 주어진 DOM이 렌더링 되고, 특정 시뮬레이션된 사용자 인터렉션시 콜백이 발생한다는 것을 테스트할 수 있습니다. Redux 리듀서의 경우 이전 상태와 액션 실행된 결과 상태가 생성되는지 테스트 할 수 있습니다.

Jest와 Enzyme에 대한 문서는 매우 간결하며, 그것을 읽음으로써 그것들을 학습하기에 충분합니다.

**예상소요시간: 2-3 일.**  당신의 React와 Redux 앱에 Jest 와 Enzyme 테스트를 작성하세요!


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

linter는 정적으로 코드를 분석하고 문제를 찾아 잠재적으로 버그/런타임 에러를 방지하는 동시에 코딩 스타일을 적용하는 도구입니다. 리뷰어가 코딩 스타일에 사소한 의견을 남겨 둘 필요가 없는 경우 pull request 리뷰에 시간이 절약됩니다. [ESLint](http://eslint.org/)는 확장성이 높고 사용자 정의가 가능한 JavaScript 코드를 linting 하기 위한 도구입니다. 팀은 팀의 사용자 정의 스타일을 적용하기 위해 자체 lint 규칙을 작성할 수 있습니다. Grab에서 우리는 현재 [Airbnb JavaScript style guide](https://github.com/airbnb/javascript) 에서 공통의 좋은 코딩 스타일로 구성된 Airbnb의 [`eslint-config-airbnb`](https://www.npmjs.com/package/eslint-config-airbnb) 사용하고 있습니다.

대부분의 경우, ESLint를 사용하는 것은 프로젝트 폴더의 구성 파일을 조정하는 것처럼 간단합니다. 새로운 규칙을 작성하지 않으면 ESLint에 대해 배울 점이 많지 않습니다. 오류가 발생했을 때 인식하고, Google에서 권장 스타일을 찾아야합니다

**예상소요시간: 1/2 일.**  별로 배울 것이 없습니다. 당신의 프로젝트에 ESLint를 추가하고, linting 에러를 수정하세요!


#### Study Links

- [ESLint Homepage](http://eslint.org/)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)

#### Alternatives

- [Prettier](https://github.com/prettier/prettier)
- [Standard](https://github.com/feross/standard)
- [JSHint](http://jshint.com/)

## CSS 검사 - stylelint

<img alt="stylelint 로고" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/stylelint-logo.svg" width="256px" />

앞에서 말했듯이, CSS는 잘 쓰기 어렵기로 악명 높습니다. CSS에 정적분석도구를 사용하면, 코드 품질과 스타일 유지에 도움이 됩니다. CSS 검사를 위해 우리는 stylelint를 사용합니다. ESLint와 비슷하게, stylelint는 모듈화된 구성을 가지고 있습니다. 규칙을 끄거나 켤 수 있고, 플러그인을 만들 수도 있습니다. CSS 외에 SCSS도 지원하고, 실험적인 기능이지만 Less도 지원합니다. 기존에 존재하는 대부분의 코드에 적용하기 더 쉬울 겁니다.

![stylelint 데모](images/stylelint-demo.png)

ESLint를 배웠다면, 비슷한 stylelint를 배우는 것도 쉽습니다. stylelint는 현재 [Facebook](https://code.facebook.com/posts/879890885467584/improving-css-quality-at-facebook-and-beyond/), [Github](https://github.com/primer/stylelint-config-primer), [Wordpress](https://github.com/WordPress-Coding-Standards/stylelint-config-wordpress) 같은 큰 회사들에서도 사용하고 있습니다.

stylelint의 한 가지 단점은, 아직 자동교정 기능이 완전하지 않고, 몇 가지 제한된 숫자의 규칙에 따라서만 고칠 수 있다는 것입니다. 시간이 지나면 점차 해결될 문제입니다.

**예상소요시간: 1/2 일.**  별로 배울 것이 없습니다. 프로젝트에 stylelint를 적용하고, 오류를 수정하세요!

#### Study Links

- [stylelint 홈페이지](https://stylelint.io/)
- [stylelint로 CSS 검사하기](https://css-tricks.com/stylelint/)

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

## 빌드 시스템 - webpack

<img alt="webpack Logo" src="https://cdn.rawgit.com/grab/front-end-guide/master/images/webpack-logo.svg" width="256px" />

이번 파트는 webpack 설정이 지루한 과정 일 수 있고, 프론트엔드 개발을 위해서 배워야 하는 새로운 것들에 대한 장벽에 의해 압도당하는 개발자들에게 흥미를 잃게 만들 수 있으므로 짧게 작성됩니다. 요약하면, [webpack](https://webpack.js.org/)은 프론트엔드 프로젝트와 그 의존성을 최종 번들로 컴파일하여 사용자에게 제공하는 모듈 번들러입니다. 보통, 프로젝트는 이미 webpack 환경구성이 설정되어 있고, 개발자는 거의 변경할 필요가 없습니다. webpack에 대한 이해가 있다면 장기적으로 좋습니다. 그것은 hot reloading과 css 모듈과 같은 webpack 기능을 가능하게 만들어주기 때문입니다.

우리는 SurviveJS의 [webpack walkthrough](https://survivejs.com/webpack/foreword/)가 webpack을 학습하는데 최고의 자료라는 것을 발견했습니다. 그것은 공식 문서에 대한 좋은 보완이며, 우리는 walkthrough를 먼저 학습하고, 추가적으로 사용자 정의가 필요할 때 공식 문서를 참조하는 것을 추천합니다.

**예상소요시간: 2 일 (선택적)**  


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
