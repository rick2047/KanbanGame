
# Workflow

## GitHub & PRs
- Repo: `https://github.com/rick2047/KanbanGame` (owned by rick2047).
- Expect `gh` auth to be logged in as `rick2047`.
- Always use the `gh` tool for all GitHub interactions (PR creation, issues, comments, etc.).
- If you don't know the exact subcommand to run, use `gh api` / `gh api graphql`.
- Always merge PRs using a merge commit (no squash merges).
- When asked to explicitly trigger a Gemini review, comment on the PR with `/gemini review`.
- Use these commands (current, preferred):
  - Trigger Gemini review:
    - `gh api -X POST repos/<OWNER>/<REPO>/issues/<PR_NUMBER>/comments -f body="/gemini review"`
  - List review threads + database IDs (for replies):
    - `gh api graphql -f query='query($owner:String!,$name:String!,$number:Int!){repository(owner:$owner,name:$name){pullRequest(number:$number){reviewThreads(first:100){nodes{isResolved isOutdated path line comments(first:10){nodes{author{login} body databaseId id url}}}}}}}' -f owner=<OWNER> -f name=<REPO> -F number=<PR_NUMBER>`
- Use `gh` to check PR-scoped CI runs and inspect artifacts. Example commands:
  - `gh pr view <PR_NUMBER> --json headRefName -q .headRefName`
  - `gh run list --branch <HEAD_BRANCH> --workflow "PR Tests" --limit 1`
  - `gh run view <RUN_ID> --log`
  - `gh run download <RUN_ID> -n <ARTIFACT_NAME> -D /tmp/gh-artifacts`

## OpenSpec Workflow
- OpenSpec is the primary driver for workflow: create one PR per OpenSpec change, and keep only one change active at a time.

## Review Comment Handling
- When asked to address review comments, reply on each review comment with what you did to resolve it.
- Never resolve review threads unless explicitly asked.
- Use `gh` for all review comment replies. Example commands you can run:
  - Fetch comment database IDs:
    - `gh api graphql -f query='query($owner:String!,$name:String!,$number:Int!){repository(owner:$owner,name:$name){pullRequest(number:$number){reviewThreads(first:100){nodes{isResolved isOutdated path line comments(first:10){nodes{author{login} body databaseId id url}}}}}}}' -f owner=<OWNER> -f name=<REPO> -F number=<PR_NUMBER>`
  - Reply to a review comment (note: `in_reply_to` uses the numeric `databaseId`):
    - `gh api -X POST repos/<OWNER>/<REPO>/pulls/<PR_NUMBER>/comments -f body="Fixed: <what changed>" -F in_reply_to=<COMMENT_DATABASE_ID>`
  - Resolve a thread:
    - `gh api graphql -f query='mutation($id:ID!){resolveReviewThread(input:{threadId:$id}){thread{id isResolved}}}' -f id=<THREAD_ID>`

# Testing

## General
- Add as many tests as you can. 

# Running

## Tooling

## Documentation Resources

### Context7 (AI-Optimized Documentation)
Always use context7 to research available documentation before proposing or making changes.