---
name: impl-with-commit
description: >
  Implements code with one git commit per completed task or objective, using
  terse Conventional Commits messages coworkers can scan. Splits oversized
  work into subtasks before committing. Use when user says "implement with
  commit", "commit as you go", "atomic commits while coding", invokes
  /impl-with-commit, or asks the agent to commit after each task while
  implementing.
---

# Implement with Commit

While implementing, commit after each finished task. Do not batch all work into one end-of-session commit.

## Loop

1. **Plan tasks** — Break the request into small objectives (one logical change each). Prefer more commits over fewer.
2. **Implement one task** — Edit only what that objective needs. Keep the tree buildable when practical.
3. **Commit immediately** — Stage relevant files (not secrets), write the message, commit, verify with `git status` / `git log -1`.
4. **Next task** — Repeat until done. Summarize the commit list at the end.

If a single task's diff is large or mixes concerns, **split into subtasks** and commit each separately before continuing.

## Commit message (caveman style)

Conventional Commits. Why over what. Terse and exact.

**Subject:** `<type>(<scope>): <imperative summary>`
- Types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci`, `style`, `revert`
- Imperative: "add", "fix", "remove" — not "added"/"adds"
- ≤50 chars when possible, hard cap 72; no trailing period

**Body:** only when why is non-obvious, or for breaking changes / migrations / security. Wrap 72. Bullets `-`. Skip fluff ("This commit...", "I/we", AI attribution) unless the user's rules require a trailer.

```
feat(api): add GET /users/:id/profile

Mobile client needs profile without full user payload.
```

Pass the message via HEREDOC:

```bash
git commit -m "$(cat <<'EOF'
feat(scope): imperative summary

Optional body when why is not obvious.

EOF
)"
```

## Git safety

- Never update git config; never force-push; never skip hooks unless asked
- Do not commit `.env`, credentials, or secrets — warn instead
- Prefer feature branch over committing straight to `main`/`master` unless user asked
- Follow the user's amend/push rules; default is new commits only
- If a hook rejects the commit, fix and create a **new** commit (do not amend unless user rules allow)

## Do not

- Wait until the whole feature is done to commit
- Mix unrelated concerns in one commit
- Restate filenames in the subject when scope already covers them
- Push unless the user explicitly asks
