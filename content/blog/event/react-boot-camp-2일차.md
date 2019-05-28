---
title: React Boot Camp 2일차
date: 2019-05-29 00:05:46
category: event
---

## React Front-End Development

- 여러 UI라이브러리를 사용해보았지만 antd가 제일 좋은 경험이였다.
- antd의 기본 구성은 less로 이루어져 있어서 less 사용한다.

### CRA 왜 써야 하나?

- CRA 없이 React프로젝트 만들려면 해야 하는일
- index.html 형식에 맞도록 만들기
- webpack 설정
- babel 설치 설정
- react 설치 설정
- TypeScript 설치 설정
- HMR 설치

### React + Typescript 프로젝트 셋팅

```javascript
npm init react-app react-fed2 --typescript

or

create-react-app react-fed2 --typescript
```

- CRACO 사용 하여 less-loader 연결

- antd 설치 styled components 의존

```javascript
npm i antd styled-components @types/styled-components
```

### Ant Design 설치 및 환경 구성

- less-loader에 inline javacript enable true 해주어야함
- 팁
  - ant-btn이라는 프롭을가진다 앤트의 버튼 컴포넌트는
  - styled컴포넌트 안에서 안쪽에 앤트 컴포넌트 를 스타일링 할 수 있다.
- TS 제네릭
  - 프롭은 내가 정의한 프롭으로 만들어

### 데모 프로젝트 만들기

- antd는 자동으로 폼크리에트로 만들면 자동으로 스테이트를 바인딩 해준다.
- validator가 환상적이다. 기존 프로젝트보다 validator가 너무나도 잘 완성되어져있다.
- npm을 선택할때 디펜던시를 꼭 체크해야 한다. 적은것일 수록 문제가 적을 확률이 높다.

### 효율적인 코드 작성 ( 구조분해할당, Async / Await, Type)

- 구조분해할당
- 모달을 프로미스로 만들어 봅시다!
  - 모달에서 프로미스를 사용하는 이유는 모달이 나타난 상태에서 동기적으로 동작하게 하기 위해 사용

### 드래그 & 리사이징 (resizing, ellipsis, Drag & drop, Sub component auto resizing)

- :after 수도 엘리먼트 가상의 엘리먼트를 만들어 준다. 조절해야될 대상이 있으면 after를 이용해 조작하기 쉽게 한다.
- onDrag를 쓰면 제어 못하는 문제가 생긴다. ⇒ 드래그 되는 대상의 허상이 따라다니는 문제가 생긴다.
- mouseDown으로 움직이면 셀렉트가 된다. 이걸 막는것도 고려해야 한다. 어떻게 선택을 막을것인가 보시죠!
- constructor 가 있는 함수에선 state를 constructor에서 정의 해주는 것이 좋다. TS를 사용할 경우 가끔 constructor 밖에서 state를 초기화 할 경우 오류가 난다.
- throttle, debounce 이 경우에선 throttle을 적용 해준다.
- 리액트의 고질병은 돔개수가 많아지면 버벅임이 심하다 따라서 보이는 부분만 렌더하는게 중요하다.

### 질문

- 왜 less로 불러와서 사용하나요?
  - 커스터마이징해서 빌드하기 위해서
  - 코어한부분만 불러오거나 선택적으로 불러오게 할 수도 있다.
- 회사에서 겪은일인데 타입스크립트의 인터페이스를 파일내에서 만 사용하거나 따로 분류 하는경우가 있는데 어떠한 경우에 분류하나요?
  - 재사용 되야 되는거는 별도의 폴더에 옮기고 재사용 되지 않는것은 그 파일안에만 있는게 맞다
  - 리액트 프롭이나 스테이트는 해당 컴포넌트 외에 쓸 일이 없기 때문에 파일안에 있는게 맞다.
  - namespace이용해서 enum쓰니까 좋았다.
- 폼 양식 처리 관련해서 궁금한점 일반적으로 인풋 타입 텍스트는 쉬운데 라디오나 다른 타입은 어떻게 쓰는 지? 포믹 어떤지?
  - antd는 다른 타입의 인풋 또한 json으로 다 뽑아줘서 편하다.
  - formik 그래서 안써봤다.

### 후기

- TypeScript는 튜토리얼 정도만 해보아서 중간 중간 따라가기 어려운 부분이 있었다. module declare 부분이나 d.ts가 존재하지 않는 서드파티의 경우 컴파일 단계에서 에러가나는데 이유 조차몰라 애먹었다. 하지만 구글의 도움을 받아 겨우 겨우 해결해 수업을 따라 간 것 같다.
  - typescript 사용 할 경우 불러오는 모듈을 정의하는 파일 또한 설치를 해주어야 한다. antd라는 라이브러리를 쓰게되면 @types/antd 또한 받아주어야한다. 하지만 지원하지 않는 라이브러리도 존재한다. 이럴 경우 직접 만들거나 tsconfig의 `compilerOptions`에 `"noImplicitAny": false` 설정을 해주면 컴파일 된다.
- typescript를 짧게나마 겪어본 경험은 꽤 괜찮았다. Eclipse에서 자바를 사용한 경험이 떠오르기도 했다. ctrl + 클릭으로 해당 펑션의 인터페이스를 d.ts 파일에서 확인하면서 개발하고 자동완성 되는 부분이 편리하기도 했다.
- 가장 좋은점이라 느꼈던점은 강사님께서 강의 중 함수의 인자로 type이 맞지 않는 변수를 할당 해주었는데 바로 빨간줄이 뜨면서 IDE에서 알려주어 런타임중 확인하는것이 아닌 IDE에서 타이핑중 바로 알 수 있다는 점에서 정적 타입 언어에서의 장점이 확 와닿았다.
- antd를 이용해 form을 처리하는 방식에 대해서 매우 극찬을 아끼시지 않았다. 간단하게 사용법 정도는 따라 해 보았지만 오히려 기존 방법에 비해 복잡해 보였다. (매직이 많아 보인다 해야하나..) 조금 분석이 필요할 것 같다.
- `craco`를 이용해 cra App을 eject하지 않고 less module을 불러와주는 config를 추가하는 법을 익혔다

> 귀중한 시간 내주셔서 경험과 지식을 공유해주신 김동우, 장기영 개발자님들께 감사드린다!
