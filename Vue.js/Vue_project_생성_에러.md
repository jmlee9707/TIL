# Vue project 생성

vue create cli-first-app

cd cli-first-app

npm run serve

## 초기세팅

npm i axios
npm i moment
npm install --save vuex-persistedstate

## 부트스트랩 추가

npm install vue bootstrap-vue bootstrap

main.js에 추가

```
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// Import Bootstrap and BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

// Make BootstrapVue available throughout your project
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)
```

# Vue error

## eslint

setting.json 파일에

```
"eslint.workingDirectories": [
    {
      "mode": "auto"
    }
```

추가하기

## template error : volar

jsoncofig.json -> compilerOptions

```
"jsx":"preserve"
```

추가 : 보통 프로젝트 폴더를 생성하지 않고 폴더를 만들어서 들고오는 경우 발생

## prettier error

```
npm run lint // 프롬프트 창에서 실행해보기
```
