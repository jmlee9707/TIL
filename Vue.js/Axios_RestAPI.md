# axios

> [axios 가이드](https://axios-http.com/kr/docs/intro)

## HTTP 통신 : axios

> axios는 브라우저, Node.js를 위한 Promise API를 활용하는 HTTP 통신 라이브러리이다

- Vue에서 권고하는 HTTP 통신 라이브러리는 axios이다
- Promise 기반의 HTTP 통신 라이브러리이며 상대적으로 다른 HTTP 통신 라이브러리들에 비해 문서화가 잘되어 있고 API가 다양하다
- axios.get(URL)<<Promise객체를 return>> then, catch 사용가능
- [axios github](https://github.com/axios/axios)

### Promise란?

서버에 데이터를 요청하여 받아오는 동작과 같은 **비동기 로직 처리에 유용한 자바스크립트 라이브러리**이다. 자바스크립트는 단일 스레드로 코드를 처리하기 때문에 특정 로직의 처리가 끝날 때까지 기다려주지 않는다.

**따라서 데이터를 요청하고 받아올 때까지 기다렸다가 화면에 나타내는 로직을 실행해야할때 주로 Promise를 활용한다.**

그리고 데이터를 받아왔을 때 Promise로 데이터를 화면에 표시하거나 연산을 수행하는 등 특정 로직을 수행한다.

### 비동기 방식이란?

비동기 방식은 웹페이지를 리로드하지 않고 데이터를 불러오는 방식이며, 서버에 요청을 한 후 멈추어 있는 것이 아니라 그 프로그램은 계속 돌아간다는 의미를 내포

페이지 리로드의 경우 전체 리소스를 다시 불러와야하는데 모든 리소스를 다시 불러오게 될 경우 불필요한 리소스 낭비가 발생하게 된다. 하지만 비동기식 방식을 이용할 경우엔 필요한 부분만 불러와 사용할 수 있다.

## REST

> REST : 웹에서 데이터를 전송하고 처리하는 방법을 정의한 인터페이스

### REST 구성요소

1. URI로 자원 구분 : http://localhost:8080/**\_\_\_**

2. METHOD로 자원에 대한 행위 구분(동작)

- GET : 자원 조회 `Read`
- POST : 자원 생성 `Create`
- PUT : 자원의 전체 항목 수정 `Update`
- fetch : 자원의 일부 항목 수정 `Update`
- DELETE : 자원의 삭제 `Delete`

3. Representation으로 자원 나타내기 =>XML, JSON, RSS 포맷 등

### REST API

> REST 서비스를 구현한 것

REST URI 설계 가이드

1. 동사 지양, 명사 지향 (작업, 행위 표현 X => method 설계)
2. \_(언더바) 지양, -(하이푼) 지향
3. 마지막 / 사용X
4. 확장자 사용 X ( / / / ~.jsp)

### RESTful

> REST 아키텍처를 잘 준수한 것

### REST API 장단점

장점

- HTTP 프로토콜을 사용해 추가적인 인프라 구축이 필요없다
- C/S 환경 구축이 가능하다 -> 분리된 개발이 가능
  단점
- 표준화X

## Promise

직접 promise 객체 만들기

```html
<script>
  // Promise 객체 생성
  const promise = new Promise((resolve, reject) => {
    console.log("promise 객체 생성.");
    resolve("성공 ^^!!");
    // reject("실패 ㅠㅠ!!");
  });

  promise
    .then((response) => {
      console.log("then : " + response);
    })
    .catch((error) => {
      console.log("catch : " + error);
    })
    .finally(() => {
      console.log("finally 실행");
    });
</script>
```

axios는 promise 기반

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
  const URL = "https://jsonplaceholder.typicode.com/todos/1"; // 서버와의 통신, json을 얻어올 수 있음
  // 1. 예전 방식
  const xhr = new XMLHttpRequest(); // ajax
  xhr.open("GET", URL); // get 방식으로 url 오픈
  xhr.send();

  const response = xhr.response; // 값 담기
  console.log("1.", response); // 얻어왔는지 확인 => 데이터 못 넘어옴

  xhr.onload = function () {
    if (xhr.status == 200) {
      const response = xhr.response;
      console.log("2.", response);
    }
  };

  // 2. axios이용하면 promise 객체를 만들지 않아도 리턴
  const promise = axios.get(URL);
  console.log("3", promise);
  promise
    .then((response) => {
      console.log("4", response);
      return response.data; // 여기서 만든 객체가 promise
    })
    .then((data) => {
      console.log("5", data);
      return data.id;
    })
    .then((id) => {
      console.log("6", id);
    })
    .catch((error) => {
      console.log(error);
    })
    .finally(() => {
      console.log("finally");
    });
</script>
```

```html
<script>
  const URL = "https://jsonplaceholder.typicode.com/todos";

  axios
    .get(URL)
    .then((response) => {
      console.log("4", response.data[4].id); // 배열의 5번째 값의 id 얻어오기
      return response.data[4].id;
    })
    .then((id) => {
      console.log("5", id);
      return axios.get(`${URL}/${id}`); // url로 이동 또 promise 리턴
    })
    .then((todo) => {
      // 한개의 값만 얻어옴
      console.log("6", todo.data.title);
    })
    .catch((error) => {
      console.log(error);
    })
    .finally(() => {
      console.log("finally");
    });
</script>
```

### async 와 await

```html
<script>
  const URL = "https://jsonplaceholder.typicode.com/todos";

  // 콜백함수 지옥.. .then 체인이 너무 많을 경우 -> async나 await 사용하기
  async function getTodo(URL) {
    // async 비동기 처리
    const response = await axios.get(URL); // await는 async 안에서만 사용 가능, 다 얻어와야만 다음으로 내려가기
    const id = response.data[4].id;
    const todo = await axios.get(`${URL}/${id}`);
    console.log(todo.data.title);
  }

  getTodo(URL).catch((error) => {
    // 에러발생시 로그 찍기
    console.log(error);
  });
</script>
```

<!--http://localhost:9999/vue/swagger-ui/index.html#/board-controller/retrieveBoardUsingGET-->
