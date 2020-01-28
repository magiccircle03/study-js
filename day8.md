# module

- javascript에서의 module import는 아직 표준화되지 않은 실험적인 기능
- 반면에 javascript를 이용한 백엔드 기반 개발에서는 많은 파일들이 필요하고, 브라우저와 달리 script src를 써서 스크립트를 불러올 수 없기 때문에 module를 불러오고 수많은 클래스들의 의존성을 정리할 필요 있음.
- 그래서 amd, common 등등의 파일들을 require들을 이용하여 import 하는 기능을 지원하기도
- ES6에서는 import, export 에 대한 스펙을 제시는 했지만 아직 많은 브라우저들이 지원하지 않고 있고, 안정화 될때까지 지원하도록 기다리고 있는 상황. ie 엣지에서는 지원을 하긴하지만 전체적으로 지원이 완전하지 않음
- 이를 지원하려면 webpack 기반으로 빌드환경을 구성하고, 바벨을 이용하여 ES5로 트랜스파일링(transpiling) 하는 작업이 필요
- 그럼에도 불구하고 import, export를 이용하여 개발이 가능합니다.

### import, export 예시 1

```javascript
import { log } from "./myLogger"; // 웹팩 설정을 해놓으면 .js 없이도 사용가능
const root = document.querySelector("#root");
root.innerHTML = "<p>Hello World ! </p>";
("my first test data");
```

```javascript
//myLogger.js
export function log(data) {
  console.log(data);
}
```

### import, export 예시 2 : default

```javascript
import log from "./myLogger";
const root = document.querySelector("#root");
root.innerHTML = "<p>Hello World ! </p>";
("my first test data");
```

```javascript
//myLogger.js
export default function log(data) {
  console.log(data);
}
```

### import, export 예시 3 : 여러 개

```javascript
import log, { getTime, getCurrentHour } from "./myLogger";

log("my first test data");
log("getTime is ${getTime()}");
log("current hour is ${getCurrentHour()}");
```

```javascript
//myLogger.js
export default function log(data) {
  console.log(data);
}
export const getTime = () => {
  return Date.now();
};
export const getCurrentHour = () => {
  return new Date().getHours();
};
```

### import, export 예시 4 : 클래스

```javascript
import log, { MyLogger } from "./myLogger";
const logger = new MyLogger();
log(logger.getLectures()); // java, IOS
```

```javascript
//myLogger.js

export default function log(data) {
  console.log(data);
}

export class MyLogger {
  constructor(props) {
    this.lectures = a["java", "IOS"];
  }
  getLectures() {
    return this.lectures;
  }
  getTime() {
    return Date.now();
  }
}
```
