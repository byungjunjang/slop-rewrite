---
name: styleguide-builder
description: 한 저자의 한국어 글 코퍼스(블로그 사이트맵/RSS/URL 목록/로컬 .md 폴더)에서 14개 절 고정 구조의 「글쓰기 문체 가이드라인」과 검사 키트(corpus·stats·targets.json·check_style)를 재현 가능하게 만드는 스킬. 숫자는 스크립트만 쓰고, 에이전트는 정성 워크시트만 채우며, 완성본은 재렌더 바이트 동일성으로 검증한다. "문체 가이드라인 만들어", "이 블로그 문체 분석해서 가이드라인", "저자 문체 프로파일", "문체 키트", "check_style 만들어", "author style guide", "style guideline from corpus", "/styleguide-builder" 에 쓴다. 보고서·개조식 문서나 초고 하나의 윤문에는 쓰지 않는다.
---

# styleguide-builder (플러그인 진입)

이 파일은 플러그인으로 설치됐을 때의 진입점이다. 실제 절차·규칙은 본체에 있다. 본체를 읽고 그대로 따르되, 경로만 아래 규칙으로 바꾼다.

## 본체

본체: `${CLAUDE_PLUGIN_ROOT}/.claude/skills/styleguide-builder/SKILL.md`. 먼저 Read 로 읽고 따른다. 본체의 `references/`, `assets/`, `scripts/`, `tests/` 는 `${CLAUDE_PLUGIN_ROOT}/.claude/skills/styleguide-builder/` 기준이다.

## 경로 규칙 (플러그인 설치본)

- 본체의 `.claude/skills/styleguide-builder/...` 상대경로는 앞에 `${CLAUDE_PLUGIN_ROOT}/` 를 붙인 절대경로로 쓴다.
- `$S` = `${CLAUDE_PLUGIN_ROOT}/.claude/skills/styleguide-builder/scripts`. 스크립트는 `python -X utf8 "$S/<script>.py"` 로 실행한다.
- 스크립트는 자기 파일 위치 기준으로 자료를 찾으므로 cwd 를 바꿀 필요가 없다.
- 공용 등록부(base-guidelines.json)는 `${CLAUDE_PLUGIN_ROOT}/.claude/skills/slop-rewrite/references/base-guidelines.json` 이다.
- HTML 수집에는 `beautifulsoup4` 와 `lxml` 이 필요하다. 빠진 패키지만 설치하고, 로컬 Markdown 처리에는 요구하지 않는다.

## 지속성 주의

이 스킬은 새 문체 가이드를 저장소 안(`styleguides/<slug>/`)에 만들고 공용 등록부에 등록한다. 플러그인 설치 폴더(`${CLAUDE_PLUGIN_ROOT}`)는 업데이트 때 초기화되므로, 만든 가이드를 계속 쓰려면 저장소를 클론해 그 폴더에서 쓰는 것을 권한다. 설치본에서 만든 결과물은 사용자가 지정한 폴더로 복사해 보관한다.

숫자 계산·재렌더·검증 절차와 등록 조건은 본체와 같다. 완성 가이드라인을 직접 고치지 않고 워크시트·프로파일·템플릿을 고쳐 재렌더한다.
