# Bitbank realtime data
根据Bitbank API数据显示深度图和步长值

## web site url
与瑞波币/日元兼容。
https://bb-realtimedata.firebaseapp.com/

## Require
- Node.js version 8.9 or above (8.11.0+ recommended)
- Yarn
- @vue/cli (`yarn global add @vue/cli`)


## Project setup
可能缺少所需的依赖项。
如果您有任何问题，请联系我们。

copy `.env.example` to `.env` then set gaid if you need google analytics.
```
yarn install
```

### Compiles and hot-reloads for development
由于开发机器环境的原因，设置了4000端口。
```
yarn run serve
```

### Compiles and minifies for production
部署到您最喜欢的静态托管服务，例如 Firebase Hosting。
构建后，定位到 dist 目录。
```
yarn run build
```
