---
name: taste-builder
description: 산출물을 코멘트용 claude.ai 아티팩트로 발행하고, 사용자가 「내가 쓴다면 이렇게 쓴다」고 단 코멘트나 대화에서 말한 취향을 케이스 원장에 쌓아 그 사람의 취향 문서(taste/writing-taste.md)를 증류한다. 취향을 적용해 글을 고치는 일은 slop-rewrite 스킬이 한다. 산출물을 코멘트 달 수 있게 올려 달라는 요청, 코멘트나 대화에서 말한 글쓰기 취향을 기록·반영해 달라는 요청(「다음부터 이렇게 써줘」 포함), 취향 문서를 만들거나 갱신해 달라는 요청에 쓴다. 예 · "코멘트 달 수 있게 올려줘", "취향으로 기록해줘", "/taste-builder".
---

# taste-builder (플러그인 진입)

이 파일은 플러그인으로 설치됐을 때의 진입점이다. 실제 데이터 스키마·병합·증류·검증 절차는 본체에 있다. 본체를 읽고 그대로 따르되, 경로만 아래 규칙으로 바꾼다.

## 본체

본체: `${CLAUDE_PLUGIN_ROOT}/.claude/skills/taste-builder/SKILL.md`. 먼저 Read 로 읽고 따른다. 본체의 `references/`, `scripts/`, `tests/` 는 `${CLAUDE_PLUGIN_ROOT}/.claude/skills/taste-builder/` 기준이다.

## 경로 규칙 (플러그인 설치본)

- 본체의 `.claude/skills/taste-builder/...` 상대경로는 앞에 `${CLAUDE_PLUGIN_ROOT}/` 를 붙인 절대경로로 실행한다.
  - 예) `python -X utf8 "${CLAUDE_PLUGIN_ROOT}/.claude/skills/taste-builder/scripts/to_artifact.py" --local "<원본>" "<원본 폴더>/<원본명>.review.<확장자>"`
- 스크립트는 자기 파일 위치 기준으로 자료를 찾으므로 cwd 를 바꿀 필요가 없다.
- 발행 도구(`Artifact`·`artifact-design`)가 실제 있는지 먼저 확인하고, 없으면 `--local` 로 로컬 검토 파일을 만들어 안내한다. 가짜 URL·발행 기록을 만들지 않는다.

## 지속성 주의

이 스킬은 취향 문서를 `${CLAUDE_PLUGIN_ROOT}/taste/writing-taste.md` 에 쌓는다. 플러그인 설치 폴더는 업데이트 때 초기화되므로, 취향을 계속 쌓아 매 윤문에 반영하려면 저장소를 클론해 그 폴더에서 쓰는 것을 권한다.

사용자 원문 보존, 명시적 요청이 있을 때만 취향 저장, 병합 → 증류 → 원장 매핑 → `validate_taste.py` 검증 → changelog 기록 절차는 본체와 같다.
