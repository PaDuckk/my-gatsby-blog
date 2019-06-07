---
title: webpack tutorial
date: 2019-06-07 19:06:87
category: development
---

## Webpack

- webpack은 모듈 번들러
  - 특징 여러번의 http request요청을보내 리소스를 보내야한다는 단점을 하나의 파일로 합쳐주어 해결해준다.
  - 모듈시스템 구현해준다.
- webpack.config.js의 핵심적인 부분은 entry, output, module, plugin 이렇게 네 개!

- resoleve는 웹팩이 알아서 경로나 확장자를 처리할 수 있게 도와주는 옵션입니다.
- modules에 node_modules를 넣으셔야 디렉토리의 node_modules를 인식할 수 있습니다.
- extensions에 넣은 확장자들은 웹팩에서 알아서 처리해주기 때문에 파일에 저 확장자들을 입력할 필요가없어집니다.
- mode 웹팩4에서 추가됨 mode가 development면 개발용 prodcution이면 배포용 배포용이면 알아서 최적화 해줌 따러서 기존 최적화 플러그인들이 대량으로 호환되지 않습니다? 웹팩3이라면 config파일에서 mode랑 optimization을 빼주세요!
- entry 부분이 웹팩이 파일을 읽어들이기 시작하는 부분입니다. app이 객체의 키로 설정되어 있는데 이부분 이름은 자유롭게 바꾸시면 됩니다. 저 키가 app이면 app.js로 나오고 zero면 zero.js로 나옵니다.

```javascript
        // 번들 파일이 2개 생성된다.
        // 보통 멀티페이지에서 entry를 여러개 사용한다.
        {
        	entry: {
        		app: '파일 경로',
        		zero: '파일 경로',
        	}
        }

        // 하나의 entry에 여러파일들을 넣고 싶을때는 배열사용

        {
          entry: {
            app: ['a.js', 'b.js'],
          },
        }
```

또한 JS파일 대신 npm모듈들을 넣어도 됩니다. 보통 @babel/polyfil이나 eventsource-polyfill 같은 것들을 적용할 때 다음과 같이 합니다.

```javascript
        // 리액트에서 자주 사용하는 entry
        {
          entry: {
            vendor: ['@babel/polyfill', 'eventsource-polyfill', 'react', 'react-dom'],
            app: ['@babel/polyfill', 'eventsource-polyfill', './client.js'],
          },
        }
```

- output 은 결과물이 어떻게 나올지 설정을 해줍니다.

```javascript
        {
          output: {
            path: '/dist',
            filename: '[name].js',
            publicPath: '/',
          },
        }
```

path와 publicPath의 차이는 path는 output으로 나올 파일이 저장될 경로입니다. publicPath는 파일들이 위치할 서버상의 경로입니다.

filename을 보면 [name].js 이렇게 되있는데 이렇게 해야 entry에서 지정한 이름으로 생성된다.

다른 옵션으로는 [hash]나 [chunkhash]가 있습니다. [hash]는 매 번 웹팩 컴파일시 랜덤한 문자열을 붙여 줍니다. 따라서 캐시 삭제시 유용합니다. [chunkhash]는 파일이 달라질 때에만 랜덤 값이 바뀝니다. 이것을 사용하면 변경되지 않은 파일들은 계속 캐싱하고 변경된 파일만 새로 불러올 수 있습니다.
