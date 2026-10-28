# 9주차 활동지 / Week 9 Worksheet

**개발 착수 / Starting development**

- 작성일 / Date: 2026-10-28
- 참여자 / Present: 아디브, 아이만, 아킬, 아마르

---

## ① 착수 점검표 / Start checklist

수업 시작 후 10분 안에 확인합니다. 체크되지 않은 항목이 있으면 그것부터 해결하고 개발을 시작합니다.
*Check within the first 10 minutes. Fix any unchecked item before you start coding.*

- [O] 초기 파일이 저장소에 올라와 있다 / Initial files are in the repository
- [O] 7주차 이슈가 GitHub에 등록되어 있고 담당자가 지정되어 있다 / Week 7 issues are on GitHub with owners
- [O] 팀원 모두 같은 환경에서 실행된다 / Everyone can run it in the same environment
- [O] 브랜치 규칙을 정했다 / Branch rule agreed — `main`에 직접 push하지 않는다. 이슈마다 `feature/T<번호>-<설명>` 브랜치를 만들고(예: `feature/T3-seed-data`), 팀원 1명이 리뷰한 뒤 Pull Request로 병합한다. / No direct pushes to `main`; one branch per issue (`feature/T<number>-<short-name>`), merged by pull request after one teammate's review.

---

## ② 개발 일정 / Development schedule

5주차 **작업 순서**와 7주차 **이슈·마일스톤**을 바탕으로 이슈를 주차별로 배치합니다.
*Place your issues week by week, based on the Week 5 work order and Week 7 issues and milestones.*

- 11주차에 MVP가 동작해야 합니다. / Fill backwards from the deadlines.
- 선행 이슈가 끝나기 전에 뒤 이슈를 넣지 않습니다. / Never schedule an issue before the ones it depends on.
- 한 사람에게 몰리지 않았는지 확인합니다. / Check that no one is overloaded.

태스크 번호는 5주차와 같습니다 (T1 = GitHub 이슈 #1). 같은 주 안에서는 화살표(→) 순서대로 진행합니다.
*Task numbers match Week 5 (T1 = GitHub issue #1). Within a week, work in arrow (→) order.*

| 주차 Week | 수업 일정 Class | 끝낼 이슈 # Issues to close | 담당 Owner |
|---|---|---|---|
| 9주차 | 개발 착수 | T3 → T1, T2, T19 | T3: Adib, T1: Irfan, T2: Aqil, T19: Nik Ammar |
| 10주차 | 막힘 진단 · 데모 시나리오 초안 | T4, T5, T13, T15 | T4: Nik Ammar, T5: Adib, T13: Aqil, T15: Irfan |
| 11주차 | **MVP 완료 · 중간 데모** | T6 → T7, T8, T14, T9, T16 | T6·T7: Irfan, T8·T14: Adib, T9·T16: Aqil |
| 12주차 | 핵심 경로 테스트 | T10, T11, T17, T18 | T10: Nik Ammar, T11: Irfan, T17: Aqil, T18: Adib |
| 13주차 | **배포 · 실행 패키지** | T20, T12 (T21은 시간이 허락하면) | T20: Adib, T12: Irfan, T21: Irfan |

> 일정이 맞지 않으면 기능을 줄입니다. 뺀 기능은 5주차 Won't 목록에 옮깁니다.
> *If it doesn't fit, cut scope and move the feature to your Week 5 Won't list.*

---

## ③ 이번 주 목표 이슈 / This week's issues

| # | 제목 Title | 담당 Owner | 완료 Done |
|---|---|---|---|
| T3 | 식당 15곳 시드 데이터 | Adib | [ ] |
| T1 | 지도 화면(첫 화면) | Irfan | [ ] |
| T2 | 하단 탭 5개 | Aqil | [ ] |
| T19 | 카카오·구글 로그인 | Nik Ammar | [ ] |

---

## ④ 오늘 진행한 것 / Progress today

- [ ] 이번 활동에서 AI를 사용했다면 `PROMPTS.md`에 기록했습니다 / Logged any AI use in `PROMPTS.md`

---

> 수업 종료 시 커밋하세요 / Commit this at the end of class
> `git add docs/week-09.md && git commit -m "docs: 9주차 활동지 작성"`
