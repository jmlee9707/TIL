# Component

## 컴포넌트란?

- Vue의 가장 강력한 기능 중 하나
- HTML Element를 확장하여 재사용 가능한 코드를 캡슐화
- Vue Component는 Vue Instance이기도 하기 때문에 모든 옵션 객체를 사용
- Life Cycle Hook 사용 가능
- 전역 컴포넌트와 지역 컴포넌트
- data는 반드시 함수형으로만 사용가능!

## 전역 컴포넌트

컴포넌트 등록하기

- 전역 컴포넌트를 등록하려면, `Vue.component(tagNmae, options)`를 활용
- 권장하는 컴포넌트 이름 :`케밥 표기법(전부 소문자, -)`
- 컴포넌트 이름엔 합성어를 사용 : todo-item
- 컴포넌트의 데이터는 반드시 함수 : 아닐 시 에러 발생

`케밥-표기법`

```jsx
Vue.component("my-component-name", {
  /* ... */
});
```

케밥-표기법으로 컴포넌트를 정의할 때는 사용자 정의 엘리먼트를 부를 때에도 my-component-name와 같이 반드시 케밥-표기법을 사용

`파스칼표기법`

```jsx
Vue.component("MyComponentName", {
  /* ... */
});
```

파스칼표기법으로 컴포넌트를 정의할 때는 사용자 정의 엘리먼트를 부를 때 두 가지 표기법 모두 사용할 수 있다. 즉 my-component-name와 MyComponentName 모두 가능.
**단, DOM에 바로 쓸 때는 케밥-표기법 이름만 가능.**

```html
<body>
  <div id="app1">
    <my-global></my-global>
    <my-global></my-global>
    <MyGlobal></MyGlobal
    ><!--불가능-->
  </div>
  <div id="app2">
    <my-global></my-global>
    <my-global></my-global>
  </div>
  <script>
    // 전역 컴포넌트 설정
    Vue.component("MyGlobal", {
      // option 설정 가능
      template: `
      <h2>전역 컴포넌트 입니다</h1>
      `,
    });
    new Vue({
      el: "#app1",
    });
    new Vue({
      el: "#app2",
    });
  </script>
</body>
```

## 지역 컴포넌트

- 컴포넌트를 components 인스턴스 옵션으로 등록함으로써 다른 인스턴스/컴포넌트의 범위에서만 사용할 수 있는 컴포넌트를 만들 수 있다

```html
<body>
  <div id="app1">
    <my-local></my-local>
    <my-local></my-local>
  </div>
  <div id="app2">
    <my-local></my-local>
    <my-local></my-local>
  </div>
  <script>
    new Vue({
      el: "#app1",
      // 지역 컴포넌트 설정, app1에서만 사용 가능
      components: {
        MyLocal: {
          template: `<h2>지역 컴포넌트 입니다</h2>`,
        },
      },
    });
    new Vue({
      el: "#app2",
    });
  </script>
</body>
```

## Component Template

```html
<body>
  <div id="app">
    <my-comp></my-comp>
  </div>
  <!-- template 설정 -->
  <template id="mycomp">
    <div>
      <h2>{{msg}}</h2>
    </div>
  </template>
  <script>
    Vue.component("MyComp", {
      template: "#mycomp", // id로 끄집어오기
      data() {
        return {
          msg: "hello component",
        };
      },
    });

    new Vue({
      el: "#app",
    });
  </script>
</body>
```

## 컴포넌트간 통신

- 상위(부모) - 하위(자식) 컴포넌트 간의 data 전달 방법
- 부모에서 자식 : props라는 특별한 속성을 전달(Pass props)
- 자식에서 부모 : event로만 전달 가능(Emit Event) => 사용자 정의 이벤트를 처리해야함
  > 뷰가 추구하는 전달은 단방향 데이터 전달이기 때문 => 부모는 자식에게 데이터를 전달할 수 있지만 자식은 부모에게 데이터를 전달할 수 없다

![](https://velog.velcdn.com/images/jmlee9707/post/c3ecbfe6-331c-4768-ad4a-afd43922cb0a/image.png)

### props : 상위에서 하위 컴포넌트로 data 전달

- 하위 컴포넌트는 상위 컴포넌트의 값을 직접 참조 불가능
- data와 마찬가지로 props 속성의 값을 **template에서 사용이 가능**

`props`

```html
<body>
  <div id="app">
    <h2>props test</h2>
    <input tyoe="text" v-model="msg" /><br />
    <!-- <child-component pdata="안녕하세요 props 입니다"></child-component> -->
    <child-component :pdata="msg"></child-component>
  </div>
  <script>
    //하위 컴포넌트
    Vue.component("ChildComponent", {
      template: `<span>{{pdata}}</span>`,
      props: {
        // 여러개를 넘길 수 있기 때문에 객체 타입
        pdata: {
          type: String,
          required: true,
          // props 유효성 검사
        },
      },
    });
    new Vue({
      el: "#app",
      data() {
        return {
          msg: "",
        };
      },
    });
  </script>
</body>
```
