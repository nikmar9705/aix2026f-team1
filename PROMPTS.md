# AI 협업 기록 / AI Collaboration Log

작성 원칙: 프롬프트 나열이 아니라 **판단 근거**를 남긴다.
Principle: record your **reasoning**, not just prompts.

---

## [이슈 #2__] 제목 / Title

**목표(스펙) / Spec**
- 입력 Input: Section 2 memo search features instruction
- 처리 Processing: Pasting the promppt into AI agent(Claude)
- 출력 Output: 2 modifications on the service.js and route.js codes
- 실패 조건 Failure: none

**요청한 프롬프트 요지 / Prompt (summary)**

"""
Task Summary

Goal: Add a memo search feature to existing Node.js/Express API

Endpoint: GET /memos/search?q=keyword

Functionality:

-Search memos by keyword in title OR body
-Return only authenticated user's own memos
-Return 400 error if q is empty

Response Format: { ok: true, data: [...] } or { ok: false, error: "ERROR_CODE" }

Constraints (Conventions):

-Database queries only in service.js
-HTTP handling only in routes.js
-Function names: verb-first (e.g., searchMemos)
-Variables: camelCase
-DB columns: snake_case
-All queries must include user_id filter
-Input validation required (return 400 on invalid input)

Database Schema:

-users table (id, email, name, created_at)
-memos table (id, user_id, title, body, created_at)
-Index on memos(user_id)

Existing Functions: listMemos(), getMemo(), createMemo()

Required Deliverables:

1. searchMemos(userId, keyword) function in service.js
2. GET /memos/search route in routes.js

"""

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because: 전체 채택. schema.sql과 대조한 결과
  지어낸 컬럼이 없었고, 쿼리에 WHERE user_id = ? 가 포함되어 있었으며,
  계층 분리·응답 형식·에러 코드 규약을 모두 지켰다. 기존 listMemos, getMemo와
  작성 방식도 같았다.

- 수정한 부분과 이유 / Changed, because: 없음. 라우트를 /memos/:id 보다 위에
  배치해야 한다는 점도 에이전트가 스스로 알려줬다.

- 폐기한 부분과 이유 / Rejected, because: 없음. 다만 함수명 searchMemos의
  "search"가 CONVENTIONS.md의 허용 동사 목록(list, get, create, update, remove)에
  없어 규약 위반 1건으로 기록했다.

**검증 방법 / How it was verified**

By looking at the complexity and how long it takes to run each codes.

---

## [이슈 #_3_] 제목 / Title

Candidate A
**목표(스펙) / Spec**
- 입력 Input: 3주차 활동지 1~3절 양식과 팀이 정한 방향(한국어 포스터를 읽기 어려운 외국인 유학생, 참여할 행사·공모전을 찾는 상황)
- 처리 Processing: AI(Claude)에게 활동지 규칙(사용자는 구체적으로, 해결책은 쓰지 않기, 질문은 과거형)에 맞게 후보 A의 네 칸, 2절 이해관계자, 3절 확인 계획과 인터뷰 질문을 작성하도록 요청함
- 출력 Output: 1절 네 칸과 한 문장 요약, 2절 두 칸, 3절 확인 계획 1행과 인터뷰 질문 3개
- 실패 조건 Failure: 사용자가 "외국인"처럼 막연하게 남음 / 칸 안에 웹사이트·AI 검수 같은 해결책이 들어감 / 인터뷰 질문이 미래 의향("쓰시겠어요?")을 물음

**요청한 프롬프트 요지 / Prompt (summary)**
1절 요청 원문: "i want to make the scattered notices, so lets only discuss on that. I need the who: mainly foreigners that struggled with reading posters about event or competition. I want it to be a website and also peole can post a poster too after being review by AI to confirm the event is legit and being host. when: people that want to find an event to enter. But i want to focus on answering the who, when, where, done"
이후 2절, 3절을 각각 같은 기준으로 작성해 달라고 요청함.

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because: 1절 네 칸, 2절 "누구의 정보" 칸, 3절 번역기 확인 계획과 인터뷰 질문. "외국인"을 "한국어 포스터를 읽기 어려운 에리카 외국인 유학생"으로 좁혀 인터뷰 대상이 분명해졌고, 성공 기준이 웹사이트가 아니라 "마감 전에 지원한다"는 결과로 쓰여 4주차 EARS로 옮기기 쉽다.
- 수정한 부분과 이유 / Changed, because: 2절 "못 쓰는 사람"에서 AI가 제안한 두 예시 중 "영어도 편하지 않은 유학생"만 남겼다. 3절에서 AI는 후보 A용 가정을 3개 제안했지만, 표를 B·C와 나눠 써야 해서 번역기 테스트 1개만 넣었다. 다른 후보가 모두 인터뷰를 쓰므로 방법이 겹치지 않는 것을 골랐다.
- 폐기한 부분과 이유 / Rejected, because: 웹사이트와 AI 검수 기능을 1절에 넣지 않았다. 해결책은 4~5주차에 정한다는 활동지 규칙 때문이다.

**검증 방법 / How it was verified**

Candidate B
**목표(스펙) / Spec**
- 입력 Input:
- 처리 Processing:
- 출력 Output:
- 실패 조건 Failure:

**요청한 프롬프트 요지 / Prompt (summary)**

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because:
- 수정한 부분과 이유 / Changed, because:
- 폐기한 부분과 이유 / Rejected, because:

**검증 방법 / How it was verified**

Candidate C
**목표(스펙) / Spec**
- 입력 Input: Candidate C의 문제와 “쓰고 싶어도 못 쓰는 사람은 누구인가?” 항목
- 처리 Processing: AI에게 해당 항목에 들어갈 사람을 질문함.
- 출력 Output: 별도의 제3자는 없다고 판단함
- 실패 조건 Failure: 존재하지 않는 이해관계자를 임의로 추가하는 것

**요청한 프롬프트 요지 / Prompt (summary)**
 Candidate C에서 “쓰고 싶어도 못 쓰는 사람은 누구인가?”에 누가 해당하는지 질문함.
   
**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because: 별도의 제3자가 없다는 판단을 채택함.
- 수정한 부분과 이유 / Changed, because: “없음”에서 이유를 덧붙여 구체적으로 작성함.
- 폐기한 부분과 이유 / Rejected, because: 근거 없이 다른 이해관계자를 추가하는 것은 적절하지 않아 제외함.

**검증 방법 / How it was verified**
현재 문제의 정보 흐름을 검토하여 판단함.
---

## [이슈 #__] 제목 / Title

**목표(스펙) / Spec**
- 입력 Input:
- 처리 Processing:
- 출력 Output:
- 실패 조건 Failure:

**요청한 프롬프트 요지 / Prompt (summary)**

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because:
- 수정한 부분과 이유 / Changed, because:
- 폐기한 부분과 이유 / Rejected, because:

**검증 방법 / How it was verified**

---
