---
title: React Boot Camp 4일차
date: 2019-06-03 16:06:56
category: event
---

- 아폴로로 데이터를 요청해 컴포넌트에 바인딩하는게 목표이다.
- 데이터 말고도 로딩중이나 에러에 대한 상태관리도 해주기때문에 리덕스나모브엑스로 미들웨어로 적용해야되는 부분이 적어진다.

### 그래프 큐엘

- 스키마는
  - 제공할 데이터를 묘사
  - 주로 서버에서 사용
  - 타입을 지정해준다
- 스키마를 지정해주면 아폴로클라이언트에서 쿼리를 날려 데이터를 긁어 온다.
- type 뒤에 !를 붙이면 null이 될 수 없다. ts에서도 일치
- 데이터를 읽는 그래프큐엘과 데이터를 변경하는 그래프큐엘(mutation) 크게 보면 두가지가 존재
- mutation은 병렬적으로 처리하는것과 한번에 하나씩처리하는것에는 차이가 있다.
- 프래그먼트라는게 있다. 코드 재사용과 관련이 있음

### 아폴로

- 아폴로 클라이언트에 대해 살펴 봅시다.
- 아폴로로 리액트 앱을 만드는 과정을 설명드릴께요
- Apollo client는 인터페이스에요 얘가 가장 핵심이 된다는것만 알고 크게 손대진않을거에요 아폴로클라이언트가 존재하기 위해선 무언가가 있어야한다.
- React Apollo
- Apollo Cache
  - 쿼리의 결과를 저장한다 요청을 보내기전에 캐시에 필요한 데이터가 있는지 확인하고 재사용 합니다.
  - 데이터 재사용에 있어 유용한 부분이 있다.
- Apollo Link
  - 외부 GraphQL 요청에 대한 인터페이스를 제공합니다. 통신레이어를 추상화하고 상황에 따라 서버없이 사용 할 수 있다.
- fetchPolicy
  - 클라이언트 쿼리요청시 fetchPolicy라는 프로퍼티로 지정
  - cache-first, cache-only, network-only.. 등
- Query Component
  - HOC에서 renderprops방식으로바뀌었다.
  - 3버전대에선 훅으로 바뀔 예정이다.
- Persist Cache
- Persisted Query Link
  - http link에 무언가 확장을 한다. 그 다음 링크를 붙인다
  - 그렇게 되면 쿼리를 안보내고해시를 보낸다. 서버가 해시를 알아들으면 무슨 해시인줄 알겠죠?
  - 해시보고 무슨 쿼리인줄아니까 해석해서 데이터를 보내준다.
  - data 패킷을 줄일수 있는 획기적인 아이디어이다.
- Batch HTTP Link
  - 여러 GraphQL 요청을 한 번에 처리하여 서버와의 통신 횟수를 줄입니다.
  - 단점
    - 느린 Query가 전체에 영향을 준다.
    - 네트워크 수준 디버깅이 불편하다
    - CDN에서 캐시하기 어렵다
    - HTTP2를 지원하면 의미가 없다.
- schemaLink라는게 있다.

  - 스키마가 서버에 있는게 자연스럽지만 클라이언트에도 있을수 있다.
  - 그래프큐엘 서버 쪽 코드에 resolver랑 typeDef를 조합하면 데이터를 뽑아오는 로직이 나온다.
  - typedef는 타입선언
  - resolver는 실제 데이터를 주는 로직이다.
  - 리졸버에는 db에서 데이터를 긁어오는 로직이 들어가있는데 이부분을 API요청으로 바꾸어 활용할 수 있다. (SSR이나 등등)

- 그래프큐엘 파일에서 타입을 빼올 수 있다.

```javascript
// undefined일 수 있는 객체를 참조할 경우 exception 대신 undefiend가 나오게 할 수 있다.
function safe(f) {
  try {
    return f()
  } catch {
    return undefined
  }
}
```

- 페이지네이션 구현 트릭

  - 드래그해서 조금 남으면 호출한다 하는 방식으로 주로 개발함
  - 하지만 다른 팁이있다.
  - 리스트중 맨 마지막 EL을 타겟팅후 걔가 화면에 보이기 직전이면 페이지네이션을 실행해줄거에요
  - intersectionObserver라는 기능이 있다. html의 기능이다. 인자를 2개받는다. 함수, 옵션
  - 옵션이 화면에 뜨느 순간 콜백함수를 실행시킬수 있다.

- 키워드 알려드릴께요!

  - 그래프큐엘에서 쿼리랑 뮤테이션이 있지만 서브스크립션 도있다. 서버푸시라 생각하면 된다.
  - 서버에 대해선 이야기하지 않았다. 스키마링크가 있으면 클라이언트에서 서버없이 사용할 수 있다.

### 질문

- Redux나 MobX를 대체해 로컬에서 글로벌한 상태관리를 Apollo로 대체 할 수 있나요?
  - 아폴로를 리덕스나 모브엑스를 대신에 사용할 수 는있다. 하지만 액션대신에 graphQL 작성해야 된다.
  - 제생각엔 아폴로의 강점은 알아서 데이터바인딩을 지원한다. 강타입을 지원하기 때문에 실수 할경우 피드백을 받기 쉽다.
  - 로컬리졸브라는 기능이 아폴로에 있다. 그래프큐엘에서 제공하는 타입체킹을지원하지 않음
- MobX 그래프큐엘을 보았다.보셧나요?
  - 아뇨
- 아폴로 구성할때 클라이언트 구성할때 쿼리방식이랑 프로파이더를 사용하는 방식 어떤 차이가 있나
  - 두번째 방식이 좀 더 리액트에 친화적인 인터페이스를 제공하기때문에 리액트를 사용하면 두번째 방식을 사용하는게 좀더 옳다.
- 스키마를 보면은 모든 스키마가 다 나오자나요 일부만 보여줄 수 있나요?
  - 아폴로 자체에서 스키마를 노출시키지 않게 하는 방법이 있다.

### 후기

- GraphQL과 Apollo 말로만 들었지 실제 접해본 경험은 처음이었다. 꽤 난이도가 있는 내용이었다.
- 기존 WEB API와 다르게 URL과 HTTP Method로 데이터의 조회, 변경을 요청했다면 하나의 API 주소에 GraphQL이라는 Query Language를 이용해 데이터를 조회, 변경한다.
- 아직 RestAPI에 비해 어떠한 장점을 갖는진 명확히 이해가 가지 않았다.
- 하지만 Component Composition과 RenderProps를 활용해 비동기 요청에 대해 loading, success, error를 핸들링하는 부분은 상태관리 라이브러리에 미들웨어를 적용해 처리하는 과정보다 매우 편리해 보였다.

  ```javascript{15,16,17,18,19,20,21,22,23,24,25,26}
  import React from 'react'
  import { Query } from 'react-apollo'
  import { gql } from 'apollo-boost'
  const query = gql`
    query Boo($id: ID!) {
      user(id: $id) {
        id
        name
      }
    }
  `

  const App: React.FC = () => {
    return (
      <Query query={query} variables={{ id: '14' }}>
        {({ data, loading, error }: any) => {
          if (loading) {
            return <p>LOADING..</p>
          }

          if (error) {
            return <p>error</p>
          }
          return <p>{data.user.name}</p>
        }}
      </Query>
    )
  }
  ```
