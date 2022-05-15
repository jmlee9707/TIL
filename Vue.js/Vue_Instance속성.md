# Vue Instance 속성

## Vue method

- Vue Instance는 생성과 관련된 data 및 method로 정의 가능
- method안에서 data를 "this.데이터이름"으로 접근 가능
- 인스턴스가 안에서 인스턴스가 가지고 있는 데이터, 인스턴스를 호출하고 싶을땐 this. 으로 접근해야함

```html
<body>
  <div id="app">
    <div>data : {{message}}</div>
    <div>method kor : {{helloKor()}}</div>
    <div>method eng : {{helloEng()}}</div>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        message: "Hello",
        name: "미피" /*methods에서 사용*/,
      },
      methods: {
        helloEng: function () {
          return (
            "Hello " + this.name
          ); /*data가 가지고 있는 name임을 알려주어야 함*/
        },
        // helloKor : () => { /*this 사용시에 arrow function 사용불가 왜???*/
        //   return '안녕 ' + this.name;
        // },
        helloKor() {
          return "안녕 " + this.name;
        },
      },
    });
  </script>
</body>
```

## Vue filter

- vue 의 필터는 화면에 표시되는 텍스트의 형식을 쉽게 변환해주는 기능
- filter를 이용하여 표현식에 새로운 결과 형식을 적용
- 중괄호 보간법[{{}}] 또는 v-bind 속성에서 사용이 가능

```html
<!-- 중괄호 보간법-->
{{message | capitalize}}

<!-- v-bind 표현-->
<div v-bind:id="rawId | formatId"></div>
```

### 전역 필터 & 지역필터

- 사용 예 :천마다 ',' 찍기, 전화번호에 '-' 넣기

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue.js</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <div>
        <!--금액에 원표시?-->
        금액 : <input type="text" v-model="msg1" /><br />
        전화번호 : <input type="text" v-model="msg2" />
      </div>
      <div>
        <h3>{{ msg1 | price | won }}</h3>
        <h3>{{ msg2 | mobile }}</h3>
      </div>
    </div>
    <script>
      Vue.filter("price", (value) => {
        if (!value) return value;
        return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ","); // 정규표현식 -> 찾아보기, 숫자의 세자리마다 ,
      });
      Vue.filter("won", (value) => {
        // chaining 필터
        // 화살표 함수 사용 가능
        return `${value}원`;
      });
      Vue.filter("mobile", (value) => {
        if (!value || !(value.length === 10 || value.length === 11))
          return value;
        return value.replace(/^(\d{3})(\d{3,4})(\d{4})/g, "$1-$2-$3");
      });
      new Vue({
        el: "#app",
        data: {
          msg1: "",
          msg2: "",
        },
      });
    </script>
  </body>
</html>
```

## Vue computed 속성

- **특정 테이터의 변경사항을 실시간으로 처리**
- 캐싱을 이용하여 **데이터의 변경이 없을 경우** 캐싱된 데이터를 반환 >> 값
- Setter와 Getter를 직접 지정할 수 있음
- 작성은 method 형태로 작성하지만 Vue에서 proxy 처리하여 property처럼 사용

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue.js</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>

  <body>
    <div id="app">
      <input type="text" v-model="message" />
      <p>원본 메시지: "{{ message }}"</p>
      <p>역순으로 표시한 메시지1: "{{ reversedMsg }}"</p>
      <!--처음 호출했으니 computed-->
      <p>역순으로 표시한 메시지2: "{{ reversedMsg }}"</p>
      <!--변수처럼 값만 리턴-->
      <p>역순으로 표시한 메시지3: "{{ reversedMsg }}"</p>
    </div>
    <script>
      var vm = new Vue({
        el: "#app",
        data: {
          message: "안녕하세요 여러분",
        },
        computed: {
          // 값이 바뀔때마다 한번만 호출, 메소드 호출이 아니라 값처럼 인식
          // 1. 항상 instance가 가진 값으로 무언가를 해야함
          // 2. 값 : 명사형 -> 함수는 return이 반드시 있어야 함
          // 3. 쓸데없는 호출이 일어나지 않음, 메소드와 다르게 처음에 한번만 실행
          // 4. 메소드 : 단순히 일처리만 할때, computed : 인스턴스가 가지고 있는 데이터를 변경해서 값을 받아야할 경우
          reversedMsg: function () {
            // 변수처럼 인식
            console.log("꺼꾸로 찍기");
            return this.message.split("").reverse().join("");
          },
        },
      });
    </script>
  </body>
</html>
```

```html
<body>
  <div id="app">
    <input type="text" v-model="message" />
    <p>원본 메시지: "{{ message }}"</p>
    <p>역순으로 표시한 메시지1: "{{ reversedMsg() }}"</p>
    <p>역순으로 표시한 메시지2: "{{ reversedMsg() }}"</p>
    <p>역순으로 표시한 메시지3: "{{ reversedMsg() }}"</p>
  </div>
  <script>
    var vm = new Vue({
      el: "#app",
      data: {
        message: "안녕하세요 여러분",
      },
      methods: {
        // 값이 바뀔 때마다 요청할 때 마다, 메소드 매번 호출 -> 동사형태
        reversedMsg: function () {
          console.log("꺼꾸로 찍기");
          return this.message.split("").reverse().join("");
        },
      },
    });
  </script>
</body>
```

computed `VS` method 의 차이점

- computed 속성
  - Vue 인스턴스가 생성될때 data의 message 값을 받아 reversedMsg를 계산하여 저장해 놓는다(캐싱-값 저장)
  - 위의 코드에서 reversedMsg()를 한번 실행하는 것과 같다
- method
  - reversedMsg()를 사용하려고 할때 마다 계산한다
  - 위의 코드에서 reversedMsg()를 세번 실행하는 것과 같다

## Vue watch 속성

> [공식문서 - 필수](https://kr.vuejs.org/v2/guide/computed.html)
> Vue는 Vue 인스턴스의 데이터 변경을 관찰하고 이에 반응하는 보다 일반적인 watch 속성을 제공

**명령적인 watch 콜백보다 computed 속성을 사용하는 것이 더 좋다.**
`watch 속성` : 감시할 데이터를 지정하고 그 데이터가 바뀌면 이런 함수를 실행하라는 방식으로 소프트웨어 공학에서 이야기하는 **‘명령형 프로그래밍’ 방식.**
`computed 속성` : 계산해야 하는 목표 데이터를 정의하는 방식으로 소프트웨어 공학에서 이야기하는 **‘선언형 프로그래밍’ 방식.** -> 명사형

```html
<div id="app">
  <div>
    <input type="text" v-model="a" />
  </div>
</div>
<script>
  var vm = new Vue({
    el: "#app",
    data: {
      a: 1,
    },
    watch: {
      a: function (val, oldVal) {
        // 새로운 값과 이전 값
        console.log("new: %s, old: %s", val, oldVal);
      },
    },
  });
  console.log(vm.a);
  vm.a = 2; // => new: 2, old: 1
  console.log(vm.a);
</script>
```
