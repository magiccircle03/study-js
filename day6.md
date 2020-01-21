# Function

## arrow function

화살표 함수를 사용하여 함수를 축약하여 표현할 수 있습니다.

- 괄호 안에 파라미터가 들어갑니다.
- 또한 블록 안에 리턴문밖에 없는 경우, 중괄호를 생략할 수 있습니다.

```javascript
// 원래 우리가 사용하던 함수형태
setTimeout(function() {
  console.log("settimeout");
}, 1000);

// arrow function 을 이용한 축약 표현법
setTimeout(() => {
  console.log("settimeout arrow");
}, 1000);
```

```javascript
// 원래 우리가 사용하던 함수형태
let newArr = [1, 2, 3, 4, 5].map(function(v) {
  return v * 2;
});
// arrow function 을 이용한 축약 표현법
let newArr2 = [1, 2, 3, 4, 5].map(v => v * 2);
```

## 화살표 함수와 this context

- 자바스크립트에서 this는 보통 실행타임에 의해서 변경이 있기 때문에 bind를 이용하여 context의 this를 잡아줘야 함
- Arrow function 안에서의 this context는 bind없이 우리가 원하는 this context를 잡아줌.
- 즉 선언된 것을 기준으로 this가 결정됨.

```javascript
// 원래 this
const myObj = {
  runTimeout() {
    setTimeout(
      function() {
        //바인드 없이 this를 찍었을 때 Timeout {_called: true, _idleTimeout: 200, _idlePrev: null, _idleNext: null, _idleStart: 296, …}
        this.printData();
      }.bind(this), // bind
      200
    );
  },
  printData() {
    console.log("hi myObj!");
  }
};

myObj.runTimeout();

// arrow function에서 this
const myObj2 = {
  runTimeout() {
    setTimeout(() => {
      //this를 찍으면 Object {runTimeout: , printData: }
      this.printData();
    }, 200);
  },
  printData() {
    console.log("hi myObj2!");
  }
};

myObj2.runTimeout();
```

```javascript
// bind 없이 this를 활용한 다른 예
const el = document.querySelector("p");

const myObj = {
  register() {
    el.addEventListener("click", evt => {
      this.printData(evt.target);
    });
  },
  printData(el) {
    console.log("clicked!!", el.innerText);
  }
};

myObj.register();
```

## default parameters

- 파라미터에 값을 주지 않으면 undefined -> 보통 함수 내부에서 기본값을 처리했음

```javascript
// 이런 함수가 있었다면
function sum(value, size) {
  return value * size;
}

console.log(sum(3)); // NaN

// 기존 처리방식
function sum(value, size) {
  size = size || 1;
  return value * size;
}

// ES6. 기본값을 줄 수 있다.
function sum(value, size = 1) {
  return value * size;
}

// ES6. 오브젝트도 이렇게 주면 된다.
function sum(value, size = { value: 1 }) {
  return value * size.value;
}
```

## rest parameters

여러개의 인자를 받아 처리하고 싶을 떄, 이전에 보통 arguments를 이용했지만 array가 아니라서 array의 편리한 메서드를 사용하려면 변환 과정이 필요했다.

```javascript
// 이전
function checkNum() {
  console.log(toString.call(arguments)); // [object Arguments]
  const argArray = Array.prototype.slice.call(arguments);
  console.log(toString.call(argArray)); // [object Array]

  const result = argArray.every(v => typeof v === "number"); // 배열 안의 값이 모두 조건식을 만족해야 true인 메서드
  console.log(result); // false
}

// const result = checkNum(10,2,"55");
const result = checkNum(10, 2, 3, 4, 5, "55");
```

```javascript
// ES6. rest parameters
// 변환없이 바로 배열로 받을 수 있다.
function checkNum(...argArray) {
  console.log(toString.call(argArray)); // [object Array]
  const result = argArray.every(v => typeof v === "number");
  console.log(result); // false
}

// const result = checkNum(10,2,"55");
const result = checkNum(10, 2, 3, 4, 5, "55");
```
