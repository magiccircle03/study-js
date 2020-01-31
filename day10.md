## 미니프로젝트 1

### 1. nodeJS 기반 환경구성과 webpack

1. 폴더생성
2. npm init
3. npm install webpack --save-dev // package.json의 devDependencies에 들어가게 됨. (--save는 일반 디펜던시에)
4. scripts 에 webpack을 추가함

```json
{
  "name": "js-study-miniproject",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.41.5",
    "webpack-cli": "^3.3.10"
  }
}
```

5. webpack.config.js 생성

```js
var path = require("path");

module.exports = {
  entry: {
    main: ["./src/index.js"]
  },
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "bundle.js"
  },
  module: {
    rules: []
  }
};
```

6. src폴더에 index.js 생성

```js
console.log("hi");
```

7. npm run start 해보면 -> dist 폴더와 그 안에 bundle.js가 생겨있음

---

### 2. babel preset 설정

오래된 브라우저에 제공하기 위해 사용.  
babel-preset-env만을 쓴다면 번들 파일의 코드의 길이는 엄청나게 길어질 것이다.  
babel-preset-env에게 원하는 브라우저만 지원가능하도록 plugin을 선택할 수 있다.

1. npm install babel-loader --save-dev
2. npm install babel-preset-env --save-dev
3. webpack.config.js

```js
var path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve(__dirname, "./src"),
        use: {
          loader: "babel-loader",
          options: {
            presets: [
              [
                "env",
                {
                  targets: {
                    browsers: ["last 2 version", "ie 9"]
                  },
                  debug: true
                }
              ]
            ]
          }
        }
      }
    ]
  }
};
```

### 3. webpack-dev-server와 html 구성

webpack-dev-server는 말그대로 임시 개발 서버를 하나 띄워서 live reloading을 제공 (실시간으로 변경되게 해주는 기능)

1. npm install webpack-dev-server --save-dev
2. 명령어 추가

```json
{
  "name": "js-study-miniproject",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "devserver": "webpack-dev-server --inline"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-preset-env": "^1.7.0",
    "webpack": "^4.41.5",
    "webpack-cli": "^3.3.10",
    "webpack-dev-server": "^3.10.1"
  }
}
```

3. index.html 작성

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <section class="controller"><button>버튼</button></section>
    <section class="blog-list"><ul></ul></section>
    <section class="like-list">
      <h4>찜목록</h4>
      <ul></ul>
    </section>
    <script src="./dist/bundle.js"></script>
  </body>
</html>
```

4. npm run devserver

5. localhost:8080 (콘솔에서 확인) 하면 만들어진 html파일을 확인가능

6. js파일 이렇게 임포트 형식으로

```js
//index.js
import blog from "./main.js";
const myBlog = new blog();
```

```js
//main.js
class Blog {
  constructor() {
    console.log("blog start");
  }
}
export default Blog;
```

## 기타

```js
var path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "./dist"),
    filename: "bundle.js"
    publicPath: "/dist"
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve(__dirname, "./src"),
        use: {
          loader: "babel-loader",
          options: {
            presets: [
              [
                //"env",
                "@babel/preset-env",
                {
                  targets: {
                    browsers: ["last 2 version", "ie 9"]
                  },
                  debug: true
                }
              ]
            ]
          }
        }
      }
    ]
  }
};
```
