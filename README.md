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
  - ES6 -> ES5 문법으로 바꿔줌
- `@babel/preset-env`
  - ES6 -> ES5
  - ES6 뿐만 아니라 브라우저에 따라 알아서 컴파일해줌
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
  - 개발 서버를 제공
  - Live Reload를 위해 사용

웹팩 설정 파일 만들기.  
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

웹팩 설정 확인하기
`package.json`에 웹팩 명령어 추가
```
...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack"
  },
...
```
entry로 설정한 `src/index.js`를 생성
```
console.log('test');
```

`npm start`로 웹팩을 실행시키면 `dist`폴더와 `bundle.[hash].js` 파일이 생성된 것을 확인할 수 있음.