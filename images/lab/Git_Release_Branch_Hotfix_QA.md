# git RELEASE BRANCH HOTFIX — Question and Answer

## Question

**Q: A production issue is discovered after the release branch has already been created. The fix is urgent and must go into the current release without waiting for the next normal feature-development cycle. How should the engineer create and deliver the hotfix?**

## Answer

The engineer should create a **hotfix branch from the release branch**, implement the fix, commit it, push it, and raise a Pull Request against the release branch.

Assume:

- Release branch: `release/v2.5.0`
- Hotfix branch: `hotfix/payment-validation`
- Hotfix commit: `[hotfix-commit]`

git CHECK CURRENT STATE

```text
git status
```

Checks the current repository state before starting the hotfix.

git FETCH LATEST RELEASE

```text
git fetch origin
```

Gets the latest references from the remote repository without changing the current working branch.

git SWITCH to RELEASE BRANCH

```text
git switch release/v2.5.0
```

Switches to the release branch from which the hotfix should be created.

git UPDATE RELEASE BRANCH

```text
git pull --ff-only
```

Gets the latest release branch changes without creating an unexpected merge commit.

git CREATE HOTFIX BRANCH

```text
git switch -c hotfix/payment-validation
```

Creates and switches to a new hotfix branch based on the current release branch.

git IMPLEMENT HOTFIX

Make the required code changes to resolve the production issue.

git CHECK CHANGES

```text
git status
```

Checks the modified files.

git REVIEW CHANGES

```text
git diff
```

Reviews the changes before staging them.

git STAGE HOTFIX

```text
git add [file]
```

Stages the files containing the hotfix.

git REVIEW STAGED CHANGES

```text
git diff --staged
```

Reviews exactly what will be included in the hotfix commit.

git COMMIT HOTFIX

```text
git commit -m "fix: resolve payment validation issue"
```

Creates a focused commit containing the hotfix.

git PUSH HOTFIX BRANCH

```text
git push -u origin hotfix/payment-validation
```

Publishes the hotfix branch to the remote repository.

git PULL REQUEST

Create a Pull Request from:

```text
hotfix/payment-validation
```

into:

```text
release/v2.5.0
```

The team can then perform code review and run the required CI/CD checks before merging the hotfix.

## Question

**Q: What happens after the hotfix Pull Request is approved and merged into the release branch?**

## Answer

The release branch now contains the hotfix and can be used for the release deployment.

The engineer can update the local release branch:

git SWITCH to RELEASE BRANCH

```text
git switch release/v2.5.0
```

git PULL LATEST RELEASE

```text
git pull --ff-only
```

Gets the merged hotfix from the remote release branch.

The team can then run the required release validation and deployment process.

## Question

**Q: Should the hotfix also be applied to `main`?**

## Answer

If the production fix is also required in future development, the fix should be propagated to the appropriate development branch according to the team's release process.

One common approach is to merge or cherry-pick the hotfix commit into `main`.

git FIND HOTFIX COMMIT

```text
git log --oneline
```

Finds the commit containing the hotfix.

git SWITCH to MAIN

```text
git switch main
```

Switches to the main development branch.

git UPDATE MAIN

```text
git pull --ff-only
```

Gets the latest main branch changes.

git CHERRY-PICK HOTFIX

```text
git cherry-pick [hotfix-commit]
```

Applies the hotfix change to `main` as a new commit.

After testing:

```text
git push origin main
```

Pushes the propagated hotfix to the remote main branch.

## Question

**Q: What if the hotfix commit cannot be cleanly applied to `main`?**

## Answer

A conflict may occur because `main` has changed since the release branch was created.

git CHECK CONFLICT

```text
git status
```

Shows the files involved in the conflict.

Resolve the affected files manually and then:

```text
git add [resolved-file]
```

Stages the resolved file.

Continue:

```text
git cherry-pick --continue
```

If the hotfix should not be propagated to `main`:

```text
git cherry-pick --abort
```

Cancels the cherry-pick.

## Question

**Q: What is the complete release-hotfix workflow?**

## Answer

git FETCH

```text
git fetch origin
```

git SWITCH to RELEASE

```text
git switch release/v2.5.0
```

git UPDATE RELEASE

```text
git pull --ff-only
```

git CREATE HOTFIX BRANCH

```text
git switch -c hotfix/payment-validation
```

Implement and test the fix.

git REVIEW

```text
git status
```

```text
git diff
```

git STAGE

```text
git add [file]
```

git REVIEW STAGED CHANGES

```text
git diff --staged
```

git COMMIT

```text
git commit -m "fix: resolve payment validation issue"
```

git PUSH

```text
git push -u origin hotfix/payment-validation
```

Create a Pull Request:

```text
hotfix/payment-validation
                ↓
        release/v2.5.0
```

After approval and merge:

```text
git switch release/v2.5.0
```

```text
git pull --ff-only
```

If the fix is required in `main`:

```text
git switch main
```

```text
git pull --ff-only
```

```text
git cherry-pick [hotfix-commit]
```

```text
git push origin main
```

## Industry Scenario Flow

```text
                 release/v2.5.0
                       |
                       ●
                       |
              Production issue
                       |
                       ↓
             hotfix/payment-validation
                       |
                       ●
                       |
                       ↓
                 Pull Request
                       |
                       ↓
              release/v2.5.0
                       |
                       ●
                       |
                    Deploy
                       |
                       ↓
                  Production
```

If the same fix is required for ongoing development:

```text
hotfix/payment-validation
            |
            ●
            |
            ↓
       release/v2.5.0
            |
            |
            └──────────────→ main
                              |
                       cherry-pick
                              |
                              ●
```

## Important Rule

**A hotfix should start from the branch that represents the version being fixed.**

For a release-specific production issue, create the hotfix branch from the appropriate `release/*` branch, validate it, review it, and merge it through the team's normal Pull Request and CI/CD process.

If the same correction is needed in future development, make sure the fix is also propagated to the appropriate development branch.
