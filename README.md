# 🤖 Gemini AI Code Review Bot (Reusable Workflow)

GitHub Actions와 Gemini API를 활용한 서버리스(Serverless) AI 코드 리뷰 봇입니다.

GitHub Actions의 **Reusable Workflow**로 구현하여, 별도의 서버 구축 없이 다른 레포지토리에서도 간단한 설정만으로 PR 코드 리뷰를 자동화할 수 있습니다.

## 🚀 사용 방법

### 1. Gemini API 키 설정

리뷰 봇을 적용할 레포지토리의 **Settings > Secrets and variables > Actions** 메뉴로 이동합니다.

[Google AI Studio](https://aistudio.google.com/)에서 발급받은 API 키를 다음 이름으로 저장합니다.

`GEMINI_API_KEY`

### 2. 워크플로우 파일 추가

리뷰 봇을 사용할 레포지토리에 `.github/workflows/ai-review.yml` 파일을 생성하고 다음 코드를 작성합니다.

```yaml
name: Call AI Reviewer

on:
  pull_request:
    types: [opened, synchronize, ready_for_review]

permissions:
  contents: read
  pull-requests: write

jobs:
  ai-review:
    if: ${{ github.event.pull_request.draft == false && !contains(github.event.pull_request.body || '', '[skip-ai-review]') }}
    uses: hungerbk/my-github-actions/.github/workflows/ai-review.yml@main
    secrets:
      GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
```


설정이 완료되면 PR을 생성하거나 새로운 커밋을 푸시할 때마다 AI가 변경된 코드를 분석하여 PR에 코드 리뷰 코멘트를 남깁니다.

> **Note**
> Fork에서 생성된 PR은 GitHub Actions의 보안 정책상 Repository Secret에 접근할 수 없어 AI 리뷰가 실행되지 않을 수 있습니다.

### 3. 리뷰가 불필요한 PR

PR 작성 시 리뷰 필요 여부를 판단하고, 동작 변경 없는 린트·포맷 정리처럼 리뷰가 불필요한 변경이면 PR 본문에 `[skip-ai-review]`를 포함합니다. AI에게 PR 작성을 맡길 때도 이 규칙을 적용할 수 있습니다.

README·Markdown 변경도 기본적으로 리뷰 대상입니다. 문서 변경만 있는 PR도 리뷰를 생략하려면 본문에 `[skip-ai-review]`를 포함합니다.

위 예제의 조건은 호출하는 저장소의 워크플로우에 설정합니다. Draft이거나 본문에 해당 문구가 있으면 reusable workflow 호출 작업을 건너뜁니다.

- PR 생성 전에 리뷰 필요 여부를 결정합니다.
- 생성 후 문구를 추가하면 다음 커밋 push(`synchronize`)부터 리뷰를 생략합니다.
- 문구를 제거하면 다음 커밋 push부터 다시 리뷰합니다.
- 리뷰 준비 완료 전환(`ready_for_review`) 시에도 같은 조건을 확인합니다.
- 본문 수정만으로는 실행되지 않으며 이미 실행 중인 리뷰를 취소하지 않습니다.

문구를 사용법 예시로 인용한 경우에도 본문에 포함되어 있으면 리뷰가 생략됩니다.

## 📚 자세한 내용

토큰 최적화, 프롬프트 엔지니어링, 할루시네이션 방지 및 트러블슈팅 등 구현 과정에서의 기술적 고민은 [Wiki](https://github.com/hungerbk/my-github-actions/wiki)에서 확인할 수 있습니다.
