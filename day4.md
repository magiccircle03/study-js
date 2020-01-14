## Set, Map, WeakSet, WeakMap

- `Array`와 비슷한 자료구조 : Set, WeakSet

  - Array는 중복이 되지만, Set은 중복이 되지 않음

- `Object`와 비슷한 자료구조 : Map, WeakMap

  - `Object`는 순서가 없지만, `Map`은 순서가 있음
  - `Map`은 바로 순회할 수 있지만, `Object`를 순회하려면 어떤 방법이든 키의 배열을 얻고, 그 배열을 순회해야 함
  - `Object`의 키에는 `String`과 `Symbol`(ES6 에 추가된 자료형, 변경 불가능한 프리미티브 타입의 값)을 사용할 수 있지만, `Map`은 함수, 객체, 원시 자료형 등 어떤 값도 사용할 수 있음(주의! number는 안 됨)

- 의문
  - 맵과 셋은 기본적으로 순서가 없다고 알고 있는데, javascript가 특이한 건가? https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set

## Set으로 유니크한 배열 만들기

- Set : 중복이 허용되지 않는다. => 중복 없이 유일한 값을 저장하려고 할때 사용한다.
- 이미 존재하는지 체크할 때 유용하다.

```javascript
let mySet = new Set();
console.log(toString.call(mySet)); //[object Set]

mySet.add("중복data");
mySet.add("rabbit");

console.log(mySet.has("중복data")); // true

mySet.add("중복data");
mySet.add("abcd");
mySet.add("중복data");
mySet.add(123);

mySet.forEach(function(v) {
  console.log(v); // 중복data, rabbit, abcd, 123
});

mySet.delete("중복data");

mySet.forEach(function(v) {
  console.log(v); // rabbit, abcd, 123
});
```

## WeakSet 으로 효과적으로 객체타입저장하기

- WeakSet : 참조를 가지고 있는 객체만 저장 가능
- Set의 경우 원본 참조가 제거되더라도 참조를 유지, 가비지 컬렉터의 정리 대상에 포함되지 않아서 메모리 누수가 발생할 수 있음
- WeakSet은 원본이 삭제되면 가비지 컬렉터의 삭제 대상에 포함되서 메모리를 효율적으로 관리할 수 있음
- .has(), .add(), .delete() 만 지원

```javascript
let arr = [1, 2, 3, 4];
let arr2 = [5, 6, 7, 8];
let obj = { arr, arr2 };

let ws = new WeakSet();
let s = new Set();

ws.add(arr); // 변수
ws.add(function() {}); // 함수

// 에러. Invalid value used in weak set
// ws.add(111); // 숫자
// ws.add("111"); // 문자
// ws.add(null);

// 에러.WeakSet은 forEach를 제공하지 않는다.
// ws.forEach(function(v) {
//   console.log(v);
// });

ws.add(arr);
ws.add(arr2);
ws.add(obj);

s.add(arr);
s.add(arr2);

arr = null;

console.log(ws.has(arr) + ", " + ws.has(arr2)); // false, true
console.log(s.has(arr) + ", " + s.has(arr2)); // false, true

console.log(ws); // WeakSet {{…}, Array(4), Array(4)}
console.log(s); // Set(2) {Array(4), Array(4)}
s.forEach(function(v) {
  console.log(v); //Array(4) [1, 2, 3, 4], Array(4) [5, 6, 7, 8]
});
```

## Map & WeakMap 추가정보를 담은 객체저장하기

- Map : key, value 구조이다.
- WeakMap의 키는 객체만! 키로 원시 데이터형은 허용되지 않는다.
- WeakMap은 .has(), .get(), .set(), .delete()만 지원

```javascript
// 맵 생성 1
const m1 = new Map([
  ["a", 1],
  ["b", 2]
]); // map 생성

console.log(m1); // Map(2) {"a" => 1, "b" => 2}

// 맵 생성 2
const m2 = new Map();
m2.set("a", 1);
m2.set("b", 2);

console.log(m2); // Map(2) {"a" => 1, "b" => 2}

// 순회 가능
for (let val of m2) {
  console.log(val); // Array(2) ["a", 1], Array(2) ['b', 2]
}
```

```javascript
let wm = new WeakMap();
let myfun = function() {};
// 이 함수가 얼마나 실행됐지? 를 알려고 한다.

wm.set(myfun, 0);

console.log(wm); // WeakMap {ƒ => 0}

let count = 0;
for (let i = 0; i < 10; i++) {
  count = wm.get(myfun); // get value
  count++;
  wm.set(myfun, count);
}

console.log(wm); // WeakMap {ƒ => 10}
console.log(wm.get(myfun)); // 10
myfun = null;
console.log(wm.get(myfun)); // undefined
console.log(wm.has(myfun)); // false
```

## WeakMap 클래스 인스턴스 변수 보호하기

```javascript
// 보호하지 않은 예
function Area(width, height) {
  this.height = height;
  this.width = width;
}

Area.prototype.getArea = function() {
  return this.height * this.width;
};

let myarea = new Area(10, 20);
console.log(myarea.getArea()); // 200
console.log(myarea.height); // 20 // 변수에 접근할 수 있다.
```

```javascript
// WeakMap 활용
const wm = new WeakMap();

function Area(width, height) {
  wm.set(this, { width, height });
}

Area.prototype.getArea = function() {
  const { width, height } = wm.get(this);
  return width * height;
};

let myarea = new Area(10, 20);
console.log(myarea.getArea()); // 200
console.log(myarea.height); // undefined , 외부에서 접근 불가
```

### WeakSet/WeakMap 정리

- 참조하고 있는 오브젝트가 사라지면 가비지 컬렉션의 대상이 됨 = 효율적 메모리 관리
- 가비지 컬렉션 : 사용이 완료된 메모리를 정리하는 것
- WeakSet에 저장되는 value, WeakMap에 저장되는 key는 반드시 객체여야 한다.
