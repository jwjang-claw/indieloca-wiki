# 📋 에이전트 태스크 티켓 [002]

## 🎯 목표
로컬 Quartz 위키를 GitHub Pages에 배포하여 사용자가 온라인에서 전략 문서를 볼 수 있도록 함.

## 🚩 관련 마일스톤 (체크포인트)
*   **대상:** [[01_Strategy/Milestones]]
*   **정렬:** MS-2 (마케팅 자산) - 문서화 가시성.

## 📂 컨텍스트 & 리소스 (필수 읽기)
*   **로컬 프로젝트:** `/home/jwjang/.openclaw/workspace/indieloca-wiki`
*   **GitHub 사용자:** `snailblu` (사용자의 GitHub 핸들).
*   **도구:** `gh` CLI (GitHub CLI), `npx quartz sync`.

## 🛠️ 요구사항 (단계별)
1.  **저장소 생성:**
    *   `indieloca-wiki`로 이동.
    *   git 초기화 (`git init`).
    *   `gh repo create --public --source=. --remote=origin`으로 `indieloca-wiki`라는 **공개** GitHub 저장소 생성.
2.  **설정:**
    *   `quartz.config.ts` 편집: `baseUrl`을 `"snailblu.github.io/indieloca-wiki"`로 설정.
3.  **배포 (동기화):**
    *   `npx quartz sync` 실행. (커밋, 푸시, GitHub Actions 트리거).
    *   *참고:* `sync`가 upstream 없음으로 실패하면 먼저 `git push -u origin v4` 수동 실행.
4.  **Pages 검증:**
    *   저장소에 GitHub Pages가 활성화되었는지 확인 (Quartz는 보통 Actions로 처리하지만, 빌드 후 `gh-pages` 브랜치 확인).

## ✅ 성공 기준 (완료 정의)
*   [ ] **저장소 존재:** `gh repo view snailblu/indieloca-wiki` 작동.
*   [ ] **코드 푸시됨:** `git log`에 커밋 표시.
*   [ ] **Action 트리거됨:** GitHub Actions 워크플로우 실행/대기 중.
*   [ ] **URL:** `https://snailblu.github.io/indieloca-wiki`가 목적지.

---
**에이전트 지침:**
1.  **컨텍스트 읽기:** 연결된 전략/기술 문서를 읽고 "IndieLoca" 이해.
2.  **실행:** 요구사항을 단계별로 따름.
3.  **보고:** 완료 후 **저장소 URL**과 **Pages URL** 보고.
