# Agent Instructions

You are an AI agent working on this project. Follow this Git workflow for all development tasks.

## Branching Strategy

- Use feature branches for implementation (e.g., `impl/{issue-id}-{description}`)
- On feature branches, operate autonomously for routine development actions
- On the main branch (e.g., `master` or `main`), require explicit user permission before any write, commit, merge, or push action
- Always verify your current branch before write operations: `git branch --show-current`

## Development Cycle

### Phase 1: Planning (MANDATORY before any code)

1. **Clarify**: Ask the user clarifying questions about requirements, scope, and acceptance criteria. Do NOT skip this step.
2. **Issue Creation**: If no issue exists for a task, create one using GitHub CLI:
   ```bash
   gh issue create --title "..." --body "..."
   ```
3. **Context**: Use `gh issue view {issue-id}` to review existing issue details.
4. **Approval**: Present your plan to the user and **WAIT** for explicit confirmation before creating branches or starting code changes.

### Phase 2: Implementation

1. **Branching**: After receiving approval, create a branch:
   ```bash
   git checkout -b impl/{id}-{description-slug}
   ```
2. **Autonomous Execution**: On feature branches, proceed without per-tool-call confirmations unless the action is destructive, high-risk, or explicitly user-gated.
3. **Work**: Commit changes following the convention. Mention logic/progress in commit bodies.
4. **Push**: `git push origin impl/{id}-{description-slug}`.
5. **Create PR**: Create a Pull Request using GitHub CLI:
   ```bash
   gh pr create --title "type(scope): description" --body "Relates to #issue-id"
   ```
6. **Closure**: DO NOT use `gh issue close`. Issues must be closed MANUALLY by the USER after validation.

### Phase 3: Review & Sync

1. **Wait**: The USER will review and approve the PR on GitHub.
2. **Notification**: Wait for the USER to notify you when the PR has been merged.
3. **Sync**: Once notified, switch back to the main branch and pull the latest changes:
   ```bash
   git checkout main  # or master
   git pull origin main  # or master
   ```

## Commit Identity

- Always use `Agent ({model-slug})` with an appropriate email.
- Examples: `Agent (SWE-1.6)`, `Agent (GPT-5)`, `Agent (Codex)`, `Agent (Gemini)`, `Agent (Windsurf)`
- Configure before committing:
  ```bash
  git config user.name "Agent ({model-slug})"
  git config user.email "your-email@example.com"
  ```
- Always verify `git config user.name` before committing.

## Commit Message Convention

- Format: `type(scope): description`
- Include issue ID in footer or body: `Relates to #issue-id`
- Examples:
  - `feat(docs): update git-flow guidelines`
  - `fix(player): resolve crash on startup`

## Pushing Rules

- Pushing to feature branches (e.g., `impl/*`) is PERMITTED and REQUIRED for collaboration.
- Pushing directly to the main branch (e.g., `master` or `main`) is STRICTLY PROHIBITED.
- Process:
  1. Commit changes locally
  2. `git push origin impl/{branchname}`
  3. Create a PR for review

## Forbidden Actions

- NEVER commit as a personal username.
- NEVER push directly to the main branch (e.g., `master` or `main`).
- NEVER fail to verify identity before committing.
