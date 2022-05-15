# 보간법(interpolation)

## 문자열

`{{속성명}}`

- 데이터 바인딩의 가장 기본 형태는 **"Mustache" 구문(이중 중괄호)**을 사용한 텍스트 보간

  - 데이터를 화면을 뿌릴 때 중괄호 두개를 이용하여 사용
  - 결과적으로 표현할 수 있는 식만 사용가능

- v-once 디렉티브(지시문)를 사용하여 데이터 변경 시 업데이트 되지 않는 일회성 보간을 수행
  - 데이터를 한번 뿌렸으면 v-once를 사용한 변수는 더이상 값을 바꿀 수 없음 -> **표현 자체가 한번!**

```html
<div id="app">
  <h2>{{message}}</h2>
  <h2 v-once>{{message}}</h2>
</div>
```

## 원시 HTML

- 이중 중괄호(mustaches)는 HTML이 아닌 일반 텍스트로 데이터를 해석
- 실제 HTML을 출력하려면 v-html 디렉티브를 사용

```html
<body>
  <div id="app">
    <div>{{message}}</div>
    <!--문자열로 인식-->
    <div v-html="message"></div>
    <!--html로 인식-->
  </div>
  <script>
    new Vue({
      el: "#app",
      data() {
        return {
          message: "<span style='color :red;'>안녕하세요</span>",
          //div 태그 안에 html
        };
      },
    });
  </script>
</body>
```

## JavaScript 표현식 사용

- {{}} 안에는 표현할 수 있는 식 사용 가능 -> if문은 불가능!
- Vue.js는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <h2>{{message}}</h2>
      <h2>{{number + 1}}</h2>
      <h2>{{number > 10 ? "Yes" : "No"}}</h2>
      <h2>{{msg.split('')}}</h2>
      <h2>{{msg.split('').reverse()}}</h2>
      <h2>{{msg.split('').reverse().join('')}}</h2>
    </div>
    <script>
      new Vue({
        // Vue 객체 자동으로 만들기 - newVue
        el: "#app",
        data() {
          return {
            message: "안녕 VueJS!!!", //함수처럼 사용
            number: 5,
            msg: "Hello world!!",
          };
        },
      });
    </script>
  </body>
</html>
```

결과
![](https://velog.velcdn.com/images/jmlee9707/post/386ed219-1ce9-4ffd-9a25-2d0ec096ce3b/image.png)
