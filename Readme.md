# babel과 webpack 설정하기
## 프론트엔드의 최종 결과물은 결국 HTML, CSS, JS 이 3가지뿐!
우리는 css, js같은 파일들은 프레임워크에서 제시해주는 색다른 파일구조로 만들것이고,
결국 이들을 Webpack이 최종 결과물 형식인 css, js로 바꿔주는 역할을 해줌.

## 그럼 HTML도 만들어주냐? 그건아니다! 이건 우리가 원 파일 그대로 만들어줘야함

## SPA에서 index.html은 아주 중요하다!
검색엔진에서 참고하는부분은 결국 index.html이다.
즉 SEO를 위해서도, 정말 중요한 파일
또한 위에서부터 순서대로 읽어나가기때문에 script를 불러오기 전 코드들도 신경을 많이 써야함.
결국 성능까지 책임지는것이 이 index.html이다.

### 전역적으로 중요한 css는 index.html에 명시하고, 덜중요한 css들은 웹팩으로인한 js로 처리하도록하자
이는 구글의 추천

### SPA는 JS가 엄청 용량이 크니 이를 해결하는 방안으로 SSR, 코드스플리트 등이 있다.

### script불러오기랑 div id와 큰 연관이있다.
```
<body>
    <div id="app"></div>
    <script src="/dist/app.js"></script>
</body>
```
jsx로 만든 태그들이 app으로 설정되어있으니 저 dist/app.js의 내용은 id가 app인 div를 채워넣는다고 생각하면된다.

---

# ts + webpack 실행하기

## `npx webpack`으로 js파일을 빌드할때 NODE_ENV=production을 하면 js용량이 줄어들어 좋다!
그래서 보통 `NODE_ENV=production npx webpack`명령어를 많이 쓰는데,
**윈도우에선 저 명령어가 안된다**
`cross-env NODE_ENV=production npx webpack`을 치면 되는데,

이걸위해선 `cross-env`, `ts-node`를 설치해야한다.
resolve에러는 --force를 붙여서 설치하면된다.

현재 이 레포에선 `npm run build`로 정적으로 명시해놓았다.

빌드한결과
![](img\간단한웹팩빌드후실행사진.png)

# 웹팩 데브 서버 설정하기

## tsx하나 바꿀때마다 빌드 다시해야하나? 아니다! 핫리로딩!
단, 핫리로딩 서버가 필요하다 이게 바로 웹팩데브서버
`npm i webpack-dev-server -D` -D는 개발전용이란뜻  
`npm i -D @types/webpack-dev-server`  
`npm i @pmmmwh/react-refresh-webpack-plugin`  
`npm i react-refresh`  
이런거 귀찮으니 그냥 다음엔 **CRA(Create React App)**사용하자.

webpack.config.ts에도  
```javascript
if (isDevelopment && config.plugins) {
  config.plugins.push(new webpack.HotModuleReplacementPlugin());
  config.plugins.push(new ReactRefreshWebpackPlugin());
}
```  

```javascript
env: {
  development: {
   plugins: [require.resolve('react-refresh/babel')],
  },
},
```

```javascript
  devServer: {
    historyApiFallback: true, // react router
    port: 3090,
    devMiddleware: { publicPath: '/dist/' },
    static: { directory: path.resolve(__dirname) },
  },
```
를 넣어야한다.

이제 dev서버를 여는 스크립트는  
`webpack serve --env development`이다.

![](/img/웹펙데브서버_핫리로딩으로index파일연사진.png)