# AI 협업 기록 / AI Collaboration Log

작성 원칙: 프롬프트 나열이 아니라 **판단 근거**를 남긴다.
Principle: record your **reasoning**, not just prompts.

---

## [이슈 # 2_] 제목 / Title

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
- 입력 Input: 사용자가 식당 또는 음식을 검색/선택한다.
- 처리 Processing: 시스템이 할랄/섭취 가능 여부와 음식 정보를 확인한다.
- 출력 Output: 음식 정보와 재료를 표시하거나, 정보가 없으면 안내한다.
- 실패 조건 Failure: 충분한 정보가 없으면 "정보를 확인할 수 없습니다"를 표시한다.

### **요청한 프롬프트 요지 / Prompt (summary)**
Week 4 Acceptance Criteria를 바탕으로 정상 경로 2개와 실패 경로 1개의 EARS Acceptance Criteria를 작성하도록 요청했다.

### **결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because: 식당 검색, 음식 정보 확인, 정보 없음 안내를 채택했다.
- 수정한 부분과 이유 / Changed, because: "international student"를 "user"로 변경하고, "빠르게"를 "3분 이내"로 변경했다.
- 폐기한 부분과 이유 / Rejected, because: 식당에 직접 문의하거나 다른 곳에서 검색하는 과정은 시스템 기능에서 제외했다.

### **검증 방법 / How it was verified**
각 Acceptance Criteria가 정상 경로 2개와 실패 경로 1개로 구성되고, EARS 형식과 명확한 완료 조건을 갖추었는지 확인했다.

---

## [이슈 # 5] 태스크 분해 및 의존 관계 작성 / Task Decomposition

**목표(스펙) / Spec**
- 입력 Input: Week 4 Acceptance Criteria
- 처리 Processing: Acceptance Criteria를 작은 태스크로 나누고 의존 관계를 정리
- 출력 Output: 태스크 목록과 작업 순서
- 실패 조건 Failure: 독립적으로 확인할 수 없는 큰 태스크가 포함됨

**요청한 프롬프트 요지 / Prompt (summary)**
Week 4 Acceptance Criteria를 독립적으로 확인할 수 있는 작은 태스크로 나누고, 태스크의 의존 관계와 작업 순서를 작성해 달라고 요청함.

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because: 검색, 데이터 확인, 결과 표시, 정보 없음 처리를 각각 태스크로 나눈 부분을 사용함.
- 수정한 부분과 이유 / Changed, because: 우리 팀의 실제 프로젝트 범위에 맞게 태스크 내용을 간단하게 수정함.
- 폐기한 부분과 이유 / Rejected, because: 핵심 기능과 관계없는 추가 기능은 제외함.

**검증 방법 / How it was verified**
- 각 태스크가 따로 완료되었는지 확인할 수 있는지 확인하고, 태스크 간 선후 관계가 맞는지 확인함.
  
---

[이슈 # 10] 막힘 진단 활동지 작성
목표(스펙) / Spec
입력 Input: 먹지 저장소 코드(main), 10주차 활동지 양식, 팀이 정리한 참여자·버린 기능(#10, #18, #21)·GitHub 이슈 화면 캡처
처리 Processing: AI(Claude)에게 저장소 코드를 읽고 막힌 지점, 디버깅 요청 4요소, 검증하지 않은 것의 초안을 쓰도록 요청함
출력 Output: week-10.md 각 칸의 초안
실패 조건 Failure: 코드에 없는 문제를 지어냄 / 확인되지 않은 수치(닫힌 이슈 수 등)를 추측해서 넣음
요청한 프롬프트 요지 / Prompt (summary)
10주차 활동지를 채워 달라고 요청함. AI가 코드로 확인할 수 없는 칸(참여자, 닫힌 이슈 수, 버린 기능)은 비워 두고 질문하게 했고, 팀이 그 답과 이슈 화면 캡처를 제공함.
결과에 대한 판단 / Decisions
채택한 부분과 이유 / Accepted, because: 음료가 메뉴로 계산되는 문제를 디버깅 요청 4요소로 정리한 부분. restaurantFits와 시드 SQL(더치킨의 치킨 메뉴 + 콜라)에서 원인을 직접 확인할 수 있었다. 시드 메뉴가 모두 verified_count = 0이라는 "검증하지 않은 것" 항목도 SQL에서 확인되어 채택함.
수정한 부분과 이유 / Changed, because: 닫힌 이슈 수는 AI가 추측하지 않고 비워 두었고, 이슈 화면의 닫힌 시점을 보고 11월 4일 기준 5 / 21로 정함. 처음 초안에 있던 영어 병기는 팀 기록 언어에 맞게 삭제하고, 원래 양식의 제목은 그대로 유지함.
폐기한 부분과 이유 / Rejected, because: 초안의 영어 문장 전체. 활동지는 한국어로만 작성하기로 함.
검증 방법 / How it was verified
디버깅 요청의 재현 단계를 profile-filter.ts, 007_dishes_batch2.sql 코드와 대조함. 닫힌 이슈 목록을 GitHub 이슈 화면의 닫힌 시점과 대조함.

---

[이슈 # 11] 중간 데모와 상호 리뷰 활동지 작성
목표(스펙) / Spec
입력 Input: 시연 후 받은 피드백 메모, 새로 등록한 이슈 #45~#54 화면 캡처, 닫힌 이슈 화면 캡처
처리 Processing: AI에게 받은 피드백을 이슈 번호에 연결하고, 시연 결과와 성찰 초안을 쓰도록 요청함
출력 Output: week-11.md 각 칸의 초안
실패 조건 Failure: 피드백에 없는 내용을 이슈와 연결함 / 상대 팀에 남긴 리뷰를 지어냄
요청한 프롬프트 요지 / Prompt (summary)
시연에서 부족했던 점(예약, 프로필 선택지)과 받은 피드백(사장님용 버전, 데이터 확장, 예약, 즐겨찾기, 폰트·UI, 음식 취향 확장, 현재 위치)을 주고, 11주차 활동지에 맞게 정리해 달라고 요청함.
결과에 대한 판단 / Decisions
채택한 부분과 이유 / Accepted, because: 받은 피드백 → 이슈 번호 표. 피드백 7개가 S2~S4, S6, S7, S9, S10 이슈와 하나씩 대응되어 그대로 사용함.
수정한 부분과 이유 / Changed, because: 처음 성찰 초안은 "데이터 신뢰성"을 가장 약한 고리로 썼지만, 실제 시연에서 예약이 가장 부족했으므로 "예약"으로 바꾸고 데이터 신뢰성은 시연에서 보이지 않은 위험으로 남김.
폐기한 부분과 이유 / Rejected, because: 피드백에 근거가 없는 S1, S5, S8은 리뷰와 연결하지 않고 "함께 등록한 이슈"로 따로 적음. 상대 팀에 남긴 리뷰는 AI에게 맡기지 않고 팀이 직접 작성함.
검증 방법 / How it was verified
이슈 번호와 제목을 GitHub 이슈 화면과 대조하고, MVP 10 / 13을 닫힌 이슈 목록과 대조함. AI가 S2(#46)의 "길찾기"가 5주차 Won't 목록과 충돌한다고 지적해, 팀에서 범위를 다시 정하기로 함.

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
