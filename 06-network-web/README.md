# 06. 네트워크와 웹

AI 서비스는 결국 API로 주고받는다. 데이터가 네트워크를 어떻게 오가는지 이해하고, API 서버를 직접 만들어 보는 단계.

## 학습 목표

- 브라우저에 주소를 입력했을 때 일어나는 일을 단계별로 설명할 수 있다.
- HTTP 요청과 응답의 구조를 이해하고, REST API를 설계할 수 있다.
- FastAPI로 간단한 API 서버를 만들 수 있다.

## 세션 계획

| # | 주제 | 내용 |
|---|---|---|
| s01 | 네트워크 기초 | OSI 7계층, TCP/IP 4계층, 패킷과 캡슐화 |
| s02 | 전송 계층 | IP 주소, 포트, TCP vs UDP, 3-way handshake |
| s03 | DNS와 URL | DNS 동작, URL 구조, "주소창에 URL을 치면 일어나는 일" |
| s04 | HTTP | 요청/응답 구조, 메서드(GET/POST/PUT/DELETE), 상태 코드, 헤더, HTTPS |
| s05 | JSON과 REST | JSON, REST API 설계 원칙, `requests`로 공개 API 호출 |
| s06 | FastAPI ① | 첫 서버 띄우기, 라우팅, 경로/쿼리 파라미터 |
| s07 | FastAPI ② | 요청 바디 검증(Pydantic), 에러 처리, 자동 API 문서 |
| s08 | 인증 기초 | 세션 vs 토큰, API 키, JWT 개념, CORS |
| s09 | 종합 실습 | **메모리에 데이터를 저장하는 CRUD API 서버** (07에서 DB를 연결) |

## 체크리스트

- [ ] TCP와 UDP의 차이를 설명할 수 있다
- [ ] 상태 코드 200, 201, 400, 401, 403, 404, 500의 의미를 안다
- [ ] REST API의 엔드포인트를 직접 설계할 수 있다
- [ ] FastAPI로 CRUD API를 만들었다
- [ ] LLM API를 호출할 때 API 키를 어떻게 다뤄야 하는지 설명할 수 있다
