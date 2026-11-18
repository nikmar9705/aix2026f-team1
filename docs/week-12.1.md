[week-12.md](https://github.com/user-attachments/files/34976786/week-12.md)
# 12주차 활동지 / Week 12 Worksheet

**테스트와 검증 / Testing and verification**

- 작성일 / Date: 2026-11-17
- 참여자 / Present: 아디브, 아이만, 아킬, 아마르
- 결과보고서 / Report chapter: 7장 검증과 실행 환경
- 최종 반영 / Feeds into: tests/ 폴더 → in the code repo `Irfnaimann02/mukji` the tests live in `mobile/src/logic/__tests__/` (where Jest/Expo expects them), run with `npm test` inside `mobile/`

---

## 테스트 3개 / Three tests (대상 / 검증 내용)

All three test `mobile/src/logic/profile-filter.ts`, the core path of the app: deciding whether a person can eat a dish. File: `mobile/src/logic/__tests__/profile-filter.test.ts` (PR #72).

1. **Dish status (halal)** / `dishStatus()`: for a halal user, pork → Avoid; chicken at a place without halal certification → Ask first; the same chicken at a certified place → Safe; plain rice → Safe; "may contain pork" → Ask first (not proof, so not Avoid).
2. **Allergy filter (#14)** / `splitMenu()`, `fadedForAllergies()`: a peanut allergy hides every dish tagged peanut, even "may contain" (팟타이 hidden, 쌀국수 shown, reason "peanut"); a restaurant whose every dish has peanut fades on the map, one with something left does not.
3. **No menu never fits** / `restaurantFits()`: a restaurant with no dishes (our own or a Kakao place) never counts as fitting, for halal, vegan, peanut-allergy and no-restriction profiles; it is not faded either ("unknown" is not "checked and bad").

## 일부러 망가뜨렸을 때 실패했는가 / Did it fail when you broke the code?

- [x] Yes
- [ ] No → 이유 Why:

We removed each rule from `profile-filter.ts` one at a time and ran `npm test`:

| Rule broken | Tests that failed |
|---|---|
| Halal: non-certified meat is no longer "Ask first" | Test 1, and the lamb test "halal: lamb is ask first unless certified" (both guard the same rule) |
| Allergy: "may contain" ignored | Test 2 |
| No menu counts as fitting | Test 3 |

With the real code all pass. The whole suite is now 18 tests (3 core, 2 lamb, 3 reviews / menu photo, 2 error messages, plus the team's booking tests).

## AI 코드리뷰 관점 2개 / 주요 지적 / Two review perspectives

Reviewed with Claude, on the code written this sprint (#52 reviews after eating, #77 test owner, #82 menu photo, #83 lamb). Full write-up: `docs/week12-quality-check.md` in the code repo.

**Perspective 1: security (who can do what)**
- The 20-photos-a-day limit counted database rows only; the photo is uploaded before the row, so a user at the limit could keep uploading files. → Fixed (`025`): the upload itself is limited.
- Users could edit `photo_paths` on their reviews with no feature behind it. → Fixed (`025`).
- GPS check-in can be faked. → Not fixable; labelled honestly (see Known and unfixed).

**Perspective 2: code quality (is it easy to change safely)**
- `Alert.alert` does nothing on the web: 43 popups were silent and the Directions button did nothing. → Fixed: `showAlert()`.
- The ingredient list was copied in 4 places; adding lamb meant 4 edits. → Fixed: one list, TypeScript refuses a missing one.
- Review and menu-photo logic had no tests. → Fixed: 3 tests.

**What one perspective caught that the other missed:** only security found the storage limit; only code quality found the silent web alerts. Neither found the menu-photo button hidden behind the booking bar or the "not signed in" bug: those were found by clicking through the app, which is why we added a Playwright smoke test (5 flows, `npm run e2e`).

## 받아들이지 않은 지적과 이유 / Findings rejected and why

| Finding (from the AI review / Supabase advisor) | Why we did not change it |
|---|---|
| 4 `SECURITY DEFINER` functions can be called by signed-in users (`add_review`, `check_in`, `review_eligibility`, `my_menu_photos_today`) | On purpose: they are the only way in. Each checks the login, the visit and the limits itself; the tables block direct writes. Switching them to `SECURITY INVOKER` would mean opening the tables instead. |
| "Anonymous access" policies on 9 tables | Guests are signed-in anonymous users. Every policy checks ownership (`user_id = auth.uid()`), so a guest only reaches its own rows. Reviews and check-ins also refuse guests. |
| "Use a stronger proof of visit than GPS" (Aqil, PR #74) | Phone GPS can't be trusted by any app. Instead of blocking the feature: check-ins are labelled "Location check-in", only a completed reservation gets "Verified visit". QR at the counter is the follow-up idea. |
| "Allow deleting a review and posting again bypasses one-per-person" | Not a bypass: you still have at most one review, and posting again needs a valid visit. Same as editing, which is allowed. People should be able to delete their own words. |

## 배포 전 점검 / Pre-deployment check

- [x] 키 노출 없음 No keys: only `mobile/.env.example` is in the repo; the Kakao key and the AI key stay in Supabase Edge Function secrets.
- [x] 입력 검증 Input validation: the database refuses bad input: stars 1 to 5, review ≤ 1000 characters, menu-photo note ≤ 300, Kakao place ID digits only, known diet tags only, photos ≤ 5 MB and image types only, 20 photos a day.
- [x] 오류 노출 없음 No error leakage: 11 places showed the raw server error to users (e.g. "new row violates row-level security policy for table reservations"). Now `userMessage()` shows a short translated message and keeps the details in the console. 2 tests check it (they fail if a raw error leaks again).
- [x] 외부 장애 대응 External failure: if Kakao fails, the app falls back to our own restaurants; Home shows "Couldn't load restaurants" with Try again; review and menu-photo errors show a clear reason; the AI "Why?" sheet (#85) still shows the data-based reasons when the AI is off.

Also run: `npx tsc --noEmit` (0 errors), `npx expo lint` (0 problems), `npm test` (18 passed), `npm run e2e` (5 passed against the real data), every SQL file applied in Supabase (except `021_featured_dish_media.sql`, see below).

---

> 수업 종료 시 커밋하세요 / Commit this at the end of class
> `git add docs/week-12.md && git commit -m "docs: 12주차 활동지 작성"`

## 알고도 고치지 못한 것 / Known and unfixed  → `ETHICS.md` 5절

| 문제 | 왜 못 고쳤나 | 사용자에게 알릴 것인가 |
|---|---|---|
| A GPS check-in can be faked, so a "Location check-in" review may come from someone who never ate there. | Phone GPS can't be verified by any app. A QR code at the counter needs the owner portal to show it. | Yes: those reviews say "Location check-in", only completed bookings say "Verified visit". |
| Dish tags are our reading of the menu and common recipes, not the restaurant's own declaration. "May contain" is a best guess. | We can't get ingredient lists from 3,000 restaurants; owners rarely publish them. | Yes: every dish shows "Ask first" when unsure, and the app says it is not halal certification. |
| Menus come from photos and can go out of date (prices, new dishes). | No feed from restaurants; updates depend on people sending photos. | Yes: each dish shows how many people checked it and when. |
| The shared test owner account (owner-test@example.com) owns a real restaurant (TriBoTea), so testers can see its booking requests. | Needed so the team can test the owner portal. | No users affected yet; delete it before real customers book TriBoTea. |
| `021_featured_dish_media.sql` (#69) is not applied in Supabase, so no featured dish photo shows. | Waiting for the owner of #69 to run it. | No: nothing breaks, the photo is just missing. |
| The AI "Why?" explanation (#85) has not run against the real API yet, and has no call limit. | No API key yet. | Before the key is set: add a limit; the sheet already labels the AI part and keeps the data reasons. |
