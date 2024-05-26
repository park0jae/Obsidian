AI 면접 질문으로 전혀 생각하지 못하고 있던 해당 질문이 나와서 당황했다. 개발을 진행하면서 많이 봤던 HTTP 요청의 Content-Type인데 막상 대답을 하려니 정리가 잘 되지 않아 이번 기회에 다시 정리해보자...

## Content-Type ?
> Content-Type은 API 연동 시, 보내는 자원을 명시하기 위해 보통 사용한다. HTTP 요청 시 Message Body에 들어가는 타입을 HTTP Header에 명시해줄 수 있는데 이때 명시해줄 수 있도록 하는 필드가 Content-Type


## application/json과 application/x-www-form-urlencoded 차이점

요즘 대부분의 request에 대한 Content-Type은 application/json 타입인 것이 많습니다.

**application/json**은 RestFul API를 사용하게 되며 request를 날릴 때 대부분 json을 많이 사용하게 됨에 따라 자연스럽게 사용이 많이 늘게 되었다.

**application/x-www-form-urlencoded**는 html의 form의 기본 Content-Type으로 요즘은 자주 사용하지 않지만 여전히 사용하는 경우가 종종 존재합니다.

차이점은 
- application/json은 {key: value}의 형태로 전송
- application/x-www-form-urlencoded는 key=value&key=value의 형태로 전달

즉 application/x-www-form-urlencoded는 보내는 데이터를 URL인코딩 이라고 부르는 방식으로 인코딩 후에 웹 서버로 보내는 방식을 의미합니다. 따라서 별도의 인코딩이 필요하여 대용량 파일 요청 시 적합하지 않습니다.

> URL Encoding ?
> : URL로 사용할 수 없는 문자(특수문자, 예약문자 등)들을 사용할 수 있도록 인코딩 해주는 것으로 인코딩 된 문자는 Triplet(트리플 렛, 세 개가 한묶음)들로 인코딩 되며 '%'로 시작하며 뒤의 숫자는 16진수 들로 표현됨


## 최종 정리
### `application/json`

- **형식**: JSON (JavaScript Object Notation)
- **용도**: 주로 API 호출에서 복잡한 데이터 구조를 전송할 때 사용
- **데이터 인코딩**: JSON 형식으로 인코딩 (예: `{"name": "John", "age": 30}`)
- **서버 파싱**: 서버에서 JSON 파서가 필요

### `application/x-www-form-urlencoded`

- **형식**: URL 인코딩 (폼 데이터 인코딩)
- **용도**: HTML 폼 데이터 전송에 주로 사용 (특히 GET 또는 POST 요청)
- **데이터 인코딩**: 키-값 쌍을 URL 인코딩 방식으로 인코딩 (예: `name=John&age=30`)
- **서버 파싱**: 대부분의 서버에서 기본적으로 지원하는 URL 인코딩 파서 사용

