# 디렉티브(Directives)

- 디렉티브는 v-접두사가 있는 특수 속성
- 디렉티브 속성 값은 **단일 JavaScript 표현식**이 된다(v-for는 예외-여러개 속성값, 하나의 값만 적용할 수 있음)
- 디렉티브의 역할은 **표현식의 값이 변경될 때** 사이드 이펙트를 반응적으로 DOM에 적용
  ![](https://velog.velcdn.com/images/jmlee9707/post/d422b0af-deec-4c4e-b84b-eab9bcf4faa4/image.png)

## v-model

- 양방향 바인딩 처리를 위해서 사용(form의 input, textarea)
- 한쪽이 바뀌면 다른쪽도 함께 바뀜, onclick, querySelector 문 사용할 필요가 없음
- 직접 얻어올 필요가 없이 바로바로 적용이 가능

```html
<body>
  <div id="app">
    <input type="text" name="" id="" v-model="message" />
    <!--양방향 소통 가능-->
    <div>{{message}}</div>
  </div>
  <script>
    new Vue({
      el: "#app",
      data() {
        return {
          message: "안녕 VueJS!!!",
        };
      },
    });
  </script>
</body>
```

## v-bind

- Mustaches는 HTML 속성에서 사용할 수 없기 때문에 대신에 v-bind 사용
- 엘리먼트의 속성과 바인딩 처리를 위해서 사용
- v-bind는 약어로 `":"`로 사용 가능

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <title>v-bind</title>
    <style>
      #btn1 {
        width: 200px;
        background-color: blueviolet;
      }
      .btn1 {
        width: 200px;
        background-color: blueviolet;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <div id="idValue">메세지</div>
      <!--id값을 가져오는 것이 아니라 문자열로 저장-->
      <div v-bind:id="idValue">메세지</div>
      <!--vue가 가진 값들을 연결-->
      <button v-bind:[key]="btnId">버튼</button>
      <!--왼쪽 : 속성 이름으로 접근할땐 [] 필수! / 오른쪽 : 변수 그대로 접근 가능-->
      <button v-bind:id="btnId">버튼</button>

      <button :[key]="btnId">버튼</button>
      <!--생략가능-->
      <button :id="btnId">버튼</button>
      <a :href="url1">이동하기</a>
      <!--값 바인딩-->
    </div>
    <script>
      new Vue({
        el: "#app",
        data() {
          return {
            idValue: "test-id1",
            key: "id",
            btnId: "btn1",
            url1: "http://www.naver.com",
          };
        },
      });
    </script>
  </body>
</html>
```

## v-show

- 조건에 따라 엘리먼트를 화면에 렌더링
- style 의 display를 변경
- false일 경우 값은 있지만 화면에 보이지 않음 : style에 display-none

```html
<body>
  <div id="app">
    <h2>v-Show</h2>
    <button @click="isShow = !isShow">보이기</button>
    <div v-show="isShow">{{msg}}</div>
  </div>
  <script>
    new Vue({
      el: "#app",
      data() {
        return {
          msg: "Hello world",
          isShow: true,
        };
      },
    });
  </script>
</body>
```

## v-if, v-else-if, v-else

- 조건에 따라 엘리먼트를 화면에 렌더링
- 태그 안에서 사용
- v-show와 다르게 보이지 않으면(조건에 맞지 않으면) **렌더링도 되지 않음**

```html
<body>
  <div id="app">
    <label for="age">나이 입력 : </label>
    <input type="text" id="age" v-model="age" />
    <!--값 바인딩-->
    <div>
      <div>요금 :</div>
      <div v-if="age<18">5000원</div>
      <div v-else-if="age<70">9000원</div>
      <div v-else>무료</div>
    </div>
  </div>
  <script>
    new Vue({
      el: "#app",
      data() {
        return {
          age: 0, // 초기값
        };
      },
    });
  </script>
</body>
```

### v-show와 v-if의 차이점

|               | v-if            | v-show            |
| ------------- | --------------- | ----------------- |
| 렌더링        | false 일 경우 X | 항상O             |
| false 일 경우 | 엘리먼트 삭제   | display:none 적용 |
| template 지원 | O               | X                 |
| v-else 지원   | O               | X                 |

## v-for

- 배열이나 객체의 반복에 사용
- v-for = "요소변수이름 in 배열" || v-for = "(요소변수이름, 인덱스) in 배열"
- 항상 `key`와 함께 사용

```html
<body>
  <div id="app">
    <h2>단순 for문</h2>
    <div v-for="i in 5" :key="index">{{i}}번</div>
    <!--5번 돌리기-->
    <h2>객체 반복</h2>
    <div v-for="value in student" :key="item.id">{{value}}</div>
    <h2>배열돌리기</h2>
    <div v-for="(area, index) in areas" :key="index">
      {{index + 1}}. {{area}}
    </div>
    <!--표현식 가능-->
  </div>
  <script>
    new Vue({
      el: "#app",
      data() {
        return {
          student: {
            id: "jenny",
            name: "이정민",
            age: 33,
          },
          areas: ["구미", "광주", "부울경", "서울", "대전"],
          // areas : []
        };
      },

      created() {
        //ajax를 이용해서 서버에서 areas 안의 값 가져오기
      },
    });
  </script>
</body>
```

> `v-if`와 `v-for` 동시 사용 금지! => ** v-if와 v-for의 수행 순서 때문에 ** 찾아보깅(v-for가 먼저) / 속도 저하

## template

- 여러 개의 태그들을 묶어서 처리해야할 경우 template를 사용
- v-if, v-for, component등과 함께 많이 사용

## v-cloak

- Vue Instance가 준비될 떄까지 mustache 바인딩을 숨기는데 사용
- [v-cloak]{display : none}과 같은 CSS 규칙과 함께 사용
- Vue Instance가 준비되면 v-cloak은 제거됨

## v-methods

- Vue Instance는 생성과 관련된 data 및 method의 정의 가능
- method 안에서 data를 `this.data`로 접근 가능
