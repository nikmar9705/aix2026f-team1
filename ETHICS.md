# 윤리 결정 기록 / Ethics Decisions

> 이 파일은 **개발 중에 내린 윤리적 판단을 남기는 곳**입니다.
> 학기말에 몰아 쓰지 말고, 결정할 때마다 한 줄씩 추가하십시오.
>
> *Record ethical judgements as you make them — not at the end of the semester.*

---

## 1. 이해관계자 / Stakeholders

> 3주차 문제 발굴 시 작성 · 이후 변경되면 갱신

| 구분 | 누구인가 / Who | 어떤 영향을 받는가 / How affected |
|---|---|---|
| 직접 사용자 Direct |  |  |
| 간접 영향자 Indirect |  |  |
| 데이터가 다뤄지는 사람 Data subjects |  |  |
| 서비스에서 배제될 수 있는 사람 Excluded |  |  |

**당사자에게 확인했는가 / Did you ask them?**

- [ ] 직접 사용자를 인터뷰했다
- [ ] 배제될 수 있는 사람의 관점을 확인했다 → 못 했다면 그 이유:

---

## 2. 데이터 / Data

> 7주차 설계 시 작성

| 저장하는 항목 | 왜 필요한가 (기능명) | 없으면 안 되는가 |
|---|---|---|
|  |  |  |

**저장하지 않기로 한 것 / What we decided NOT to store**

- 항목:
- 이유:

> 수집하지 않은 데이터는 유출될 수 없습니다.
> *Data you never collect cannot leak.*

---

## 3. 포기한 것과 영향 / What we dropped

> 5주차 범위 결정 시 작성

| Won't 항목 | 영향받는 사람 | 그래도 포기한 이유 | 나중에 추가 계획 |
|---|---|---|---|
|  |  |  |  |

> **특정 집단이 아예 못 쓰게 되는 항목**과 단순히 편의 기능인 항목을 구분하십시오.
> *Distinguish items that exclude a group from items that are merely conveniences.*

---

## 4. 검증하지 않은 것 / Not verified

> 10주차 이후, 알게 될 때마다 추가

| 무엇을 검증하지 않았나 | 왜 | 어떤 위험이 남는가 |
|---|---|---|
|  |  |  |

---

## 5. 알고도 고치지 못한 것 / Known and unfixed

> 12~14주차

| 문제 | 언제 알았나 | 왜 못 고쳤나 | 사용자에게 알릴 것인가 |
|---|---|---|---|
| A GPS check-in can be faked, so a "Location check-in" review may come from someone who never ate there. | Week 12 (Aqil's review of PR #74) | Phone GPS can't be verified by any app. A QR code at the counter needs the owner portal to show it. | Yes: those reviews say "📍 Location check-in"; only completed bookings say "✓ Verified visit". |
| Dish tags are our reading of the menu and common recipes, not the restaurant's own declaration. "May contain" is a best guess. | Week 11 (dish library) | We can't get ingredient lists from 3,000 restaurants, and owners rarely publish them. | Yes: unsure dishes show "Ask first" with a Korean question for the staff, and the app says it is not halal certification. |
| Menus come from photos and can go out of date (prices, new dishes). | Week 11 | No feed from restaurants; updates depend on people sending photos (#82). | Yes: each dish shows how many people checked it and when. |
| The shared test owner account (owner-test@example.com) owns a real restaurant (TriBoTea), so testers can see its booking requests. | Week 12 (security review) | Needed so the team can test the owner portal. | No real users affected yet. Delete it before real customers book TriBoTea. |
| `021_featured_dish_media.sql` (#69) is not applied in Supabase, so no featured dish photo shows. | Week 12 (pre-deploy check) | Waiting for the owner of #69 to run it. | No: nothing breaks, the photo is just missing. |
| The AI "Why?" explanation (#85) has not run against the real API, and has no call limit yet. | Week 12 (review of PR #85) | No API key yet. | The sheet labels the AI part and always shows the data reasons. A call limit goes in before the key. |

> **감추지 않는 것이 평가 대상입니다.** 여기 적힌 내용은 감점 사유가 아닙니다.
> *Being honest here is what is graded. Entries here do not lose you points.*
---

## 6. 결정 로그 / Decision log

> 위 분류에 안 맞는 판단은 여기에 한 줄씩

| 날짜 | 상황 | 우리의 결정 | 근거 |
|---|---|---|---|
|  |  |  |  |
