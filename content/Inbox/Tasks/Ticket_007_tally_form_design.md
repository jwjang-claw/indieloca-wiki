# 📋 에이전트 태스크 티켓 [007]

## 🎯 목표
**MS-2: "낚시" 자산**의 핵심인 **Tally 설문지(Waitlist Form)**를 설계합니다.
단순한 "대기 등록" 폼이 아니라, **글로벌 게임 번역가(주 타겟)**의 **고통점(Pain Points)**을 자극하여 "이 도구가 내 문제를 해결해 주겠구나"라는 기대감을 심어주는 것이 목표입니다.

## 🚩 관련 마일스톤
*   **대상:** [[01_Strategy/Milestones#MS-2|MS-2: "낚시" 자산]]
*   **성공 기준:**
    - [ ] **Tally 폼:** 타겟팅된 질문(고통점)이 포함된 라이브 URL (또는 초안)

## 📂 컨텍스트 & 리소스 (필수 확인 완료)
*   **전략:** [[01_Strategy/Master_Plan]] ✅ (타겟: 게임 번역가, 핵심: 태그/맥락/오버플로우)
*   **타겟 페르소나:** [[01_Strategy/Target_Persona]] ✅ (주 타겟: "Ren'Py 전문가")
*   **타겟 시장:** 글로벌 (영어권 번역가)
*   **핵심 고통점:**
    1.  **Tag Nightmare:** Game breaks due to `{player_name}` → hours of manual checking
    2.  **Context Loss:** Line-by-line translation in Excel → no idea who's speaking or the emotion
    3.  **Overflow Stress:** Client complains "text got cut off" after build delivery
*   **도구:** Tally.so (무료 계정 활용)

## 📅 업무 수행 계획

### Step 1: 질문 초안 작성 (에이전트 수행)
- [ ] 3개 고통점 질문 구체화 + 선택지 설계
- [ ] 폼 헤더/제목/설명 초안 작성
- [ ] CTA 문구 확정
- [ ] 결과물: `Ticket_007_questions_draft.md` 파일 생성

### Step 2: Tally.so 폼 생성 (사용자 수동)
- [ ] Tally.so 무료 계정 생성 (없는 경우)
- [ ] Step 1의 초안을 바탕으로 폼 생성
- [ ] 헤더 이미지 업로드 (IndieLoca 로고/일러스트)
- [ ] 테스트 제출 후 링크 공유

### Step 3: 검증 & 완료 (에이전트 수행)
- [ ] Tally 링크를 티켓에 업데이트
- [ ] HEARTBEAT.md에 완료 체크
- [ ] Ticket_008 (샘플 LQA 리포트) 진행 준비

---

## 🛠️ 요구사항 (단계별)
1.  **Hook Questions (3~4, English):**
    *   **Tag Pain:** "Has a Ren'Py tag like {player_name} or {color=#hex} ever broken your game build?"
    *   **Context Pain:** "Ever found yourself digging through scripts wondering 'Who's saying this line?' while translating in Excel?"
    *   **Overflow Pain:** "Has a client ever come back saying 'the text got cut off' after you delivered?"
    *   **Translator Language:** "LQA", "quality check", "tags", "delivery", "client", "revisions"
2.  **솔루션 암시:**
    *   질문이 IndieLoca의 기능(태그 안전, 캐릭터 톤, 오버플로우 감지)으로 자연스럽게 이어지도록 구성.
    *   "AI ruins nuance" 반론 대응: "We don't replace you. We handle the *tags* and *layout*. You polish the *soul*."
3.  **CTA (Call to Action):**
    *   "Join Beta" 대신 **"Stop being a code checker. Go back to being a translator."** (핵심 메시징)
    *   또는: "Cut your tag checking time to zero"
4.  **Tally 폼 구조:**
    *   헤더 이미지 / 제목 / 설명 / 질문 / 이메일 필드 / 감사 페이지 메시지.

## ✅ 성공 기준 (완료 정의)
*   [ ] **Questions Finalized:** 3 pain-point questions (tags/context/overflow) + choices, in English.
*   [ ] **Form Structure:** Title, description, question order organized in markdown.
*   [ ] **(Optional) Tally Link:** Create actual Tally form and share link.

---
**에이전트 지침:**
1. **필수 참조 문서:** Master_Plan.md, Milestones.md, Target_Persona.md (모두 확인 완료 ✅)
2. **타겟 일치 검증:** 주 타겟 = Global game translators ← 전략과 일치 ✅
3. Use translator terminology ("LQA", "tags", "delivery", "client", "revisions") in English.
4. Keep it short (under 1 minute to complete).
5. Questions should naturally lead to IndieLoca's core features.