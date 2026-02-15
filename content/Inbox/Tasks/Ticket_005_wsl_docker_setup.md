# Ticket_005: WSL 개발 환경 설정 (Docker)

## 개요
- **목표:** WSL2 환경에서 `project-translation`의 Docker 개발 환경을 완벽하게 구축하여 에이전트가 통합 테스트를 수행할 수 있게 함.
- **현재 상태:**
  - 호스트(`jwjang`) 환경에서 `pip install` 실패 (Wheel 빌드 에러).
  - 그러나 `docker ps` 결과, 이미 `translation_backend` 등 **컨테이너가 정상 실행 중**임이 확인됨.
  - **전략 수정:** 호스트 Python 환경 대신, **이미 실행 중인 Docker 컨테이너 내부를 활용**하여 통합 테스트 수행.

## 필요 작업
1. **Docker 컨테이너 활용:** `sudo docker exec translation_backend ...` 명령으로 컨테이너 내부 쉘 접근.
2. **테스트 스크립트 실행:** 컨테이너 내부 환경(`uv` 또는 `pip`로 이미 구성됨)에서 `test_renpy_tag_safety.py` 실행.
3. **결과 확인:** 통합 테스트 성공 시 환경 구축 완료로 간주.

## 참고
- Ticket_004에서 시뮬레이션 테스트는 성공했으나, 실제 통합 테스트를 위해 이 환경 설정이 선행되어야 함.
