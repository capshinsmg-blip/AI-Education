# CLAUDE.md

이 파일은 Claude Code가 이 프로젝트에서 작업할 때 참고하는 안내 문서입니다.

## 프로젝트

AI 교육 관련 작업물 저장소.

## Git 동기화 설정

- **원격 저장소(remote):** https://github.com/capshinsmg-blip/AI-Education.git
- **기본 브랜치:** `main`
- **동기화 규칙:** 이 경로(`C:\Users\Administrator\Desktop\AI_education`)의 모든 작업건은
  위 저장소에 동기화한다.

## 작업 워크플로우 (git-sync)

1. **작업 전:** `git pull origin main` 으로 최신 상태 동기화
2. **작업 수행:** 요청된 수정/작업 진행 후 변경 파일 요약을 사용자에게 보고
3. **작업 후(push 전 사용자 보고 필수):**
   - `git add -A`
   - `git commit -m "update: [변경 내용]"` (커밋 메시지는 한국어)
   - `git push origin main`

## 배포 URL

- (아직 배포 설정 없음)
