## 20장 strict mode



strict mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 사능성이 높거나 자바스크립트 엔지의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명식적인 에러를 발생



### strict mode의 적용

전역의 선두 또는 함수 몸체의 선두에 'use strict'를 추가. 스크립트 전체에 적용. 



### 전역에 strict mode를 적용하는 것은 피하자

스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용.

전역에 strict mode를 선언하는 것은 바람직하지 않음.

이 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 stric mode를 적용.

```js
// 즉시 실행 함수의 선두에 strict mode 적용
(function (){
    'use strict';
}())
```



### 함수 단위로 strict mode를 적용하는 것도 피하자

모든 함수에 일일히 strict mode를 적용하는 것은 번거롭고, strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 문제가 발생 할 가능성이 있음.

```js
(function (){
    //non - strict mode
    var let = 10; // 에러 발생 x
    function foo(){
        'use strict'
        
        let = 20; // SyntaxError
    }
    foo();
}());
```



### strict mode가 발생시키는 에러

- 암묵적인 전역

  선언하지 않은 변수를 참조하면 ReferenceError

  ```js
  (function (){
      'use strict';
      x =1;
      console.log(x); // ReferenceError: x is not defined
  }())
  ```

- 변수, 함수, 매개변수의 삭제

  delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError

  ```js
  (function(){
      'use strict';
      
      var x = 1;
      delete x; // SyntaxError
      
      function foo(a){
          delete a; // SyntaxError
      }
      delete // SyntaxError
  })
  ```

- 매개변수 이름의 중복

  중복된 매개변수 이름을 사용하면 SyntaxError

  ```js
  (function(){
      'use strict';
      
      // SyntaxError
      function foo(x, y){
          return x + x;
      }
      console.log(foo(1, 2));
  }());
  ```

- with 문의 사용

  ```js
  (function (){
      'use strict';
      
      // SyntaxError
      with({x:1}){
          console.log(x);
      }
  }());
  ```



### strict mode 적용에 의한 변화

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문. 에러 발생 x

```js
(function (){
    'use strict';
    function foo(){
        console.log(this); // undefined
    }
    foo();
    
    function Foo(){
        console.log(this); // Foo
    }
    new Foo();
}());
```



arguments 객체

strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않음.

```js
(function(a){
    'use strict';
    
    // 매개변수에 전달된 인수를 재할당하여 변경
    a=2;
    
    // 변경된 인수가 arguments 객체에 반영되지 않음.
    console.log(arguments); // {0:1, length: 1}
}(1));
```

