---
title: JavaScript Patterns 5.1 namespace 패턴
date: 2019-02-03
category: 'reading'
---

- 자바 스크립트에서 아래와 같이 선언시 전역 객체에 프로퍼티로 추가 되어 진다.

```javascript
    var a = "hi";
    
    // 만약 실행 환경이 브라우저라면 (브라우저의 전역 객체는 window이다)
    console.log(window.a); // "hi" 가 출력
```

이러하게 전역 변수를 무분별하게 생성할 경우 코드 내의 이름 충돌이나 같은 페이지에 존재하는 자바스크립트 라이브러리나 위짓 등 서드트파티코드와의 이름 충돌이 생길 위험이 있다.

```javascript
    // 안티 패턴
    
    function Parent() {}
    function Child() {}
    
    var some = "hi";
    
    var module1 = {};
    module1.data = {a: 1, b: 2};
```

위의 코드를 리팩토링하기 위해서 어플리케이션 전용 전역객체 예를 들어 MYAPP을 생성한다. 그 다음 모든 함수와 객체들을 이 객체의 프로퍼티로 변경한다.

이 때 전역 네임스페이스 객체의 이름은 어플리케이션이름이나 라이브러리의 이름, 도메인명, 회사이름을 선택할 수 있다. 그리고 **전역 객체이름은 모두 대문자**로 쓰는 명명 규칙을 사용하기도한다. (이 규칙은 상수를 쓸때도 동일)

```javascript
    // 수정 후 전역변수 개수 4개에서 1개로 준다.
    var MYAPP = {};
    
    MYAPP.Parent = function() {};
    MYAPP.child = function() {};
    
    MYAPP.some = "hi";
    
    // 객체 컨테이너
    MYAPP.modules = {};
    
    MYAPP.modules.module1 = {};
    MYAPP.modules.module1.data = {a: 1, b: 2};
```

다만 단점은 

- 모든 변수와 함수에 접두어를 붙여야 하기 때문에 전체적으로 코드량이 약간 늘어난다.
- 전역 인스턴스가 단 하나 뿐이기 때문에 코드의 어느 한 부분이 수정되어도 전역 인스턴스를 수정하게 된다.
- 이름이 중첩되고 길어지므로 프로퍼티를 판별하기 위한 검색작업도 길고 느려진다

## 범용 네임스페이스 함수

- 프로그램의 복잡도가 증가하게 되면 내가 생성하게 되는 프로퍼티가 처음으로 정의한다고 가정하기 어렵다. 이미 존재할 경우 덮어쓰게 될 지도 모른다. 따라서 먼저 존재하는지 여부를 확인하는것이 최선이다.

```javascript
    // 안티 패턴
    var MYAPP = {};
    
    // 개선안 1
    if (typeof MYAPP === "undefined") {
    	var MYAPP = {};
    }
    
    //개선안 2 (MYAPP이 undefined 이면 {}를 할당함)
    var MYAPP = MYAPP || {}
```

만약 MYAPP.modules.module2를 정의해야 하는 경우 각 단계의 객체와 프로퍼티를 정의 할때마다 확인 작업을 거쳐야 한다. 이러한 확인 작업 때문에 상당량의 중복코드가 생겨 이를 위한 함수를 만들어 쓴다고 한다. 이 **함수를 namespace()** 라 하고 다음과 같이 사용한다.

```javascript
    var MYAPP = MYAPP || {};
    
    MYAPP.namespace = function(ns_string) {
    	var parts = ns_string.split('.'),
    	parent = MYAPP,
    	i;
    	
    	//전역 객체명을 제거 해준다.
    	if (parts[0] === "MYAPP") {
    		parts = parts.slice(1);	
    	}
    
    	for (i = 0; i < parts.length; i++) {
    		// 프로퍼티가 존재하지 않으면 생성한다.
    		if (typeof parent[parts[i]] === "undefined") {
    			parent[parts[i]] = {};
    		}
    
    		parent = parent[parts[i]];
    	}
    	return parent;
    }
```

이 namespace()라는 함수는 이제 MYAPP내에 원하는 프로퍼티를 추가할 때 **기존 프로퍼티에 덮어 씌우는 것을 방지하고 생성** 할 수 있게 해준다.

```javascript
    MYAPP.namespace('MYAPP.modules.module2');
```

- 실행결과

<figure>
	<img src="/paduckk-blog/images/javascript-patterns-chapter-5.png" alt="">
</figure>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MTM4MjAwMDVdfQ==
-->