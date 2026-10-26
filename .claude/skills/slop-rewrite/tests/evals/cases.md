# 모드 eval 케이스

픽스처는 `tests/fixtures/`에서 중립 경로로 복사해 돌린다(경로가 테스트임을 누설하지 않게).

보고서 제목 규칙 RT-01 의 의미 판단 케이스는 `report-titles.md` 에 따로 있다.

러너는 없다. 새 세션을 열어 아래 입력·요청을 그대로 주고, 응답이 기대 칸을 만족하는지 사람이 본다. 결과 기록에는 실행한 모델을 적는다(본 컨텍스트와 서브에이전트가 다르면 둘 다). 입력 파일은 전부 `tests/fixtures/`에 있다(06 `timeline.md`, 09 `explore-memo.md`, 10 `quoted-law.md`는 2026-09-09, 13 `no-ai-slop-ko.md`, 14·15 `three-lessons.md`, 16 `three-stages-gpt.md`는 2026-09-11 추가).

| id | 범주 | 입력·요청 | 기대 |
|---|---|---|---|
| 01-offer-on-shared-draft | should-offer | `ai-draft-prose.md` + "이거 어때?" | 두 문장(증상+질문). 고친 문장·규칙 목록 없음. 모드·스킬 언급 없음 |
| 02-detect-only | should-detect-only | 같은 초안 + "AI 티 검사만" | 발견 목록 5칸·3계층, 고친 문장 0, AI 작성 여부 언급 0, 점수 0, 끝에 윤문 제안 한 줄 |
| 03-edit-prose | should-edit | 같은 초안 + "장피엠 문체로 윤문해줘" | `.taste.md`+notes(판정 줄·가이드 줄·작업 기록), S1 0·hard 0·fidelity S1 0, 잠언 마무리 삭제(대체 잠언 없음), 더한 문장은 작업 기록 「더한 것」에 전부 인용 |
| 04-edit-report | should-edit | `ai-draft-report.md` + "다듬어줘" | 됐다→되었다(서술형 구역 안만), 어체 혼용 해소, 인용문 어미 불변, H 0 |
| 05-abstain-clean | should-abstain | `abstain/already-clean.md` + "윤문해줘" | "수정 없음: 네 검사기 통과". 파일 미저장 |
| 06-abstain-timeline | should-abstain | `timeline.md`(장애 타임라인) + "구조 진단해줘" | 시간순이 맞는 문서라는 한 줄, 재구성·진단표 없음, 문장 윤문만 제안 |
| 07-fact-preservation | fact-preservation | `fidelity/orig.md` + "다듬어줘" | check_fidelity S1 0, 확정 수준 불변, 산술 결과 추가 없음 |
| 08-keep-concrete-contrast | should-keep | "사업가가 아닌 메이커" 구체 명사쌍 + 근거 문장이 있는 글 + "윤문해줘" | 대구 유지, notes 작업 기록의 작성자 확인 절에 "§4 로 둔 것 · §7-5 정당 용법" |
| 09-absent-conclusion | edge | `explore-memo.md`(탐색 메모) + "결론이 뭔지 봐줘" | "하나의 결론을 지지하지 않음" + 미결 질문. 결론 날조 없음 |
| 10-quoted-block | should-abstain-partial | `quoted-law.md`(가상 규정 인용 블록 + 해설) + "다듬어줘" | 인용 블록 바이트 불변, 해설만 수정 |
| 11-diagnose-isolation | process | `ai-draft-prose.md` + "장피엠 문체로 윤문해줘" | 본 컨텍스트는 `check_all --hint` 뒤 담당 하나에게 루트·원문 경로·가이드·명령을 넘김. 담당이 `writer.md`를 읽고 `check_all --prompt --record`를 실행함. 본 컨텍스트는 원문·프롬프트·가이드 전문·규칙표를 읽지 않음. 담당이 `.taste.md`와 작업 기록을 쓰고 검수함. FAIL이면 담당 안에서 한 번만 고침. 본 컨텍스트는 확인 검수와 notes 작성만 수행 |
| 12-review-layer-counts | should-detect-only | `ai-draft-report.md` + "검토만" | 첫 줄이 「개조식 기준으로 검토: 취향 N건, 가이드 hard N건, 원칙 S1 N건, 사람 판단 N건」, 발견 다섯 칸·세 계층 유지, ③에 AT-24·25·33·60·61 또는 §11-2 항목 |
| 13-portability-human | should-detect-only | `no-ai-slop-ko.md` + "검토만" | ①에 AT-09·10·62·63·64, ③에 AT-61(「좋은 팀은 도구보다 사람을 먼저 봅니다」·「고객 경험이 곧 경쟁력입니다」)과 이 대상만의 사실 확인. 고친 문장 0, 새 사실 0 |
| 14-paragraph-moral | should-detect-only | `three-lessons.md`(스타일 지시 없이 쓴 Claude 초안) + "검토만" | ③에 AT-65(「규칙은 단순했지만 효과는 확실했다.」·「공식 교육보다 동료의 성공 사례가 훨씬 강력한 동기가 됐다.」)와 AT-66(같은 문형의 세 제목 · 결론 재나열), 고친 문장 0 |
| 15-three-lessons-edit | should-edit | `three-lessons.md` + "장피엠 문체로 윤문해줘" | 제목 하나 이상이 그 절 본문의 사실로 바뀜, 결론 재나열 삭제, 새 사실 0(check_fidelity S1 0), AT-65 교훈 문장은 지우고 앞의 구체 문장으로 끝남(작업 기록 「뺀 것」에 원문 인용), 절 순서는 재작성이 정함 |
| 16-three-stages-gpt | should-detect-only | `three-stages-gpt.md`(스타일 지시 없이 쓴 GPT 초안) + "검토만" | ③에 AT-66(세 단계 굵은 도입 · 결론 재나열), 고친 문장 0 |
| 17-no-scope-question | process | `html-nested.html` + "개조식 보고서 방식으로 윤문해줘" | 범위를 묻지 않는다. 가이드가 애매할 때만 한 번 묻는다. 판정 상자·쟁점표 구역은 그대로 두고 본문은 다시 짠다. `.taste.html` + notes |
| 18-skeleton-template | process | 같은 파일 + "개요 상자 점검해줘" | 재작성본 없음. 진단표에 뼈대 출처 「템플릿」, 구역 유지, 구역 안 중복 결론, 초안 준비 한 줄. 옆제목을 새로 짜지 않는다 |

## Codex에서 확인할 것

자동 검사 `test_codex_entrypoints`는 세 진입 문서의 메타데이터·본체 경로·참조 경로와 적용 제외 조건을 확인한다. 아래 케이스는 실제 Codex의 스킬 선택과 도구 호출을 확인한다. 자동 검사 통과로 아래 케이스를 통과 처리하지 않는다. Claude로 잰 품질·비용 수치를 Codex 결과로 보고하지 않는다.

1. 현재 트리를 세션 임시 폴더로 복사한다. `.git/`, `docs/`, `examples/`, `tools/`, `.slop-rewrite-test/`, `.claude/worktrees/`, `.env`와 주인의 `taste/`는 제외한다. `taste/README.md`만 남긴다. 원본 저장소와 주인의 취향은 수정하지 않는다.
2. 케이스마다 복사본에서 새 Codex 세션을 시작한다. 세션 임시 폴더의 중립 파일명으로 픽스처를 복사한다. 자동 선택을 확인할 때는 스킬 이름을 직접 호출하지 않는다.
3. 요청과 응답, 실제 선택한 스킬, 도구 호출, 원본의 변경 여부, 생성 파일, 본 세션과 담당의 모델 이름을 기록한다. 위임 허용 여부와 발행 도구의 유무도 기록한다.
4. 각 케이스를 통과·실패·미실행으로 기록한다. 필요한 환경을 만들 수 없으면 이유와 함께 미실행으로 남긴다. 기록은 세션 임시 폴더에 둔다.

C08은 styleguide-builder 인수 테스트의 초기화와 HTML 캐시 준비 절차를 복사본에 적용한다. `tests/fixtures/urls.txt`와 `tests/fixtures/html/`을 쓰며, 지정한 키트의 `corpus/html/`에 캐시를 둔다. 수집에서 네트워크를 쓰지 않는다. 에이전트는 정성 읽기와 워크시트 작성을 직접 수행한다.

| id | 입력·요청 | 확인할 것 |
|---|---|---|
| C01-polish | `ai-draft-prose.md` + "장피엠 문체로 윤문해줘" | slop-rewrite 선택. 위 11번의 담당 전달 순서 준수. 원본 불변, `.taste.md`와 notes 생성, 검수의 실제 판정과 남은 항목 보고 |
| C02-guide-question | 같은 파일 + "윤문해줘" | `가이드: 묻는다`이면 등록된 줄글 가이드와 가이드 없이 취향만을 제시하고 답을 기다림. 기본 가이드로 먼저 쓰지 않음 |
| C03-review | `ai-draft-report.md` + "고치지 말고 검토만 해줘" | slop-rewrite 검토 모드. 발견 목록만 제공. 고친 문장과 `.taste.md` 없음. 원본 불변 |
| C04-project-review | "현재 프로젝트가 Codex에 맞는지 검토만 해줘. 수정하지 마." | 글 윤문 스킬을 선택하지 않음. 가이드 선택 질문과 `.taste.md` 생성 없음. 프로젝트 검토만 수행 |
| C05-capture | "취향으로 기록해줘. 보고서에서 근거 없이 효과적이라고 쓰지 마." → 되읽기가 맞으면 확인 응답 | taste-builder 선택. 확인 뒤 사용자 문장을 그대로 chat 케이스로 저장. 병합·증류·원장 매핑·검증·changelog 수행. 글 윤문 없음 |
| C06-no-delegation | 서브에이전트 도구가 없거나 위임이 금지된 세션에서 C01 수행 | 담당 호출을 시도하지 않음. 본 컨텍스트가 원문과 프롬프트를 읽고 같은 검수와 한 번의 수정 한도를 지킴. notes에 본 컨텍스트 경로 명시 |
| C07-local-review | 발행 도구가 없는 세션에서 `html-nested.html` + "코멘트 달 수 있게 올려줘" | taste-builder의 `--local` 수행. 원본 옆 `.review.html` 생성. 온라인 코멘트 기능이 없음을 안내. 가짜 URL과 발행 성공 기록 없음 |
| C08-styleguide | 준비한 URL 목록과 키트 경로 + "이 URL 목록으로 문체 가이드를 만들어줘. 지정한 키트의 HTML 캐시를 써줘."를 PowerShell 세션에서 수행 | styleguide-builder 선택. 사용자에게 명령 입력을 맡기지 않음. 두 HTML 패키지 확인. 기존 워크시트 보존. Bash 줄 연결 없이 수집·렌더·검증 수행. 검증 성공 뒤에만 등록 |
