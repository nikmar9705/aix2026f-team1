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

Candidate B
**목표(스펙) / Spec**
- 입력 Input: Candidate B의 문제와 "쓰고 싶어도 못 쓰는 사람은 누구인가?" 항목
- 처리 Processing: AI에게 해당 항목에 들어갈 사람이 있는지 질문함
- 출력 Output: 특정 접근 권한이나 인증이 필요하지 않아 구조적으로 못 쓰는 사람은 없다고 판단됨. 다만 정보가 특정 커뮤니티 중심으로 공유되어 인지도 격차가 있을 수 있다는 점이 논의됨
- 실패 조건 Failure: 존재하지 않는 이해관계자를 임의로 추가하는 것

**요청한 프롬프트 요지 / Prompt (summary)**
Candidate B에서 "쓰고 싶어도 못 쓰는 사람은 누구인가?"에 누가 해당하는지, 그리고 특정 커뮤니티 중심 정보 공유 문제도 여기 해당하는지 질문함

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because: 구조적 접근 제약이 없다는 판단을 채택함 ("없음"으로 기재)
- 수정한 부분과 이유 / Changed, because: 처음에는 특정 커뮤니티 밖 학생을 "못 쓰는 사람"으로 기재하려 했으나, 이는 접근 제약이 아니라 인지도(홍보 범위) 문제임을 확인하고 이 항목에서는 제외함
- 폐기한 부분과 이유 / Rejected, because: 특정 커뮤니티 이름을 worksheet에 명시하는 것은 부적절하다고 판단하여 제외함

**검증 방법 / How it was verified**
도구 사용에 필요한 조건(가입, 인증 등)이 있는지 현재 설계 흐름을 검토하여 판단함

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
