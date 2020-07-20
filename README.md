# webpack-react
Node.js, npm

### 1. 의존성 초기화
작업 폴더에 `package.json` 파일을 만들어서 초기화.
```
npm init -y
```

### 2. 바벨 설정
바벨을 위한 패키지들을 install.  
개발단계에서만 사용하기 때문에 `-D` 옵션으로 `devDependencies`에 추가.
```
npm i -D @babel/core @babel/preset-env @babel/preset-react
```

작업폴더에 `.babelrc` 파일을 만들고 아래 코드를 추가.
```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

- `@babel/core`
  - babel이 실제 동작하는 코드
  - ES6 -> ES5
- `@babel/preset-env`
  - babel이 동작할 때 지원범위가 어느정도까지여야 하는지에 대해 지정하도록 해주는 패키지
  - ES6 뿐만 아니라 브라우저에 따라 알아서 컴파일해줌
  - ES6 -> ES5
  - [[번역] babel-preset-env는 무엇이고 왜 필요한가?](https://velog.io/@pop8682/%EB%B2%88%EC%97%AD-%EC%99%9C-babel-preset%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%98%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80-yhk03drm7q)
- `@babel/preset-react`
  - JSX -> JavaScript

### 3. 웹팩 설정
웹팩을 사용하기 위한 패키지들을 `-D` 옵션으로 install.
```
npm i -D webpack webpack-cli webpack-dev-server
```
- `webpack`
  - 웹팩의 모듈 번들러
  - 모든 리액트 파일을 컴파일된 하나의 자바스크립트 파일에 넣기 위해 사용
- `webpack-cli`
  - 웹팩의 cli (Command Line Interface)
  - build 스크립트를 통해 webpack 커맨드를 쓰기 위해 사용
- `webpack-dev-server`
  - 웹팩에서 제공하는 개발 서버
  - Live Reload를 위해 사용

#### 웹팩 설정 파일 만들기
작업 폴더에서 `webpack.config.js`파일을 만들고 아래 코드를 작성.
`module.exports = {}` 부분에서 웹팩을 설정

```
module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'bundle.[hash].js'
  },
};
```
- `mode`
  - 현재 모드를 개발환경(`development`)로 설정
- `entry`
  - 애플리케이션 진입점
  - 컴파일 할 파일의 경로
- `output`
  - 컴파일된 결과물
  - 번들된 파일을 저장할 경로
  - `[hash]`는 애플리케이션이 컴파일 될 때 웹팩에서 생성된 해시를 사용

#### 웹팩 설정 확인하기
`package.json`에 웹팩 명령어 추가
```
// ...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack"
  },
// ...
```
entry로 설정한 `src/index.js`를 생성
```
console.log('test');
```

`npm start`로 웹팩을 실행시키면 `dist`폴더와 `bundle.[hash].js` 파일이 생성된 것을 확인할 수 있음.

#### `loader` 사용하기
`loader`를 사용하기 위한 패키지 install.
```
npm i -D babel-loader html-loader
```
- `babel-loader`
  - 바벨을 웹팩에서 사용할 수 있게 해줌
  - `.js` 파일을 babel preset/plugin과 webpack을 사용하여 ES5로 컴파일 해주는 plugin
  - JSX -> JavaScript 로 컴파일
  - html webpack plugin
- `html-loader`
  - 웹팩이 html을 읽을 수 있게 해줌

`webpack.config.js`에 아래 내용 추가
```
// ...
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: {
              minimize: true,
            },
          },
        ],
      },
    ],
  },
// ...
```
- `test`
  - 빌드할 파일
  - 정규표현식 사용
  - `.js`와 `.jsx` 확장자를 번들
- `exclude`
  - 제외할 파일
  - 정규표현식 사용
  - `node_modules` 안에 있는 파일은 번들에서 제외
- `loader`
  - 사용할 로더 이름
- `option`
  - 로더 옵션
  - `minimize: true` 코드 최적화 옵션

#### `plugin` 설정
```
npm i -D html-webpack-plugin
```
- `html-webpack-plugin`
  - 템플릿을 지정하거나 favicon을 설정할 수 있음

`webpack.config.js`에 아래 내용 추가
```
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {

// ...

  plugins: [
    new HtmlWebpackPlugin({
      template: 'public/index.html',
    })
  ],
```
- `template`
  - `public/index.html`를 템플릿으로 지정

#### 개발서버 설정
```
npm i -D webpack-dev-server
```
- `webpack-dev-server`
  - 웹팩에서 제공하는 개발 서버
  - Live Reload를 위해 사용

`package.json`의 스크립트 변경
```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server"
  },
```
`webpack.config.js`에서 개발 서버를 설정

```
const port = process.env.PORT || 3000;

module.exports = {

// ...

  devServer: {
    host: 'localhost',
    port: port,
    open: true,
  },
```
- `host`
  - 개발서버의 url
- `port`
  - 기본값으로 3000번 포트 사용
- open 
  - 서버가 실행될 때 브라우저 자동 열기 여부

[devServer의 다른 옵션](https://webpack.js.org/configuration/dev-server/)

#### React 
```
npm i react react-dom
```

`src/index.js` 파일 수정
```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));
```

`src/App.js` 파일 생성
```
import React from 'react';

const App = () => (
  <div>
    Hello, Webpack!
  </div>
);
export default App;
```

`public/index.html` 템플릿 생성
```
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Webpack-for-react</title>
</head>

<body>
  <div id="root"></div>
</body>

</html>
```

`npm start`로 애플리케이션을 실행하면,  
**바벨**을 통해 ES6 -> ES5로 변환되고  
**웹팩**을 통해 하나의 파일(`bundle.[hash].js`)로 번들되어 브라우저에 띄워진다.

개발자 도구 (developer tool) > Sources 탭에서 번들된 파일을 확인할 수 있다.

\~ 끝 \~