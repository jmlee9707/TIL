# 바인딩 binding

## class binding

- element의 class와 style을 변경
- **v-bind:class는 조건에 따라 class를 적용할 수 있다**
- 사용법 -> v-bind:class ="{class이름 : t/f}"
- 클래스 여러개 적용 가능

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue.js</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      .active {
        background: rgb(106, 148, 238);
        color: white;
      }

      div {
        width: 200px;
        height: 200px;
        border: 1px solid #444;
      }
    </style>
  </head>

  <body>
    <div id="app">
      <div v-bind:class="{ active: isActive }">VueCSS적용</div>
      <button v-on:click="toggle">VueCSS</button>
    </div>
    <script type="text/javascript">
      new Vue({
        el: "#app",
        data: {
          isActive: false,
        },
        methods: {
          toggle: function () {
            this.isActive = !this.isActive;
          },
        },
      });
    </script>
  </body>
</html>
```

클래스 뿐만 아니라 스타일도 바인딩 가능

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue.js</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style type="text/css">
      .completed {
        text-decoration: line-through;
        font-style: italic;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <ul>
        <li
          :class="{completed: todo.done}"
          :style="myStyle"
          v-for="(todo, i) in todos"
          :key="i"
        >
          {{todo.msg}}
          <button @click="complete(todo)" class="btn">완료</button>
        </li>
      </ul>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          todos: [
            {
              msg: "5시간 잠자기",
              done: false,
            },
            {
              msg: "알고리즘 1시간 공부하기",
              done: false,
            },
            {
              msg: "Vue 1시간 공부하기",
              done: false,
            },
          ],
          myStyle: {
            fontSize: "20px",
          },
        },
        methods: {
          complete: function (todo) {
            // 메세지에 완료 붙이기
            if (!todo.done) {
              todo.msg = todo.msg + "완료";
              todo.done = !todo.done; // true, false 값
              event.target.innerText = "취소";
            } else {
              todo.msg = todo.msg.substr(0, todo.msg.indexOf("완료"));
            }
          },
        },
      });
    </script>
  </body>
</html>
```

## 폼 입력 바인딩(Form input binding)

- v-model 디렉티브를 사용하여 폼 input과 textarea 엘리먼트에 양방향 데이터 바인딩을 생성할 수 있다.
  - text와 textarea 태그는 value 속성과 input 이벤트를 사용한다
  - 체크박스들과 라디오버튼들은 checked 속성과 change이벤트를 사용한다
  - select 태그는 value를 prop으로, change를 이벤트로 사용한다

[공식문서](https://kr.vuejs.org/v2/guide/forms.html)

### checkbox의 v-model

```html
<body>
  <div id="app">
    <div>
      <p>
        이메일 수신
        <input type="checkbox" id="emailYN" v-model="email" />
        <label for="emailYN">{{ email }}</label>
      </p>
    </div>
    <div>
      <p>
        SMS 수신
        <input
          type="checkbox"
          id="smsYN"
          v-model="sms"
          true-value="Y"
          false-value="N"
        />
        <label for="smsYN">{{ sms }}</label>
      </p>
    </div>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        email: false,
        sms: "Y",
      },
    });
  </script>
</body>
```

```html
<body>
  <div id="app">
    <div>당신이 가고 싶은 지역을 선택하시오</div>
    <input type="checkbox" id="buk" value="부울경" v-model="checkedAreas" />
    <label for="buk">부울경</label>
    <input type="checkbox" id="gwangju" value="광주" v-model="checkedAreas" />
    <label for="gwangju">광주</label>
    <input type="checkbox" id="gumi" value="구미" v-model="checkedAreas" />
    <label for="gumi">구미</label>
    <input type="checkbox" id="daejeon" value="대전" v-model="checkedAreas" />
    <label for="daejeon">대전</label>
    <input type="checkbox" id="seoul" value="서울" v-model="checkedAreas" />
    <label for="seoul">서울</label>
    <br />
    <span>체크한 이름: {{ checkedAreas }}</span>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        checkedAreas: [], // 배열로 여러가지 저장
      },
    });
  </script>
</body>
```

### radio button의 v-model

값을 하나만 받을 수 있음 => 변수로 지정

```html
<body>
  <div id="app">
    <div>수업을 듣는 지역을 선택하시오</div>
    <div>
      <input type="radio" id="buk" value="부울경" v-model="ckArea" />
      <label for="buk">부울경</label>
      <input type="radio" id="gwangju" value="광주" v-model="ckArea" />
      <label for="gwangju">광주</label>
      <input type="radio" id="gumi" value="구미" v-model="ckArea" />
      <label for="gumi">구미</label>
      <input type="radio" id="daejeon" value="대전" v-model="ckArea" />
      <label for="daejeon">대전</label>
      <input type="radio" id="seoul" value="서울" v-model="ckArea" />
      <label for="seoul">서울</label>
    </div>
    <span>선택한 지역 : {{ ckArea }}</span>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        ckArea: "광주",
      },
    });
  </script>
</body>
```

### 수식어

- `.lazy` : **값 변경되는 시점을 파악하여 이벤트를 발생**
  - 기본적으로, v-model은 각 입력 이벤트 후 입력과 데이터를 동기화(단 IME 구성은 제외) **`.lazy` 수식어를 추가하여 change 이벤트 이후에 동기화 할 수 있다**
- `.trim` : v-model이 관리하는 input을 자동으로 trim 하기 원하면, trim 수정자를 추가 -> ** 앞뒤의 공백 제거**
- `.number` : **사용자 입력이 자동으로 숫자로 형변환**, 기본값은 text

```html
<body>
  <div id="app">
    <div>
      아이디 :
      <input v-model.trim="id" placeholder="아이디를 입력하세요" />
      <!-- v-model은 기본적으로 모든 key stroke가 발생할 때마다 값을 업데이트 시킨다.
           .lazy 수식어를 추가하여 change 이벤트 이후에 동기화 할 수 있습니다. -->
      <input v-model.lazy="id" placeholder="아이디를 입력하세요" />
    </div>
    <div>
      메세지 :
      <textarea v-model="message" placeholder="내용을 입력하세요"></textarea>
    </div>
    <p>{{ id }} 님에게 보내는 메세지 : {{ message }}</p>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        id: "",
        message: "",
      },
    });
  </script>
</body>
```
