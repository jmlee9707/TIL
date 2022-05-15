# Vue event

[공식문서 최고..](https://kr.vuejs.org/v2/guide/events.html)

## 이벤트 청취 => v-on

- DOM Event를 청취(listener)하기 위해 `v-on` directive 사용
- v-on directive를 사용하여 DOM 이벤트를 듣고 트리거 될 떄 JavaScript를 실행할 수 있다
- inline event handling -> jQuery는 스크립트에서 로직처리
- method를 이용한 event handling

```html
<body>
  <div id="app">
    <button v-on:click="counter += 1">클릭</button>
    <p>위 버튼을 클릭한 횟수는 {{counter}} 번 입니다.</p>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        counter: 0,
      },
    });
  </script>
</body>
```

## method event handler

> 이벤트 발생시 처리 로직을 v-on에 넣기 힘들다. 이 때문에 v-on에서는 이벤트 발생시 처리해야하는 method의 이름을 받아 처리

```html
<body>
  <div id="app">
    <button v-on:click="greet">Greet</button>
  </div>
  <script>
    var vm = new Vue({
      el: "#app",
      data: {
        name: "EVERYONE",
      },
      methods: {
        greet: function (event) {
          alert("Hello " + this.name + "!");
          console.dir(event.target); // 누구를 클릭했는가? 클릭한 버튼
          event.target.innerText = "눌려졌다"; //
        },
      },
    });
    // 또한 JavaScript를 이용해서 메소드를 호출할 수 있습니다.
    //vm.greet(); // => 'Hello Vue.js!'
  </script>
</body>
```

## 이벤트 수식어

이벤트 핸들러 내부에서 `event.preventDefault()` 또는 `event.stopPropagation()`를 호출하는 것은 매우 보편적인 일. 메소드 내에서 쉽게 이 작업을 할 수 있지만, DOM 이벤트 세부 사항을 처리하는 대신 데이터 로직에 대한 메소드만 사용할 수 있으면 더 좋을 것.

이 문제를 해결하기 위해, Vue는 v-on 이벤트에 이벤트 수식어 제공 -> 수식어는 점으로 표시된 접미사

- `.stop`
- `.prevent` <= 제일 많이 사용
- `.capture`
- `.self`
- `.once`
- `.passive`

### .prevent

prevent 수식어를 사용하기 이전에는 button을 사용시 중복으로 submit을 처리하는 결과가 발생

```html
<body>
  <div id="app">
    <form action="http://www.naver.com">
      <button v-on:click="greet1('SSAFY')">Greet</button>
      <button v-on:click="greet2($event, 'Ssafy')">Greet</button>
    </form>
  </div>
  <script>
    new Vue({
      el: "#app",
      methods: {
        // 둘의 차이 -> button은 기본적으로 submit 속성을 가지고 있기 때문에 form의 action도 처리
        greet1: function (msg) {
          alert("Hello " + msg + "!");
          console.dir(event.target);
        },
        greet2: function (e, msg) {
          if (e) e.preventDefault(); // button의 submit 속성 막기
          alert("Hello " + msg + "!");
          console.dir(e.target);
        },
      },
    });
  </script>
</body>
```

.prevent 수식어를 사용하면

```html
<body>
  <div id="app">
    <h2>페이지 이동</h2>

    <a href="test29.html" @click="sendMsg1">페이지 이동 막기1</a><br />
    <a href="test29.html" @click="sendMsg2">페이지 이동 막기2</a><br />
    <a href="test29.html" @click.prevent="sendMsg1">페이지 이동 막기3</a><br />
  </div>
  <script>
    new Vue({
      el: "#app",
      methods: {
        sendMsg1() {
          alert("이동할까요?");
        },
        sendMsg2(e) {
          e.preventDefault(); // 이동 막기
          alert("이동막기");
        },
      },
    });
  </script>
</body>
```

## 키 수식어

키보드 이벤트를 청취할 때, 종종 공통 키 코드를 확인해야 한다. Vue는 키 이벤트를 수신할 때 v-on에 대한 키 수식어를 추가할 수 있다.

- `.enter` : 엔터를 누르는 순간 <= 제일 많이 사용
- `.tab`
- `.delete` (“Delete” 와 “Backspace” 키 모두를 캡처합니다)
- `.esc`
- `.space`
- `.up` : 키보드 방향키를 누르는 순간
- `.down`
- `.left`
- `.right`

```html
<body>
  <div id="app">
    아이디 :<br />
    <input
      placeholder="검색할 아이디를 입력하세요"
      v-model="id"
      @keyup="sendId"
    /><br />
    <input
      placeholder="검색할 아이디를 입력하세요"
      v-model="id"
      @keyup.enter="sendId"
    /><br />
    <input
      placeholder="검색할 아이디를 입력하세요"
      v-model="id"
      @keyup.13="sendId"
    /><br />
    <button @click.once="sendId">검색</button>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        id: "",
      },
      methods: {
        sendId() {
          alert(this.id);
        },
      },
    });
  </script>
</body>
```
