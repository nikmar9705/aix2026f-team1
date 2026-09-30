# 5주차 활동지 / Week 5 Worksheet

**1-page 기획서 / One-page plan**

- 작성일 / Date: 30일 9월
- 참여자 / Present: 아디브, 아이만, 아킬, 아마르

---

## ① 주제 확정 / Confirm topic

- 확정 주제 / Topic: Halal Hunter (HxH)
- 이유 / Reason: 안산에 거주하는 무슬림 학생들은 어떤 음식과 장소가 할랄(halal)인지 확인하는 데 어려움을 겪고 있습니다. 공식 인증을 받은 할랄 관련 정보가 거의 없기 때문에, 학생들은 주로 다른 학생들의 입소문이나 개인적인 경험에 의존하는데, 이는 종종 신뢰하기 어렵습니다. 

---

## ② 태스크 분해와 의존 관계 / Tasks and dependencies

앱 구조: **3개 화면**
1. 지도 화면 — 하단 탭: Restaurants / Nearby
2. 식당 상세 화면
3. Settings 화면

| # | 태스크 Task | 완료 조건 Done when | 선행 태스크 Depends on | 담당 Owner |
|---|---|---|---|---|
| T1 | [Must] 데이터 형식 정하기 | 식당명, 주소, 좌표, 추천 수와 음식명, 재료, 상태(✅⚠️❌), 주문 조건의 필드가 정해진다. | 없음 | 아킬 |
| T2 | [Must] 식당 데이터 수집하기 | ERICA 주변 식당 10개 이상과 각 식당의 음식 1개 이상이 데이터 형식에 맞게 정리된다. | T1 | 아디브 |
| T3 | [Must] Google Sheet 데이터 불러오기 | 시스템이 Google Sheet에서 식당 데이터 1개 이상을 가져온다. | T1 | 아이만 |
| T4 | [Must] 지도 화면 만들기 | 지도에 더미 좌표를 사용한 식당 핀이 표시된다. | 없음 | 아마르 |
| T5 | [Must] 실제 식당을 지도에 표시하기 | Google Sheet의 식당이 실제 데이터로 지도에 핀으로 표시된다. | T3, T4 | 아마르 |
| T6 | [Must] 식당 상세 화면 만들기 | 식당을 선택하면 음식 상태(✅⚠️❌), 주문 조건, 공식 할랄 인증이 아니라는 안내가 표시된다. | T2, T5 | 아디브 |
| T7 | [Must] 정보 없음 안내 만들기 | 음식 정보가 없으면 "정보를 확인할 수 없습니다"가 표시된다. | T6 | 아디브 |
| T8 | [Should] Restaurants 탭 만들기 | 식당이 추천 수가 높은 순서로 표시되고, 선택하면 상세 화면으로 이동한다. | T3, T6 | 아마르 |
| T9 | [Should] 현재 위치 확인하기 | 위치 권한을 허용하면 현재 좌표를 가져오고, 거부하면 안내 문구를 표시한다. | 없음 | 아킬 |
| T10 | [Should] Nearby 탭 만들기 | 현재 위치에서 100m 이내의 식당만 표시되고, 없으면 "100m 이내에 식당이 없습니다"를 표시한다. | T3, T9 | 아킬 |
| T11 | [Should] 가상 위치 기능 만들기 | 목록에서 위치를 선택하면 현재 위치로 사용되고 Nearby 목록이 변경된다. | T10 | 아이만 |
| T12 | [Should] Settings 화면 만들기 | Strict / Flexible을 변경하면 ⚠️ 음식의 표시 여부가 변경된다. | T6 | 아이만 |

### 의존 관계 그래프 / Dependency graph (DAG)

```mermaid
graph LR
  T1["T1 데이터 형식 정하기"] --> T2["T2 식당 데이터 수집"]
  T1 --> T3["T3 Google Sheet 데이터 불러오기"]

  T4["T4 지도 화면 만들기"] --> T5["T5 실제 식당 지도 표시"]
  T3 --> T5

  T2 --> T6["T6 식당 상세 화면"]
  T5 --> T6
  T6 --> T7["T7 정보 없음 안내"]

  T3 --> T8["T8 Restaurants 탭"]
  T6 --> T8

  T9["T9 현재 위치 확인"] --> T10["T10 Nearby 탭"]
  T3 --> T10
  T10 --> T11["T11 가상 위치"]

  T6 --> T12["T12 Settings 화면"]
```
---

## ③ 범위 결정 / Scope

### Must — 없으면 성립 안 됨 / essential

핵심 시나리오 1개가 끝까지 동작하는 데 필요한 것만 / *Only what the core scenario needs to work end-to-end*

- 핵심 시나리오 / Core scenario: 



### Should (없을 경우에는 작성하지 마세요)
 
 - 시나리오



### Could (없을 경우에는 작성하지 마세요)

 - 시나리오

### **Won't — 이번 학기에 안 함 / not this semester**

| Won't 항목 Item | 포기한 이유 Why |
|---|---|
|  |  |
|  |  |

### 실행 가능성 확인 / Feasibility check

- 특수 장비·유료 API·실제 개인정보가 필요한가? 필요하다면 대안은?
  *Does it need special hardware, paid APIs or real personal data? If so, what is the alternative?*
- 15주차에 발표장에서 시연할 수 있는 형태인가?
  *Can it be demonstrated live in Week 15?*

---

## ④ 가장 먼저 동작시킬 흐름 (Walking Skeleton) / First end-to-end flow

예 / Example: 과제 ID를 입력하면 → LMS에서 제출 기록을 받아 와서 → 화면에 제출 인원 숫자 하나가 뜬다

> [무엇을 입력하면] → [무엇을 처리해서] → [화면에 무엇이 나온다]
>
앱에서 '내 주변' 탭을 누르면 → 확인된 할랄 식당 중 현재 위치에서 1km 안에 있는 곳을 골라 거리순으로 정렬해서 → 화면에 식당 이름과 거리(m)가 뜬다

---

> 수업 종료 시 커밋하세요 / Commit this at the end of class
> `git add docs/week-05.md && git commit -m "docs: 5주차 활동지 작성"`
