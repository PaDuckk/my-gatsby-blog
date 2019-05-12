---
title: JavaScript에서 비공개 프로퍼티와 메서드
date: 2019-02-03
category: 'Javascript'
---

- 자바 스크립트에서는 접근제어자인 private, protected, public을 나타내는 별도의 문법이 없다. 그러나 클로저 개념을 이용해 구현 가능하다.

### 비공개 멤버

1. 생성자 패턴
   ```javascript
   function Gadget() {
       var name = 'deok';
       this.getName = function() {
           return name;
       }
   }

   var toy = new Gadget();
   
   console.log(toy.name): // undefined

   console.log(toy.getName()); // "deok"
   ```
   
   생성자 함수인 Gadget의 name은 this의 프로퍼티가 아니기 때문에 직접 접근하는 것은 불가하나 공개 메서드인 getName은 'name'에 접근 할 수 있다. 이러한 메서드를 **특권 메서드**라 칭한다.

2. 객체 리털럴 방식
    ```javascript
    var myobj = (function() {
        // 비공개 멤버
        var name = "hello";

        // 공개될 부분
        return {
            getName: function() {
                return name;
            }
        }
    });
    ```
    즉시 실행 함수의 스코프에 있는 name은 즉시 실행함수를 통해 반환된 객체를 통해서만 접근이 가능하다.

### 비공개 멤버의 허점

특권 메서드에서 비공개 변수의 값을 반환할 경우 이 변수가 객체나 배열일 경우 값이 아닌 참조가 반환되기 때문에 주의 하여야 한다.
참조에 의해 반환된 객체의 프로퍼티가 바뀔 경우 비공개 변수 또한 변경된다.

### 비공개 함수를 공개 메서드로 노출 시키는 방법

노출 패턴 (revelation pattern)은 비공개 메서드를 구현하면서 동시에 공개 메서드로도 노출하는 것을 의미한다. 객체의 모든 기능이 객체가 수행하는 작업에 필수불가결한 것들이라 보호가 필요한데, 동시에 이 기능들의 유용성 때문에 공개적인 접근도 허용하고 싶은 경우가 있다.

 ```javascript
    var myarray;
    (function() {
        var astr = "[object Array]",
            toString = Object.prototype.toString;

        function isArray(a) {
            return toString.call(a) === astr;
        }

        function indexOf(haystack, needle) {
            // ...
        }

        myarray = {
            isArray: isArray,
            indexOf: indexOf,
            inArray: indexOf
        };
    })();
```

myarray에는 isArray, indexOf, inArray라는 3가지 공개 메서드가 생겼다. 만약 공개된 메서드인 indexOf가 잘못된 사용에 의해 변경되더라도 inArray의 indexOf는 비공개 멤버인 indexOf를 나타내므로 잘 동작할 수 있게 되었다.

    