# git CONFLICT RESOLUTION

## Question

**Q: Two engineers are working on the same application. Engineer A modifies `UserService.cs` on `feature/user-profile` and pushes the changes. Engineer B also modifies the same part of `UserService.cs` on `feature/email-notification`. When Engineer B tries to merge the latest `main` into the feature branch, Git reports a conflict. How should the engineer resolve it?**

## Answer

Git cannot automatically determine which changes should be retained because the same part of the file was changed in different branches.

The engineer should first make sure they are on their feature branch and then bring the latest `main` changes into it.

git CHECK CURRENT BRANCH

```text
git status
```

Checks the current branch and repository state.

git SWITCH to FEATURE BRANCH

```text
git switch feature/email-notification
```

Switches to the feature branch that needs to be updated.

git FETCH LATEST CHANGES

```text
git fetch origin
```

Downloads the latest commits and references from the remote repository without modifying the current branch.

git MERGE MAIN INTO FEATURE BRANCH

```text
git merge origin/main
```

Attempts to merge the latest `main` branch into the current feature branch.

If Git finds conflicting changes, it reports a conflict.

git CHECK CONFLICTS

```text
git status
```

Shows the files that have conflicts and need to be resolved.

git REVIEW CONFLICT

```text
git diff
```

Shows the conflicting changes so the engineer can understand what changed on both sides.

The conflicted file may contain markers similar to:

```text
<<<<<<< HEAD
Engineer B's changes
=======
Changes from main
>>>>>>> origin/main
```

The engineer must manually edit the file and decide what the final correct code should be.

The conflict markers must be removed.

git STAGE RESOLVED FILE

```text
git add UserService.cs
```

Marks the conflict as resolved after the engineer has reviewed and corrected the file.

git CHECK RESOLUTION

```text
git status
```

Confirms that the conflicted file has been staged and that no unresolved conflicts remain.

git COMMIT MERGE

```text
git commit
```

Creates the merge commit after the conflicts have been resolved.

git PUSH FEATURE BRANCH

```text
git push origin feature/email-notification
```

Pushes the resolved feature branch to the remote repository.

## Question

**Q: What if the engineer wants to keep the version from the current feature branch during the conflict?**

## Answer

For a supported merge conflict, the engineer can use:

```text
git checkout --ours [file]
```

Keeps the current branch version of the specified file.

Then:

```text
git add [file]
```

Marks the file as resolved.

## Question

**Q: What if the engineer wants to keep the version coming from `main`?**

## Answer

For a supported merge conflict, the engineer can use:

```text
git checkout --theirs [file]
```

Keeps the incoming version of the specified file.

Then:

```text
git add [file]
```

Marks the file as resolved.

The engineer should use these commands only after understanding the business logic. In many real conflicts, the correct solution is to manually combine parts of both changes rather than choosing only one side.

## Question

**Q: What if the engineer realizes that the merge should not be performed?**

## Answer

The engineer can cancel the in-progress merge.

git ABORT MERGE

```text
git merge --abort
```

Cancels the merge and returns the working tree to the state it was in before the merge started.

## Question

**Q: What is the complete conflict-resolution workflow?**

## Answer

The typical workflow is:

git CHECK STATUS

```text
git status
```

git SWITCH to FEATURE BRANCH

```text
git switch feature/email-notification
```

git FETCH LATEST CHANGES

```text
git fetch origin
```

git MERGE MAIN

```text
git merge origin/main
```

git CHECK CONFLICTS

```text
git status
```

git REVIEW CONFLICT

```text
git diff
```

Resolve the conflicting code manually.

git STAGE RESOLVED FILE

```text
git add [resolved-file]
```

git CHECK STATUS AGAIN

```text
git status
```

git COMPLETE MERGE

```text
git commit
```

git PUSH RESOLVED BRANCH

```text
git push origin feature/email-notification
```

## Question

**Q: What happens if there are conflicts in multiple files?**

## Answer

Git may report several conflicted files.

For example:

```text
UserService.cs
NotificationService.cs
UserController.cs
```

The engineer should resolve each file, stage each resolved file, and then complete the merge.

```text
git add UserService.cs
```

```text
git add NotificationService.cs
```

```text
git add UserController.cs
```

Then:

```text
git status
```

Finally:

```text
git commit
```

## Industry Scenario Flow

```text
main
  |
  |------ Engineer A changes UserService.cs
  |
  |------ Engineer B changes UserService.cs
                         |
                         ↓
              Engineer B merges main
                         |
                         ↓
                    CONFLICT
                         |
                         ↓
                 git status
                         |
                         ↓
                  git diff
                         |
                         ↓
             Manually resolve code
                         |
                         ↓
                  git add [file]
                         |
                         ↓
                    git commit
                         |
                         ↓
                    git push
```

## Important Rule

**Do not blindly choose `ours` or `theirs`.**

First understand why the conflict occurred and what the application should do after combining the changes. Then resolve the code, run the required tests, and push the corrected branch.
