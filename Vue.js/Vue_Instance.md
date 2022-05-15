# Vue Instance

> 뷰 인스턴스는 뷰로 화면을 개발하기 위해 필수적으로 생성해야하는 기본 단위

## 인스턴스 형식

```jsx
new Vue({
  ...
});
```

`newVue()` 로 인스턴스를 생성할 때 Vue를 **생성자**라고 한다.

## 옵션 속성

| 속성     | 설명                                                                                                                                  |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| el       | Vue가 적용될 요소 지정. CSS Selector or HTML Element                                                                                  |
| data     | Vue에 사용되는 정보 저장. 객체 또는 함수의 형태                                                                                       |
| template | 화면에 표시할 HTML, CSS등의 마크업 요소를 정의하는 속성. 뷰의 데이터 및 기타 속성들도 함께 화면에 그릴 수 있다.                       |
| methods  | 화면 로직제어와 관계된 method를 정의하는 속성. 마우스 클릭 이벤트 처리와 같이 화면의 전반적인 이벤트와 화면 동작과 관련된 로직을 추가 |
| created  | 뷰 인스턴스가 생성되자마자 실행할 로직을 정의 - life cycle hook                                                                       |

## Vue Instance의 유효범위

> 뷰 인스턴스를 생성하면 HTML의 특정 범위 안에서만 옵션 속성들이 적용되어 나타난다. 이를 인스턴스 유효 범위라고 한다.

- Vue Instance를 생성하면 HTML의 `특정 범위 안에서만 옵션 속성들이 적용`
- el 속성과 밀접한 관계가 있다!! -> el 속성의 값이 가진 돔 요소가 인스턴스의 유효 범위이기 때문

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
    </div>
    <script>
      new Vue({
        // Vue 객체 자동으로 만들기 - newVue
        el: "#app",
        data() {
          return {
            message: "안녕 VueJS!!!", //함수처럼 사용
          };
        },
      });
    </script>
  </body>
</html>
```

#app 이 el이므로, `<div id="app">{{message}}</div>` 가 유효범위인 것이다.

### 순서

1. Vue()로 인스턴스가 생성됨
2. el 속성에 지정한 화면 요소(돔)애 인스턴스가 부착됨
3. el 속성에 인스턴스가 부착된 후(`mount`) data 속성이 el 속성에 지정한 화면 요소와 그 이하 레벨의 화면 요소에 적용되어 값이 치환

## Vue Instance Life Cycle

**인스턴스의 상태에 따라 호출할 수 있는 속성들**을 라이프 사이클 속성이라고 한다.
그리고 **각 라이프 사이클 속성에서 실행되는 커스텀 로직**을 라이프 사이클 훅(hook)이라고 한다.
![](https://velog.velcdn.com/images/jmlee9707/post/20ccf69a-09ac-4ed4-819a-a48873e7e0ec/image.png)

> Life Cycle은 크게 나누면 Instance의 `생성`, 생성된 Instance를 화면에 `부착`, 화면에 부착된 Instance의 내용이 `갱신`, Instance가 제거되는 `소멸`의 4단계로 나뉜다. (생성과 갱신이 가장 중요!)

| life Cycle 속성 | 설명                                                                                                                                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| beforeCreate    | Vue Instance가 생성되고 각 정보의 설정 전에 호출. DOM과 같은 화면요소에 접근 불가                                                                                                                                  |
| `created`       | Vue Instance가 생성된 후 데이터들의 설정이 완료된 후 호출, Instance가 화면에 부착하기 전이기 때문에 template 속성에 정의된 DOM 요소는 접근 불가. 서버에 데이터를 요청(http 통신)하여 받아오는 로직을 수행하기 좋다 |
| beforeMount     | 마운트가 시작되기 전에 호출, vue와 div 연결 전                                                                                                                                                                     |
| mounted         | 지정된 element에 Vue Instance 데이터가 마운트 된 후에 호출. template 속성에 정의한 화면 요소에 접근할 수 있어 화면 요소를 제어하는 로직 수행.                                                                      |
| beforeUpdate    | **데이터가 변경될 때 virtual DOM이 랜더링**, 패치 되지 전에 호출                                                                                                                                                   |
| `updated`       | **Vue에서 관리되는 데이터가 변경되어 DOM이 업데이트 된 상태**. 데이터 변경 후 화면 요소 제어와 관련된 로직을 추가                                                                                                  |
| beforeDestroy   | Vue Instance가 제거되기 전에 호출                                                                                                                                                                                  |
| destroyed       | Vue Instance가 제거된 후에 호출                                                                                                                                                                                    |

> created와 update가 가장 중요, 어떠한 데이터가 뿌려지기 전. BE와 통신을 해서 데이터를 얻어옴 -> **ajax를 통해 비동기 통신을 함**

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
      <h2>클릭 카운트 : {{count}}</h2>
      <button @click="count++">카운트 증가</button>
      <!--@함수 = "이벤트"-->
    </div>
    <script>
      new Vue({
        // Vue 객체 자동으로 만들기 - newVue
        el: "#app", // app과 연결
        data() {
          return {
            count: 0, //이 안에서는 변수를 사용 가능, 연결
          };
        },
        beforeCreate() {
          console.log("beforeCreate 호출!!!");
          // console.log("beforeCreate count : "+ count); // 에러 isnotdefined -> count가 누구 것인지 모르기 때문
          console.log("beforeCreate count : " + this.count); // vue 객체 안에 있는 count, undefined - 만들어지고 초기화가 되기 않았을떼
        },
        created() {
          console.log("created 호출!!!"); // 생성시에 호출
          console.log("created count : " + this.count); // el과 data가 초기화되는 시점
          console.log("created el : " + this.$el); // div와 연결 전
        },
        beforeMount() {
          console.log("beforeMount호출!!!");
          console.log("beforeMount count : " + this.count);
        },
        mounted() {
          console.log("mounted 호출!!!"); // div와 vue 연결시에 호출
          console.log("mounted count : " + this.count);
          console.log("created el : " + this.$el); // div와 연결!
        },
        updated() {
          console.log("updated 호출!!!"); // 카운트 값이 바뀌면 호출
        },
        destroyed() {
          console.log("destroyed 호출!!!");
        },
      });
    </script>
  </body>
</html>
```
