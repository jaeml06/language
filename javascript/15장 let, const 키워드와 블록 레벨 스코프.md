## 15장 let, const 키워드와 블록 레벨 스코프



### var 키워드로 선언한 변수의 문제점

- 변수 중복 선언 허용

  ```js
  var x = 1;
  console.log(x);
  // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용.
  // 초기화문이 있는 변수 선언문은 자바스크립트 엔지에 의해 var 키워드가 없는 것처럼 동작.
  var x = 100;
  // 초기화문이 없는 변수 선언문이 무시.
  // var x;
  console.log(x);
  ```

- 함수 레벨 스코프

  ```js
  var x = 1;
  
  if (true){
      // x는 전역 변수. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언됨
      // 이는 의도치 않게 변수값이 변경되는 부작용을 발생.
      var x = 10;
  }
  console.log(x);
  ```

  함수 레벨 스코프는 전역 변수를 남발할 가능성을 높임.

- 변수 호이스팅

  ```js
  // 이 시점에는 변수 호이스팅에 의해 이미 a 변수가 선언됨 (1. 선언 단계)
  // 변수 a는 undefined로 초기화됨 (2. 초기화 단계)
  console.log(a) // undefined
  
  // 변수에 값을 할당 (3. 할당 단계)
  a =123;
  console.log(a) // 123
  
  //변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행
  var a;
  ```

  

### let 키워드

- 변수 중복 선언 금지

  ```js
  let a = 123;
  //let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않음
  let b = 456; // SyntaxError
  ```

- 블록 레벨 스코프

  ```js
  let a = 1; // 전역 변수
  
  {
      let b = 2; // 지역 변수
      let a = 3; // 지역 변수
  }
  console.log(a); // 1
  console.log(b) //ReferenceError
  ```

- 변수 호이스팅

  let 키워드로 선언한 변수는 선언단계와 초기화 단계가 분리되어 진행.

  일시적 사각지대 : 스코프의 시작 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간

  ```js
  // 런타임 이전에 선언 단계가 실행됨. 이전 변수가 초기화되지 않음.
  // 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없음.
  console.log(a); // ReferenceError
  
  let a; //변수 선언문에서 초기화 단계가 실행됨.
  console.log(a); // undefined
  
  a = 1;
  console.log(a) // 1
  ```

  자바스크립트는 ES6에서 도입된 let, const를 포함해서 모든 선언을 호이스팅 함.

- 전역 객체의 let

  let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님.

  ```js
  let x =1;
  console.log(window.x); // undefined
  console.log(x); // 1
  var y = 1;
  console.log(window.y) // 1
  ```

  

### const 키워드

- 선언과 초기화

  const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함.

- 재할당 금지

  const 키워드로 선언한 변수는 재할당이 금지.

- 상수

  상수는 재할당이 금지된 변수.

  일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타냄.

- const 키워드와 객체

  const 키워드로 선언된 변수에 객체를 할당할 경우 값을 변경할 수 있음.

  ```js
  const person = {
      name = 'Lee'
  };
  // 객체는 변경 가능한 값. 따라서 재할당 없이 변경 가능.
  
  person.name = 'Kim';
  console.log(person); // {name : "Kim"}
  ```

  const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지 않음.



### var vs. let vs. const

- ES6를 사용한다면 var키워드는 사용하지 않음
- 재할당이 필요한 경우에 한정해 let키워드를 사용. 이때 변수의 스코프는 최대한 좁게.
- 변경이 필요하지 않고 읽기 전용으로 사용하는 원시 값과 객체에는 const 키워드 사용. const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전.

