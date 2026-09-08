# 🤖 Gemini AI Code Review Bot (Reusable Workflow)

GitHub Actions와 무료 Gemini API를 활용한 서버리스(Serverless) AI 코드 리뷰 봇입니다. 
Reusable Workflow 형태로 구현되어 있어, 다른 레포지토리에서도 단 몇 줄의 코드만으로 즉시 도입할 수 있습니다.

## 🚀 사용 방법

### 1. API 키 설정
리뷰 봇을 적용할 레포지토리의 **Settings > Secrets and variables > Actions** 메뉴로 이동하여, `GEMINI_API_KEY`라는 이름으로 [Google AI Studio](https://aistudio.google.com/)에서 발급받은 API 키를 저장합니다.

### 2. 워크플로우 파일 추가
해당 레포지토리의 `.github/workflows/ai-review.yml` 파일을 생성하고 아래 코드를 작성합니다.

```yaml
name: Call AI Reviewer

on:
  pull_request:
    types: [opened, synchronize, ready_for_review]

jobs:
  ai-review:
    if: github.event.pull_request.draft == false
    uses: hungerbk/my-github-actions/.github/workflows/ai-review.yml@main
    secrets:
      GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
```

세팅이 완료되면 PR을 올리거나 새로운 커밋을 푸시할 때마다 AI가 코드를 분석하여 리뷰 코멘트를 남깁니다.

💡 기술적 고민(최적화, 프롬프트 엔지니어링, 트러블슈팅)에 대한 상세한 내용은 [Wiki](https://github.com/hungerbk/my-github-actions/wiki)를 참고해 주세요.
