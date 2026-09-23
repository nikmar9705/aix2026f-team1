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

## [이슈 # 4] 음식 적합 여부 정보 확인 시스템 / Food Suitability Information System

### **목표(스펙) / Spec**

- 입력 Input:
  - 사용자가 식당 또는 음식 항목을 검색하거나 선택한다.

- 처리 Processing:
  - 시스템은 검색된 식당 또는 음식의 할랄/섭취 가능 여부와 관련된 정보를 확인한다.
  - 음식의 재료 및 관련 정보를 확인한다.
  - 충분한 정보가 없는 경우 정보가 없음을 판단한다.

- 출력 Output:
  - 식당의 할랄/섭취 가능 음식 정보를 표시한다.
  - 선택한 음식의 재료 및 할랄/섭취 가능 여부 정보를 표시한다.
  - 확인 가능한 정보가 없을 경우 안내 문구를 표시한다.

- 실패 조건 Failure:
  - 식당 또는 음식 항목에 대한 충분한 할랄/섭취 가능 여부 정보를 찾을 수 없는 경우
  - 시스템은 "정보를 확인할 수 없습니다"라는 안내 문구를 표시한다.

### **요청한 프롬프트 요지 / Prompt (summary)**

사용자가 식당이나 음식 항목을 검색하거나 선택했을 때 할랄/섭취 가능 여부와 관련 정보를 확인할 수 있도록 하고, 충분한 정보가 없는 경우 사용자에게 안내하도록 요청했다. 정상 경로 2개와 실패 경로 1개를 포함하여 EARS 형식의 Acceptance Criteria를 작성하도록 요청했다.

### **결과에 대한 판단 / Decisions**

- 채택한 부분과 이유 / Accepted, because:
  - 사용자가 식당을 검색하면 해당 식당의 할랄/섭취 가능 음식 정보를 표시하는 기능을 채택했다.
  - 사용자가 음식 항목을 선택하면 재료 및 할랄/섭취 가능 여부 정보를 표시하는 기능을 채택했다.
  - 정보가 충분하지 않을 경우 "정보를 확인할 수 없습니다"라는 안내를 표시하는 실패 경로를 채택했다.
  - 위 기능들은 WHEN, THE, SHALL 또는 IF, THEN THE, SHALL 구조로 작성할 수 있어 EARS의 조건, 주체, 동작, 결과를 명확하게 표현할 수 있기 때문이다.

- 수정한 부분과 이유 / Changed, because:
  - 기존의 대상 사용자를 "international student"에서 "user"로 수정했다.
  - 시스템을 특정 사용자뿐만 아니라 일반적인 사용자가 사용할 수 있도록 더 일반적인 표현으로 작성하기 위해 수정했다.
  - "빠르게"와 같은 모호한 표현 대신 "3분 이내"와 같이 확인 가능한 기준을 사용했다.

- 폐기한 부분과 이유 / Rejected, because:
  - 사용자가 직접 식당에 문의하거나 다른 곳에서 추가로 검색해야 하는 과정은 시스템의 주요 기능에 포함하지 않았다.
  - 이는 시스템이 직접 수행하거나 검증할 수 있는 기능으로 명확하게 정의하기 어렵기 때문에 제외했다.

### **검증 방법 / How it was verified**

- 사용자가 식당을 검색했을 때 할랄/섭취 가능 음식 정보가 3분 이내에 표시되는지 확인한다.
- 사용자가 음식 항목을 선택했을 때 재료 및 할랄/섭취 가능 여부 정보가 표시되는지 확인한다.
- 충분한 정보가 없는 식당 또는 음식 항목을 검색했을 때 "정보를 확인할 수 없습니다"라는 안내 문구가 표시되는지 확인한다.
- 각 Acceptance Criteria가 정상 경로 2개와 실패 경로 1개로 구성되어 있는지 확인한다.
- 각 요구사항이 조건, 시스템 주체, 동작 및 결과를 포함하여 EARS 형식으로 작성되었는지 확인한다.
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
