# Vue project 생성

vue create cli-first-app

cd cli-first-app

npm run serve

npm i axios
npm i moment

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
