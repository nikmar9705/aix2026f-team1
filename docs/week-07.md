# Week 07 팀 활동지 / Team Activity

| 팀명 / Team | 작성일 / Date | 참여자 / Members |
| --- | --- | --- |
| 1팀, 프로젝트: 먹지? | 2026-10-15 | 아이만, 아디브, 아킬, 아마르 |

## 1. 아키텍처 다이어그램 / Architecture Diagram

<img width="872" height="1280" alt="photo_2026-10-21_07-19-00" src="https://github.com/user-attachments/assets/86028af4-baf0-4d99-95ec-2b7962b3edb1" />
<img width="884" height="1280" alt="photo_2026-10-21_07-19-02" src="https://github.com/user-attachments/assets/674c8822-8eae-48ae-9d73-99d008c684ef" />
<img width="854" height="1280" alt="photo_2026-10-21_07-19-03" src="https://github.com/user-attachments/assets/ae003bd4-6a6f-434f-a52b-34fb581989a1" />


Screen sketches: see the clickable prototype at https://claude.ai/artifact/Ruqs4wU1dTWFEm2D2ojS4z.

| 계층 / Layer | 모듈·파일 / Modules | 의존하지 않는 것 / Does not depend on |
| --- | --- | --- |
| 표현 / Presentation | MapScreen(앱 실행 시 첫 화면), SearchScreen, HomeScreen, ReserveScreen, FavouritesScreen, ProfileScreen, RestaurantDetailScreen, ContactSheet, KoreanOrderCard, MenuScanScreen, FoodProfileScreen, 한국어/영어 언어 문자열 | 데이터베이스·저장소 코드, AI 서비스, 필터링·순위 규칙 |
| 로직 / Logic | ProfileFilter(음식 프로필에 맞는 식당과 메뉴 판별), BadgeService("무엇을 제공하는지"로부터 "누구에게 맞는지" 도출), ConfidenceScorer(안전 / 먼저 확인 / 피하기 및 신뢰도), NearbyService(거리 필터), SuggestionRanker, AIExplainer와 MenuScannerService(AI 호출), OrderPhraseBuilder | 화면, UI 컴포넌트, UI 프레임워크 |
| 데이터 / Data | RestaurantRepository, DishRepository, ReviewRepository, UserProfileStore, FavouritesStore, SearchHistoryStore, 시드 데이터 파일 | 로직과 표현 계층(데이터를 저장하고 반환하기만 함) |

규칙: 화면은 데이터베이스를 직접 읽지 않습니다. 화면은 로직 모듈에 요청하고, 로직은 Repository에 요청합니다. 이렇게 하면 화면을 건드리지 않고도 데이터베이스나 AI 제공자를 바꿀 수 있습니다.

모듈 이름은 제안 단계이므로, 실제 파일명에 맞게 변경하세요.
### 기술 스택 / Tech Stack (decided)

앱은 모바일 앱이며 하나의 코드베이스로 iOS와 Android에서 모두 실행됩니다. 팀원 중 모바일 프레임워크를 사용해 본 사람이 없기 때문에, 튜토리얼이 가장 많고 휴대폰 테스트가 가장 쉬운 것을 선택했습니다.

| 계층 / Layer | 선택 / Choice | 이유 / Why |
| --- | --- | --- |
| 표현 / Presentation | React Native with Expo (JavaScript or TypeScript) | iOS와 Android를 하나의 코드베이스로 개발; Expo Go로 본인 휴대폰에서 테스트 가능하며 iPhone 테스트에 Mac이 필요 없음 |
| 지도 / Map | `react-native-maps` | Expo Go 안에서 동작; 핀은 자체 데이터에서 가져옴 |
| 카메라 / Camera | `expo-image-picker` | 	메뉴 스캐너용 메뉴 사진 촬영(#10) |
| 로직 / Logic | JavaScript modules in the app, plus an optional Python FastAPI service | 필터링은 앱 안에서 실행 가능; AI 호출은 서버를 거치므로 API 키가 앱에 포함되지 않음 |
| 데이터 / Data | Supabase (PostgreSQL) | 실제 SQL, 호스팅 API, 로그인(카카오, 구글), 위치 기반 쿼리 지원 |
| 데이터 수집 / Data seeding | Kakao Local API | ERICA 주변 실제 식당 이름, 전화번호, 좌표(#3) |

수업에는 필요 없는 것: App Store 또는 Play Store 출시. 데모는 Expo Go로 실행합니다.

## 2. 이슈 / Issues

각 이슈는 확인 가능한 테스트가 있는 하나의 작업 단위입니다. 각 이슈에는 완료까지 책임지는 담당자가 한 명씩 있으며, 누구나 돕거나 리뷰할 수 있습니다. 커밋 메시지에 이슈 번호를 넣고(예: "#4 add food profile filter"), 완료 조건을 충족하면 이슈를 닫으세요.

| # | 제목 / Title | 라벨 / Label | 담당 / Assignee | 완료 조건 / Acceptance Criteria |
| --- | --- | --- | --- | --- |
| #1 | 지도 화면(첫 화면) | frontend | Irfan | 앱이 2D 지도로 열리고 시드된 모든 식당에 핀이 표시됨; 각 핀에 음식 종류 로고와 배지 개수가 표시되며, 핀을 탭하면 식당 미리보기가 나타남 |
| #2 | 하단 탭 5개 | frontend | Aqil | 	홈, 예약, 검색 지도(가운데), 즐겨찾기, 프로필 탭으로 화면이 전환되고, 앱은 검색 지도 화면으로 열림 |
| #3 | 식당 15곳 시드 데이터 | data | Adib | 	ERICA 주변 식당 최소 15곳, 각각 위치, 전화번호, 배지, 원산지, 메뉴 2개 이상 포함 |
| #4 | 음식 프로필과 필터 | backend | Nik Ammar | 할랄, 비건 또는 돼지고기 제외를 선택하면 조건에 맞는 핀은 강조 표시되고 나머지는 흐리게 표시되며, 홈 목록에는 조건에 맞는 곳만 표시됨 |
| #5 | 식당 상세 페이지 | frontend | Adib | 	식당을 탭하면 배지, 안전 / 먼저 확인 / 피하기로 표시된 메뉴와 빼야 할 재료, 원산지, 그리고 하단에 "예약하기" 버튼이 표시됨 |
| #6 | 주변 홈 추천 | backend | Irfan | 홈에 프로필의 거리 설정 내 식당이 프로필 기준으로 필터링되어 표시되고, 음식 종류를 탭하면 목록이 필터링됨 |
| #7 | 추천 순위 | backend | Irfan | 추천 수가 많고 리뷰가 좋은 식당이 그렇지 않은 식당보다 위에 표시됨 |
| #8 | 신뢰 배지와 AI 신뢰도 | backend | Adib | 	식당에 신뢰도와 그 출처가 표시되고, 메뉴에는 검증 횟수와 마지막 확인 날짜 또는 "미검증"이 표시됨 |
| #9 | 한국어 주문 카드 | frontend | Aqil | 메뉴를 탭하면 한국어 문장과 영어 뜻이 전체 화면으로 표시되며, 최소 3개의 문구를 사용할 수 있음 |
| #10 | AI 설명과 메뉴 스캐너 | ai | Nik Ammar | 먼저 확인 메뉴에서 "왜?"를 누르면 이유와 출처가 표시됨; 테스트용 메뉴 사진을 넣으면 메뉴가 안전 / 먼저 확인 / 피하기로 분류되어 반환되고, 먼저 확인 메뉴마다 한국어 질문이 함께 제공됨 |
| #11 | 사용자 설문 | docs | Irfan | 최소 8명(한국인 및 외국인)이 응답하고, 핵심 수치 1개와 인용 2개가 저장소에 저장됨 |
| #12 | 	데모 스크립트와 발표 | docs | Irfan | 3분 분량의 스크립트가 있고, 팀이 데모를 처음부터 끝까지 한 번 실행해 봄 |
| #13 | 이중 언어 UI(한국어·영어) | frontend | Aqil | 언어 설정을 변경하면 앱을 재시작하지 않고 모든 화면 텍스트가 한국어와 영어 사이에서 전환됨 |
| #14 | 알레르기 필터 | backend | Adib | "땅콩"을 선택하면 땅콩 태그가 붙은 모든 메뉴가 제외되고, 땅콩이 없는 메뉴가 하나도 없는 식당은 흐리게 표시됨 |
| #15 | 검색 페이지 | frontend | Irfan | 	지도 검색창을 탭하면 사용자 선호에 맞는 인기 검색어와 최근 검색어 5개가 표시됨 |
| #16 | 예약 탭과 연락처 카드 | frontend | Aqil | 	예약 탭에 식당 목록이 표시됨; "예약하기"를 누르면 전화번호와 전화·복사 버튼, 인원수 안내, 한국어 문장이 표시됨; 기록 탭에 연락한 곳이 표시됨 |
| #17 | 개인 메모가 있는 즐겨찾기 | frontend | Aqil | 	식당에 별표를 누르면 개인 메모와 함께 즐겨찾기에 추가됨; 목록이 비어 있으면 빈 상태 화면이 표시됨 |
| #18 | 프로필, QR, 친구 | frontend | Adib | 	프로필에 사진, 이름, QR 코드, 설정, 친구 수, 초대 링크가 표시됨; 친구의 QR을 스캔하면 친구로 추가됨 |
| #19 | 	카카오·구글 로그인 | backend | Nik Ammar | 사용자가 카카오 또는 구글로 로그인할 수 있고, 앱을 재시작해도 로그인 상태가 유지됨 |
| #20 | 사진 포함 공개 리뷰 | backend | Adib | 	로그인한 사용자가 별점, 텍스트, 사진, 식단 태그를 게시하면 다른 사용자가 식당 페이지에서 볼 수 있음 |
| #21 | 	같이 먹기(스트레치) | backend | Irfan | 서로 다른 프로필을 가진 친구 두 명을 선택하면 모두에게 맞는 식당만 남음 |

## 3. 마일스톤 / Milestones

| 마일스톤 / Milestone | 마감 / Due | 포함 이슈 / Issues |
| --- | --- | --- |
| MVP 완료 | 11주차 / Week 11 | #1 to #9, #13 to #16 |
| 실행 패키지 | 13주차 / Week 13 | #10, #11, #12, #17 to #20 and any unfinished MVP issue |
| Stretch | If time allows | #21 |

AI를 사용했다면 PROMPTS.md에 기록 / Log any AI use in PROMPTS.md
