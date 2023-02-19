# 8장 AJAX를 활용해 새로고침 없이 데이터 갱신하기

## 8.1 질문/답변 게시판 구현

## 8.2 AJAX 활용해 답변 추가, 삭제 실습

브라우저가 서버에서 HTML 응답을 받아 처리하는 과정
1. HTML 응답을 받으면 브라우저는 먼저 HTML을 라인 단위로 읽어내려가면서 서버에 재요청이 필요한 부분(CSS, 자바스크립트, 이미지 등)을 찾아 서버에 다시 요청을 보낸다.
2. 서버에서 자원을 다운로드하면서 HTML DOM 트리를 구성한다.
3. 서버에서 CSS 파일을 다운로드하면 앞에서 생성한 HTML DOM 트리에 CSS 스타일을 적용한 후 모니터 화면에 그리게 된다.

매번 서버에 요청을 보내 위 과정 전체를 실행하는 것은 상당히 불합리하다.
이 같은 단점을 보완하기 위해 등장한 기술이 AJAX(Asynchronous JavaScript and XML)이다.

### 8.2.1 답변하기

> ### 이슈사항 1
> 1. Answer 클래스의 createdDate 타입을 LocalDateTime으로 지정했다.
> 2. Answer 객체를 Jackson 라이브러리를 통해 JSON으로 변환한다.
> 3. 타입 에러가 발생하였다.
> ### 해결방안 1
> 1. jackson-datatype-jsr310 라이브러리를 추가한다.
> 2. JavaTimeModule 모듈을 등록한다.
> ```java
> ObjectMapper mapper = new ObjectMapper();
> mapper.registerModule(new JavaTimeModule());
> ```

> ### 이슈사항 2
> 1. LocalDateTime을 JSON 응답으로 받을 때 배열 형식으로 바뀐다.
> ### 해결방안 2
> 1. Answer 클래스의 createdDate에 @JsonFormat을 등록한다.
> ```java
> @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd'T'HH:mm:ss.SSS")
> @JsonSerialize(using = LocalDateTimeSerializer.class)
> @JsonDeserialize(using = LocalDateTimeDeserializer.class)
> private LocalDateTime createdDate;
> ```

### 8.2.2 답변 삭제하기 실습

> ### 이슈사항 1
> 1. 삭제 버튼 클릭 이벤트가 받기 전에 submit 되어버린다.
> ```javascript
> $(".qna-comment button[type=submit]").click(deleteAnswer);
> ```
> ### 해결방안 1
> 1. 삭제 버튼이 동적으로 생겨 이벤트 바인딩이 되지 않아 문제였다.
> 2. 페이지가 처음 로드될 때 존재하는 부모 요소에 바인딩 후 하위 요소에 이벤트 처리를 위임한다.
> ```javascript
> $(".qna-comment").on('click', '.form-delete', deleteAnswer);
> ```

> ### 이슈사항 2
> 1. 삭제 후 처리하는 부분에서 `$(this)`가 비어있다.
> ```javascript
> function onSuccessDelete(json, status) {
>   if (json.status) {
>     $(this).closest(".article").remove();
>   } else {
>     alert(json.message);
>   }
> }
> ```
> ### 해결방안 2
> 1. 삭제 후 성공 이벤트를 처리할 때 컨텍스트가 변경되어 `$(this)`가 클릭한 요소를 가질 수 없었다.
> 2. 성공 후 처리를 같은 컨텍스트에서 하거나 클릭 할때 `this`를 파라미터로 넘겨준다.
> ```javascript
> $.ajax({
>   type : 'post',
>   url : '/api/qna/deleteAnswer',
>   data : queryString,
>   dataType : 'json',
>   error : onError,
>   success : (json, status) => {
>     if (json.status) {
>       $(this).closest(".article").remove();
>     } else {
>       alert(json.message);
>     }
>   }
> });
> ```

## 8.3 MVC 프레인워크 요구사항 2단계

### 8.3.1 요구사항 분리 및 힌트



To be continued...