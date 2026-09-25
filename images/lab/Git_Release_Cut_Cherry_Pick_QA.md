# git RELEASE CUT and CHERRY-PICK — Question and Answer

## Question

**Q: The release branch has already been cut from `main`, but an engineer is late in merging their feature. The feature is required for the release. What should the engineer do to add the already-committed changes to the release branch without merging the entire feature branch?**

## Answer

The engineer can **cherry-pick the required commit** onto the release branch.

Assume:

- Release branch: `release/v2.5.0`
- Feature branch: `feature/payment-api`
- Required commit: `abc1234`

The engineer should execute the following commands.

git SWITCH to RELEASE BRANCH

```text
git switch release/v2.5.0
```

Switches to the release branch.

git PULL LATEST RELEASE CHANGES

```text
git pull --ff-only
```

Gets the latest changes from the remote release branch without creating an unexpected merge commit.

git CHERRY-PICK FEATURE COMMIT

```text
git cherry-pick abc1234
```

Applies the changes introduced by the specified commit to the current release branch and creates a new commit.

git CHECK STATUS

```text
git status
```

Checks whether the cherry-pick completed successfully or whether there are conflicts that need to be resolved.

git RESOLVE CONFLICT

If Git reports a conflict, manually resolve the affected files and then stage the resolved files.

```text
git add [resolved-file]
```

Marks the resolved file as resolved.

git CONTINUE CHERRY-PICK

```text
git cherry-pick --continue
```

Continues the cherry-pick after all conflicts have been resolved and staged.

git ABORT CHERRY-PICK

If the feature should not be included or the cherry-pick cannot be completed safely:

```text
git cherry-pick --abort
```

Cancels the cherry-pick and returns the branch to its state before the cherry-pick started.

git RUN TESTS

```text
git status
```

Check the repository state and run the project's normal build and test commands before pushing the release change.

git PUSH RELEASE BRANCH

```text
git push origin release/v2.5.0
```

Pushes the cherry-picked commit to the remote release branch.

## Scenario Flow

The original feature branch contains:

```text
feature/payment-api
        |
        ● abc1234
```

The release branch has already been created:

```text
main
 |
 ●
 |
 release/v2.5.0
```

After cherry-picking:

```text
feature/payment-api
        |
        ● abc1234
        |
        |
release/v2.5.0
        |
        ● abc1234'
```

The changes from `abc1234` have been applied to the release branch as a **new commit**.

## Question

**Q: What if the engineer has multiple commits that are required for the release?**

## Answer

The engineer can cherry-pick multiple specific commits.

git CHERRY-PICK MULTIPLE COMMITS

```text
git cherry-pick [commit1] [commit2] [commit3]
```

Applies the selected commits to the current release branch.

For a continuous range of commits:

```text
git cherry-pick [first-commit]^..[last-commit]
```

Applies the specified range of commits, including the first and last commits.

## Question

**Q: What if the cherry-pick produces a conflict?**

## Answer

The engineer should resolve the conflict rather than blindly choosing one side.

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

If the cherry-pick should be cancelled:

```text
git cherry-pick --abort
```

Cancels the operation and returns the branch to its previous state.

## Question

**Q: Why use cherry-pick instead of merging the entire feature branch?**

## Answer

Cherry-pick is useful when the release team needs **specific commit(s)** from a feature branch rather than the complete branch history.

For example:

```text
git cherry-pick abc1234
```

applies the changes introduced by `abc1234` to the release branch.

The important point is that cherry-pick creates a **new commit** on the target branch containing the selected change.

## Question

**Q: What should the engineer finally do after the cherry-pick succeeds?**

## Answer

The engineer should verify the code, run the project's required tests, and push the release branch.

```text
git status
```

Checks the repository state.

```text
git push origin release/v2.5.0
```

Publishes the cherry-picked change to the remote release branch.

## Industry Workflow — Complete Command Sequence

```text
git switch release/v2.5.0
```

```text
git pull --ff-only
```

```text
git cherry-pick [commit-id]
```

```text
git status
```

```text
git add [resolved-file]
```

```text
git cherry-pick --continue
```

```text
git push origin release/v2.5.0
```

If the cherry-pick must be cancelled:

```text
git cherry-pick --abort
```
