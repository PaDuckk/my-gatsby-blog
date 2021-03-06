---
title: React Boot Camp 1일차
date: 2019-05-27 23:05:70
category: event
---

## React Boot Camp 1일차

> React 시작 부터 배포 까지

### SPA

- 서버에 URL로 요청해 전체 페이지를 새로 로드 하는 것이 아닌 동적으로 현재 페이지를 다시 작성하여 사용자와 상호작용하는 웹 APP, 웹 사이트
- 연속적인 페이지 사이의 사용자 경험을 방해하지 않는다.

### React

- Virtual DOM

  - 빠른 렌더링

- Component

  - 높은 재사용성 & 빠른 개발

- JSX (Javascritp eXtension)
  - 선언적, 직관적, 높은 생산성
  - Javascript에서 XML문법이 사용 가능함 HTML은 아니다.

### React 시작하기

- CRA

  - 원하는 대로 설정하기 위해선 eject 해야 함, 되돌릴 수 없기 때문에 커밋 후 eject 가능하다.

- next.js

  - SSR, CodeSpliting, HMR, 이 3가지를 도와주는 프레임 워크
  - 페이지 단위로 라우팅이 강제되는것이 단점이 될 수 있다.

### Props & State

- `Props`
  - 단방향 데이터 바인딩
  - 부모 컴포넌트로부터 전달 된다.
  - 왜 불변객체이여야 하나?
    - 프롭스 자체의 주소값이 바뀌면 리렌더링 되어진다.
    - 키를 만드는 이유는 컴포넌트와 컴포넌트를 구분하기 위해 사용, 키 또한 불변성을 지켜주어야 올바른 사용법이다.
- `State`
  - 컴포넌트의 로컬 상태
  - state는 변경 가능하고 setState메서드를 사용해 변경

### JS or TS?

> TypeScript를 도입하여 사전에 버그를 줄이는 것에 큰 도움이 된다.

- 장점
  - 생산성 (IDE에서 도와주는 부분이 있다)
  - 안정성
  - 편의성
- 단점
  - 러닝커브

### 상태 관리 `Redux` or `MobX`

`Redux`

- Redux는 state를 관리하기 위한 거대한 이벤트 루프
- 액션 = 이벤트
- 리듀서 = 이벤트에 대한 반응

`MobX`

- Mobx는 일관성 없는 상태를 만들수 없도록 상태관리를 간단하게 만듬
- functional reactive Programming (TFRP)
- React의 `setState`가 필요하지 않습니다.

### 프로젝트 폴더 구성 `Components` or `Atomic`

- 정답은 없다. 다양하게 시도해보고 팀원간의 의견이 맞는 방향으로 하는게 정답

### React 브라우저 라우팅

- 브라우저 라우팅
  서버로 URL 요청을 보내지 않고 브라우저 `history API`를 이용해 URL이 변경되고 URL에 따라 페이지가 변경 됨

### 높은 생산성을 위한 HMR: react-hot-loader

> `webpack HMR`

- HMR != live reload

> `React Hot Loader`

- React Component를 핫스왑 해주기 때문에 높은 생산성 때문에 필수로 사용
- 소스코드의 변경이 생기더라도 현재 개발중인 App의 상태를 유지하면서 개발 가능 해 진다.
- 사용법 : hot이라는 함수로 컴포넌트를 감싸준다. 코드스플리팅이나 어싱크하게 동작하는 경우엔 다 hot으로 감싸주어야 한다.
- next 6버전이상에선 포함 되어있다.

### 검색 엔진을 위한 SSR (Server side rendering)

- 웹서비스는 검색엔진에 검색되는게 중요하다.
- SPA는 SEO에 불리함
- SSR 하는 이유
  - 검색엔진
  - 소셜미디어에서 페이지가 공유 될 때 `오픈그래프`가 중요함 `썸네일`과`description`
  - 퍼포먼스
- hydrate사용시 SSR 페이지 과 CSR 페이지가 다른경우 다시렌더함
  - 가장 실수 하는 경우
    - 키가 다른경우
    - mutable한 데이터를 쓰는 경우
    - 라이프사이클은 SSR에선 한번밖에 돌지 않기 때문에 라이프사이클을 이용한 렌더를 하는 경우
- CRA를 이용해 SSR을 하는 경우 매우 어렵..
- next를 이용해 SSR을 하면 편하다
  - getInitialProps url에 해당되는 첫번째 컴포넌트에서만 사용하다는 단점이 있다.

### 배포하기: S3 or Github or ECS or Lambda

- S3
  - 매우 저렴 비용
  - SSR 불가로 정적 페이지에 적합
  - `404 fallback`을 index.html을해놓는 방식으로 호스팅
- Github
  - 오픈소스이면 무료, 글로벌 CDN으로 매우 빠름
  - CNAME을 이용해 Domain과 연결 햊 ㅜㄹ 수 잇다.
- Heroku
  - Cloud Application Platform
  - CLi를 통해 쉽게 배포 가능
  - SSR가능
  - Free (US only)(sleeps after 30 mins of inactivity)
- Zeit: Now
  - SSR 가능, 스케일업 쉬움
  - Free, Unlimited, Pay as you Grow
  - 다른 클라우드에 비해 비용은 비싸지만 값어치가 있다.
- 람다 (서버리스 방식으로 배포가능)
  - 어렵다
  - Now 쓰자
- ECS
  - 쿠버네티스에 비해 쉽다
  - 도커를 잘지원해주어서 편리함
  - 스케일업 쉬움, 무중단 배포 쉬움
  - Docker 컴포즈 지원
  - Docker 사전 지식 필요

### 질문

- 배포

  - 리액트의 렌더가 스트림으로 이제 렌더가됨.
  - 노드인스턴스는 싱글 프로세스 싱크하게 동작하는 코드에서는 블락이 생김
  - API랑은 분리하는게 좋다.
  - SSR에서 렌더프로세스가 여러개 띄울수있게해야 확장성에서 좋다.

- MobX

  - mobX는 런타임에도 타입체크 가능하다는 장점이 있다.
  - 리덕스에선 리듀서 잘못 짤 경우 스토어가 아예 깨질 수 있다.

- 컨텍스트
  - 뮤터블하고 스파게티 코드가 되기 쉽다.
  - 금방 복잡해지고 더러워짐
  - 컨슈머가 연결된 컴포넌트가 많을때 스토어에 있는 스테이트가 하나만 바뀌어도 모든 컨슈머에 있는게 리렌더링 되는 문제도 있음.

### 보너스

> 해당 프로젝트롤 공부해보고 직접 실행해보면 큰 공부가 될거다.

- zeit/Hyper
- Imsomnia
