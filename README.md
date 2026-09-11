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

jobs:
  ai-review:
    if: github.event.pull_request.draft == false
    uses: hungerbk/my-github-actions/.github/workflows/ai-review.yml@main
    secrets:
      GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
```

설정이 완료되면 PR을 생성하거나 새로운 커밋을 푸시할 때마다 AI가 변경된 코드를 분석하여 PR에 코드 리뷰 코멘트를 남깁니다.

> **Note**
> Fork에서 생성된 PR은 GitHub Actions의 보안 정책상 Repository Secret에 접근할 수 없어 AI 리뷰가 실행되지 않을 수 있습니다.

## 📚 자세한 내용

토큰 최적화, 프롬프트 엔지니어링, 할루시네이션 방지 및 트러블슈팅 등 구현 과정에서의 기술적 고민은 [Wiki](https://github.com/hungerbk/my-github-actions/wiki)에서 확인할 수 있습니다.
