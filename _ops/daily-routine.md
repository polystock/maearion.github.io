# 메아리온 일일 콘텐츠 작업 절차

매일 05시(KST) 예약 작업이 이 파일을 읽고 그대로 따릅니다.
이 절차를 바꾸려면 이 파일만 고치면 됩니다.

## 절대 규칙
1. **main에 직접 push하지 않는다. PR을 머지하지 않는다.** 머지는 으르신만 한다.
2. **불완전하면 PR을 만들지 않는다.** 하루 건너뛰는 것이 틀린 글보다 낫다.
3. **원문을 실제로 연 자료만 쓴다.** 검색 스니펫, 초록 요약 사이트, 다른 AI의 답은 근거가 아니다. 수치·주장은 연 원문에서 위치를 찾을 수 있어야 한다.
4. 분류 기준은 세기 전에 정하고 끝까지 바꾸지 않는다.
5. "개 침에 아밀라아제가 있다"는 수정은 절대 반영하지 않는다.
6. 원문에서 확인 못 한 수치는 출처가 무엇이든(다른 AI, 2차 문헌) 넣지 않는다.

## 0. 준비
- 레포 `polystock/maearion.github.io`를 push 권한으로 붙이고 clone한다.
- `apt-get install -y jekyll` (rubygems 접근 불가라 gem 설치는 실패한다)
- GitHub: `gh pr create` 등 GraphQL 기반 명령은 막혀 있다. `unset GH_TOKEN` 후 `gh api repos/polystock/maearion.github.io/...` REST 경로만 쓴다(PR 생성 `POST /pulls`, 댓글 `POST /issues/{n}/comments`, 목록 `GET /pulls?state=open`).

## 1. 주제 고르기
- `_ops/topic-queue.md`에서 첫 항목 중 다음을 모두 만족하는 것:
  `_docs/<slug>.html`이 main에 없음, `daily/<slug>` 브랜치의 열린 PR 없음.
- 열린 `daily/` PR이 이미 3개 이상이면 새 글을 쓰지 않는다. 대신 2단계로 간다.
- 큐가 비었으면 아무것도 만들지 않고 "큐 비었음"만 보고한다.

## 2. 열린 daily PR 정리 (매번)
- 열린 `daily/` PR이 main과 충돌하면 rebase한다. 충돌은 목록 추가 줄(index.html, 분류 페이지, sitemap.xml, URL_MANIFEST.txt, _data/myths.yml)에서만 나야 한다. 양쪽 줄을 모두 살려 해결한다.
- 그 밖의 충돌은 손대지 않고 PR에 댓글로 알린다.

## 3. 조사
- 1차 자료 우선: 학술지 원문(PMC 공개본 포함), 학회·기관 지침(AAHA, WSAVA, AAFCO, FEDIAF, RECOVER 등), 정부 자료(농림축산식품부, 농촌진흥청, FDA 등).
- 근거표를 `_ops/evidence/<slug>.md`에 만든다. 행마다: 본문 문장(또는 수치) | 출처 | 원문 위치(절·표·쪽) | 원문 해당 내용 요지.
- 원문을 열 수 없는 자료는 근거표에 "미열람"으로 적고 본문에 쓰지 않는다.
- 핵심 주장을 받칠 1차 자료가 2건 미만이면 중단하고 "보류: 자료 부족"으로 보고한다.

## 4. 원고
- 기존 문서(`_docs/dog-heatstroke.html`, `_docs/myth-dog-age-seven.html`)와 같은 구조로 쓴다:
  front matter(title, og_title, description, og_description, ld_headline, ld_description, published_at, modified_at, category, nav_variant, meta_variant, faq) → h1 → lead → meta(작성: 에카원 · YYYY년 M월) → 번호 붙은 h2 절 → 자주 묻는 질문 → 참고 문헌(ul.src) → 관련 정보(.related) → note.
- 본문은 "~다" 체, FAQ 답은 "~니다" 체. front matter의 faq와 본문 FAQ 내용이 일치해야 한다.
- 통설 검증 문서는 판정(근거 없음 / 부분적으로 맞음 / 권고와 어긋남 등)을 lead에서 밝힌다.
- 연구가 말한 범위를 넘겨 일반화하지 않는다(사례 수 ≠ 위험도, 상관 ≠ 인과, 종·품종·국가 한정).
- 진단·처방 대체가 아님을 note에 적는다.

## 5. 연결 (고아 페이지 방지 — 하드코딩이다)
- 분류 페이지(`<분류>.html`)의 목록 ul에 li 추가. 통설 검증은 `_data/myths.yml`에 카드 추가.
- `index.html` "전체 문서"의 해당 분류 section에 li 추가.
- `sitemap.xml`에 url 추가(lastmod = 오늘).
- `URL_MANIFEST.txt`에 경로 추가.

## 6. 독립 검수 (원고 작성자와 다른 에이전트)
- 새 에이전트를 띄운다. 넘기는 것은 **완성 원고 파일과 참고 문헌 목록뿐**. 근거표·작성 메모는 넘기지 않는다.
- 지시: 출처를 직접 열어 본문의 사실 주장·수치마다 `일치 / 불일치 / 출처에 없음 / 열람 불가`로 판정하고 원문 위치를 적어라. 수정하지 말고 표만 돌려라. 없으면 0건이라 적어라.
- 불일치·출처에 없음 → 작성자가 고치거나 그 문장을 지운다. 그 뒤 **새 에이전트로** 한 번 더 검수한다.
- 두 번째 검수 후에도 남는 문장은 지운다. 지우고 나서 문서가 성립하지 않으면 PR을 만들지 않고 "보류"로 보고한다.

## 7. 빌드 확인
- `jekyll build` 성공, `_site/<slug>.html` 존재, 분류 페이지 문서 수 +1, Liquid 오류 없음.
- 바뀐 파일마다 `git hash-object` 값, `_site/<slug>.html`의 sha256 값을 기록한다.

## 8. PR
- 브랜치 `daily/<slug>`, main 기준. 커밋 1개.
- PR 제목: `[daily] <문서 제목>`
- PR 본문 맨 위에 **외부 AI 검수용 링크 2개**(브랜치 기준 raw 주소):
  `원고: https://raw.githubusercontent.com/polystock/maearion.github.io/daily/<slug>/_docs/<slug>.html`
  `근거표: https://raw.githubusercontent.com/polystock/maearion.github.io/daily/<slug>/_ops/evidence/<slug>.md`
- 그 아래: 한 줄 요약 / 바뀐 파일 / blob SHA·빌드 sha256 / 자료 부족·삭제한 문장이 있으면 그 목록.
- PR에 댓글로 최종 검수표를 단다(1차·2차 결과 모두).
- PR을 머지하지 않는다.
## 9. 보고
- 결과를 한 줄로: `PR #N 올림: <제목>` 또는 `보류: <사유>` 또는 `큐 비었음`.
