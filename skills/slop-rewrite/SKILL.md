---
name: slop-rewrite
description: 문서 산출물을 원칙(공용 AI 티 목록)·스타일가이드(줄글 장피엠·업무·토스 문체, 보고서 개조식 가이드라인, 등록한 내 가이드)·취향(taste/writing-taste.md) 세 레이어로 윤문·퇴고하고, 고쳐 달라는 말이 없으면 고치지 않고 검토·구조 진단·제안으로 답한다. 글의 종류로 가이드를 고르고, 가이드가 여럿이거나 애매하면 사용자에게 묻는다. 한국어 문서(글·보고서·공지·메일)를 윤문·퇴고하거나 AI 티·번역투를 없애 달라는 요청, 취향이나 특정 문체 가이드로 고쳐 달라는 요청, 고치지 말고 검토·검사만 해 달라는 요청, 두괄식·결론 위치·개요 상자 같은 구조 진단 요청에 쓴다. 예 · "윤문해줘", "AI 티 없애줘", "장피엠 문체로 고쳐줘", "taste polish <파일>", "이 문서 검토만 해줘", "두괄식인지 진단해줘", "/slop-rewrite". 코드·PR 검토에는 쓰지 않는다.
---

# slop-rewrite (플러그인 진입)

이 파일은 플러그인으로 설치됐을 때의 진입점이다. 실제 절차·규칙·검사 기준은 본체에 있다. 본체를 읽고 그대로 따르되, 경로만 아래 규칙으로 바꾼다.

## 본체

본체: `${CLAUDE_PLUGIN_ROOT}/.claude/skills/slop-rewrite/SKILL.md`

먼저 이 파일을 Read 로 읽고, 네 모드(윤문·검토·구조 진단·제안)와 Step 1~6 절차를 그대로 따른다. 본체가 참조하는 `references/`(modes.md·writer.md·subagent.md·base-guidelines.json 등)는 `${CLAUDE_PLUGIN_ROOT}/.claude/skills/slop-rewrite/references/` 다.

## 경로 규칙 (플러그인 설치본)

- 본체가 `.claude/skills/slop-rewrite/...` 로 시작하는 상대경로(scripts·references)를 쓰면, 앞에 `${CLAUDE_PLUGIN_ROOT}/` 를 붙인 절대경로로 실행한다.
  - 예) `python -X utf8 "${CLAUDE_PLUGIN_ROOT}/.claude/skills/slop-rewrite/scripts/check_all.py" --guide 없음 --hint <원문>`
- 공용 자료는 `${CLAUDE_PLUGIN_ROOT}/principles/` 와 `${CLAUDE_PLUGIN_ROOT}/styleguides/` 다.
- 스크립트는 자기 파일 위치(`__file__`) 기준으로 principles·styleguides·references·base-guidelines.json 을 찾는다. 그래서 스크립트 경로만 위처럼 절대경로로 주면 cwd 와 무관하게 동작한다. cwd 를 설치 폴더로 바꿀 필요는 없다.
- 윤문 결과(`<원본명>.taste.<확장자>`)·notes·작업 기록·스크래치 파일은 사용자의 원문이 있는 폴더나 세션 스크래치패드에 쓴다. 설치 폴더(`${CLAUDE_PLUGIN_ROOT}`)에는 쓰지 않는다 — 플러그인 업데이트 때 지워진다.
- 본체 Step 2 에서 서브에이전트 브리프에 넣는 「프로젝트 루트」 값은 `${CLAUDE_PLUGIN_ROOT}` 다. 담당도 같은 절대경로 규칙으로 스크립트를 실행하고, 결과·기록은 원문 옆이나 스크래치패드에 쓴다.

## 취향(taste)

본체는 취향을 `${CLAUDE_PLUGIN_ROOT}/taste/writing-taste.md` 에서 읽는다. 플러그인 설치본에는 기본으로 없으므로 취향 없이 동작한다. 취향을 쌓아 매 윤문에 반영하려면 저장소를 클론해 그 폴더에서 쓰는 것을 권한다(설치 폴더는 업데이트 때 초기화된다).

원본 보존, 네 모드 구분, 검사 기준, 되돌림 한 번, 결과와 notes 형식은 본체와 같다. 검토 요청에는 고친 문장을 제공하지 않는다.
