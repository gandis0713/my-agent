---
name: git-commit
description: 스테이징된 git 변경 사항을 분석하고 Claude Haiku로 커밋 메시지를 생성한 후 커밋합니다.
model: haiku
---

# Git Commit

스테이징된 git 변경 사항을 분석하고 Claude Haiku를 활용해 간결한 커밋 메시지를 자동 생성한 후 커밋합니다.

## 실행 단계

### 1. 스테이징된 변경 사항 확인

다음 명령어로 스테이징 상태를 확인합니다:

```bash
git status
git diff --cached
```

스테이징된 변경 사항이 없으면 실행을 중단하고 사용자에게 커밋할 내용이 없음을 알립니다.

### 2. Claude Haiku로 커밋 메시지 생성

스테이징된 diff를 캡처하여 Claude Haiku에 전달하고 커밋 메시지를 생성합니다:

```bash
git diff --cached | claude -m claude-haiku-4-5-20251001 -p "Write a git commit message for the following diff. Rules: (1) Subject line must be 72 characters or less, written in imperative mood (e.g. 'Add feature', not 'Added feature'). (2) Optionally follow with a blank line and a short body if the change needs explanation. (3) Output only the commit message — no extra commentary."
```

출력 결과를 커밋 메시지로 사용합니다.

### 3. 사용자 확인

생성된 커밋 메시지를 사용자에게 보여주고 커밋 전에 확인을 받습니다.

### 4. 커밋 생성

승인된 메시지로 git commit을 실행합니다:

```bash
git commit -m "<생성된 메시지>"
```

메시지가 여러 줄(제목 + 본문)인 경우 heredoc을 사용합니다:

```bash
git commit -m "$(cat <<'EOF'
<제목>

<본문>
EOF
)"
```

## 주의사항

- `git add`로 스테이징된 파일만 메시지 생성 컨텍스트로 사용합니다.
- 이 스킬은 파일을 스테이징하거나 언스테이징하지 않습니다.
- diff가 매우 큰 경우(500줄 이상), 전체 diff 대신 주요 변경 사항을 요약하여 전달합니다.
- pre-commit 훅을 건너뛰지 않습니다(`--no-verify` 사용 금지).
