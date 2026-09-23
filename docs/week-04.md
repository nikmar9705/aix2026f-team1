# 4주차 활동지 / Week 4 Worksheet

**주제 선택과 요구 명세 / Choosing a problem & writing the spec**

- 작성일 / Date: 9월 23일
- 참여자 / Present: 아킬, 아디브, 아마르, 아이만

---

## ① 주제 선택 / Choosing one problem

| 항목 Item | 내용 |
|---|---|
| 선택한 주제 Chosen | 후보 B |
| 선택 근거 Why | 저희는 무슬림으로서 이 문제에 대해 깊이 공감할 수 있습니다. 또한 이 문제는 저희 커뮤니티에 직접적인 도움을 줄 수 있으며, 일상적인 음식 섭취에 대한 불확실성과 고민을 줄이는 데 도움이 될 수 있습니다. |

## ② 성공 기준 가져오기 / Success criteria from Week 3

| 3주차 성공 기준 원문 Original (Week 3) | 모호한 표현 Vague words |
|---|---|
| *(예시) 학생들이 과제 제출 현황을 쉽게 확인할 수 있다* | *쉽게, 확인할 수 있다* |
| 식당이나 제품의 할랄 가능성에 대한 학생 후기/스크리닝 정보를 2~3분 이내에 찾을 수 있고, 매번 따로 물어보거나 검색할 필요가 없음 | 할랄 가능성, 스크리닝 정보, 찾을 수 있고, 따로 물어보거나 검색할 필요가 없음 |

## ③ Acceptance Criteria

최소 정상 경로 2개 + 실패 경로 1개. **판정 방법** 칸이 비면 아직 명세가 아닙니다.
At least two normal paths + one failure path. If "How to check" is empty, it is not yet a spec.

| # | 경로 Path | EARS 문장 Sentence | 판정 방법 How to check |
|---|---|---|---|
| *예시* | *정상* | *WHEN 학생이 과제 목록을 열면 THE 시스템은 SHALL 과목별 미제출 과제를 마감일 순으로 표시한다* | *미제출 과제 3건을 만든 뒤 목록을 열어 마감일 순으로 나오는지 확인* |
| AC-1 | 정상 Normal | WHEN 유학생이 식당을 검색하면 THE 시스템은 SHALL 해당 식당의 할랄/적합한 음식 정보를 3초 이내에 표시한다. | 식당을 검색하고 시간을 측정한다. 3초 이내에 할랄/적합한 음식 정보가 표시되는지 확인한다. |
| AC-2 | 정상 Normal | WHEN 사용자가 음식 항목을 선택하면, THE 시스템은 SHALL 해당 음식의 재료 및 할랄/섭취 가능 여부 정보를 표시한다.| 음식 항목을 선택하고 재료 및 할랄/적합한 음식 정보가 표시되는지 확인한다. |
| AC-3 | 실패 Failure | IF 시스템이 식당 또는 음식 항목에 대한 충분한 할랄/섭취 가능 여부 정보를 찾을 수 없으면, THEN THE 시스템은 SHALL "정보를 확인할 수 없습니다"라는 안내 문구를 표시한다. | 정보가 충분하지 않은 식당/음식 항목을 검색하고 "정보를 확인할 수 없습니다"라는 안내 문구가 표시되는지 확인한다. |

> 확인할 동작이 더 있으면 AC-4부터 행을 추가해 쓰십시오.
> If there are more behaviors to check, add rows from AC-4.

- [O] 이번 활동에서 AI를 사용했다면 `PROMPTS.md`에 기록했습니다 / Logged any AI use in `PROMPTS.md`

---

> 수업 종료 시 커밋하세요 / Commit this at the end of class
> `git add docs/week-04.md && git commit -m "docs: 4주차 활동지 작성"`
