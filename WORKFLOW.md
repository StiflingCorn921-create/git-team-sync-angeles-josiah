# WORKFLOW.md

## 1. What did the rejected push error message tell you, and why did it happen?

The rejected push showed: `! [rejected] feature/loyalty-points -> feature/loyalty-points (fetch first)`,
followed by a hint explaining that "the remote contains work that you do not have locally." This happened
because another clone (acting as a teammate) had already pushed a commit to the same branch on GitHub
before I tried to push my own commit. Git refuses non fast forward pushes by default to prevent silently
overwriting someone else's work — it forces you to first pull down the missing history so you can see and
resolve any conflicts before your changes are added.

## 2. What's the actual difference between how you resolved Task 3 (merge) vs Task 4 (rebase)?

In Task 3, I used `git fetch` + `git merge`, which created a new merge commit that combined both branches'
histories, with two parent commits (mine and the teammate's). The commit history stayed exactly as it
happened — messy but honest, showing both parallel lines of work joining together.

In Task 4, I used `git fetch` + `git rebase`, which instead replayed my commit on top of the latest commit
from the remote, rewriting my commit's history so it looks like I had made my change *after* the teammate's
change, rather than at the same time. This produced a clean, linear history with no merge commit — but it
also meant my original commit hash changed (rebase rewrites commits), and I had to resolve any conflicts one
commit at a time as they were replayed, rather than once for the whole set of changes like in a merge.

## 3. What one habit would have avoided both rejected pushes in this lab?

Running `git fetch` (or `git pull`) right before starting new work on a shared branch, every single time,
would have caught both situations early. In both cases, I made changes without first checking whether the
remote had moved on since my last sync — fetching first would have shown me the incoming changes right away,
before I invested time in a conflicting local commit.

## 4. Which approach — merge or rebase — would you default to on a shared team branch, and why?

I'd default to merge on a shared team branch. Rebase rewrites commit history, which is risky if anyone else
has already pulled the branch you're rebasing — it can cause duplicated or conflicting history for teammates
who based work on the original commits. Merge preserves the true, original timeline of what actually happened
and is much safer when multiple people are pushing to the same branch regularly. I'd save rebase for cleaning
up my own local, not yet shared commits before pushing for the first time.