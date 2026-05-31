# Agent Git Workflow

This document outlines the standard Git workflow for AI agents working on projects.

## Branching Strategy

*   **Implementation Phase Branching**: Use `impl/{issue-id}-{description}`.
    *   Example: `impl/93-restore-git-flow`
*   **Autonomy by Branch**:
    *   On feature branches (e.g., `impl/*`), agents are expected to run autonomously for routine development actions without asking for confirmation on every tool call.
    *   On the main branch (e.g., `master` or `main`), agents must stay cautious and require explicit user permission before any write, commit, merge, or push action.
    *   If branch context is unclear, verify current branch first (`git branch --show-current`) before write operations.

## Task Tracking

*   **GitHub Issues**: ALL tasks (including documentation) MUST have a GitHub issue for tracking.
*   **Planning Phase (MANDATORY before any code)**:
    1.  **Clarify**: Ask the user clarifying questions about the task to fully understand the requirements, scope, and acceptance criteria. Do NOT skip this step.
    2.  **Issue Creation**: If no issue exists for a task, create one using GitHub CLI:
        ```bash
        gh issue create --title "..." --body "..."
        ```
    3.  **Context**: Use `gh issue view {issue-id}` to review existing issue details when working on a pre-existing issue.
    4.  **Approval**: Present the plan to the user and **WAIT** for explicit confirmation ("Green Light") before creating the initial `impl/*` branch or starting code changes.
*   **Implementation Phase**:
    1.  **Discovery**: Use `gh issue list` to view pending tasks.
    2.  **Branching**: **After receiving approval**, create a branch:
        ```bash
        git checkout -b impl/{id}-{description-slug}
        ```
    3.  **Autonomous Execution**: Once on `impl/*`, proceed without per-tool-call confirmations unless the action is destructive, high-risk, or explicitly user-gated.
    4.  **Work**: Commit changes following the convention. Mention logic/progress in commit bodies.
    5.  **Push**: `git push origin impl/{id}-{description-slug}`.
    6.  **Create PR**: Create a Pull Request using GitHub CLI:
        ```bash
        gh pr create --title "type(scope): description" --body "Relates to #issue-id"
        ```
    7.  **Closure**: **DO NOT** use `gh issue close`. Issues must be closed MANUALLY by the USER after validation.
*   **Review & Sync Phase**:
    1.  **Wait**: The USER will review and approve the PR on GitHub.
    2.  **Notification**: The USER will notify the agent when the PR has been merged.
    3.  **Sync**: Once notified, switch back to the main branch and pull the latest changes:
        ```bash
        git checkout main  # or master
        git pull origin main  # or master
        ```

## Commit Identity

*   **Identity**: Always use `Agent ({model-slug})` with an appropriate email.
    *   Examples: `Agent (SWE-1.6)`, `Agent (GPT-5)`, `Agent (Gemini)`
*   **Configuration**:
    ```bash
    git config user.name "Agent ({model-slug})"
    git config user.email "your-email@example.com"
    ```
*   **Verification**: Always verify `git config user.name` before committing.
*   **Cleanup**: Unset the configuration after the session/commit if needed, though most agents persist per-session.

## Commit Message Convention

*   **Format**: `type(scope): description`
    *   Includes issue ID if applicable in the footer or body, but the title should follow Conventional Commits.
    *   Example: `feat(docs): update git-flow guidelines`
    *   Example: `fix(player): resolve crash on startup`

## Pushing Rules

*   **Allowed**: Pushing to `impl/*` branches is **PERMITTED** and **REQUIRED** for collaboration.
*   **Forbidden**: Pushing directly to the main branch (e.g., `master` or `main`) is **STRICTLY PROHIBITED**.
*   **Process**:
    1.  Commit changes locally.
    2.  `git push origin impl/{branchname}`.
    3.  Create a PR for review.

## Forbidden Actions

*   NEVER commit as a personal username.
*   NEVER push directly to the main branch (e.g., `master` or `main`).
*   NEVER fail to verify identity before committing.
