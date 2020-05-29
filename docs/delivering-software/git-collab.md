 # Collaborating with Git

## Merge conflicts

Merge conflicts can occur when you have an existing commit on your local repo and you try to **pull** the latest commits from the remote repo.

There are two ways that merge conflicts can occur.

- You are behind the remote branch by a few commits. Your commit edits the same line on a same file with another commit (on the remote repo) that you have not integrated with.
- You are merging another branch to master branch.

You can manually fix merge conflicts by removing the `<<<<<< HEAD` and `======` strange lines that were added by git when it detected a merge conflict.

If you would like to review the latest commits without causing merge conflicts, use **fetch** instead of pull.

### Fixing merge conflict in VS Code

![fix merge conflict](_media/gitcollab-fixMergeConflict.png)

- Click on **Accept current change** to select the remote changes and delete your change
- Click on **Accept incoming change** to select your changes and delete the changes from remote
- Click on **Accept both** to select all changes and keep both

Use `Control + Z` to undo if you selected the wrong choice
Alternatively, you can directly edit in VS Code.

### Why do we prefer to use rebase when pulling?

Why can't we just pull normally without rebasing? If we do that, then it will create an ugly merge commit when there are merge conflicts. We prefer to `git pull --rebase`, solve the merge conflicts, then continue with the rebasing.

An extra merge commit might make sense if there are many merge conflicts but that is a good indication that integration was not done often enough.

### Merge hell

If and when we don't follow the practice of (i) committing often and (ii) push often on the master branch (a.k.a. trunk-based development), we can land ourselves in merge hell.
Merge hell is a sticky place and getting out of it can be painful and time-consuming (it can take hours or even days). Instead of building your application, now you have to spend time tearing your hair out!

## Pull request

Pull request (PR) is a feature by Github (or Gitlab, Bitbucket etc), not git.

There can be two types of pull requests:

- Pull Request from a forked repository
- Pull Request from a branch within a repository

### A Pull Request lifecycle in Github

#### Create a PR

By default, we can create a PR from a branch. Click **compare across forks** to create a PR from a forked repository. It should look something like this:

![create pull request](_media/gitcollab-createPullRequest.png)

#### PR is open

Example of an **open** Pull Request from a **forked** repository:

![open pull request](_media/gitcollab-openPullRequest.png)

A PR should have a good title and description that could summarise the PR.
Sometimes, we can even have pull request templates for larger projects with many contributors. These templates will automatically load content into the description field when creating a new PR. Look at [an example here](https://blog.axosoft.com/enhancing-pull-request-descriptions-templates/).

See Github help for more information for [PR templates](https://help.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository).

![pull request sidebar](_media/gitcollab-pullRequestSidebar.png)

It is possible to assign, add labels and even add to a project (see Github Project Boards).

#### Review a PR

Go to any PR and click **Files Changed**.

![review pull request](_media/gitcollab-reviewPullRequest.png)

- Give general comments by clicking **Review Changes** on the top right corner
- Click the `+` button on the line that you want to comment on. The review will be **pending** until you finally submit the review.

You can require a review from contributors before merging any PR. These settings are at the branch protection rules.

![branch protection rule](_media/gitcollab-branchProtectionRule.png)

#### Merge and close a PR

After merging a PR, the PR can be now closed.

## Git workflows

There are two common git workflows.

### Trunk-based development

![smaller trunk based](_media/gitcollab-smallerTrunkBased.png)
(Image from https://trunkbaseddevelopment.com/)

In trunk-based development (TBD), developers always check into one branch, typically the **master branch** also called the “trunk”.

They check in (git push) as frequently as possible to the master — at few times a day.

Every developer is touching the trunk, so all features grow in the mainline which acts as a communication point.

This workflow is meant to be used together with a **Continuous Integration** server that runs the test after every commit to ensure that no commit breaks the build.

Read https://www.thoughtworks.com/insights/blog/enabling-trunk-based-development-deployment-pipelines to understand how trunk-based development can be enabled with pipelines and processes.

#### Scaled Trunk-based development (optional)

![scaled trunk based](_media/gitcollab-scaledTrunkBased.png)
(Image from https://trunkbaseddevelopment.com/)

You might wonder, in a larger team, how do we enforce the reviewing of pull requests if everyone pushes directly to the master branch? It is stil possible, with developers pushing to **short-lived** feature branches. The difference is that these feature branches do not last more than a few days and are not related to any specific versions.

#### Pros

No integration problems later as all features are integrated everyday from the very start.

Better communication between developers.

#### Cons

Many frequent changes to master every day. If there is not enough automated tests or a poorly setup continuous integration server, an integration fault could go undetected.

Might need feature toggles to switch off features that are in testing but not supposed to be in production.

#### How to merge to master for trunk-based

1. Start from master
1. Develop a new feature on master
1. `git add -p` and `git commit -m`
1. `git pull --rebase`
1. Run all tests. If failing, fix the error, amend the commit `git commit --amend` and back to Step 4
1. If there are conflicts, solve the merge conflicts and then run `git rebase --continue`
1. `git push`

Note: if switching between different work (that should be in another commit) remember to stash your changes

### Feature Branch

![feature branch workflow](_media/gitcollab-featureBranchWorkflow.png)
Feature branch workflow

- Developers pull the latest master and branch out to their individual feature branch
- They work on the feature on their branch and when it is complete, merge it to master locally.
- They run tests after the merge as well to ensure that nothing is broken before pushing their changes to master

#### Pros

Not that many changes to master branch in a day

#### Cons

If there are "long lived branches" (eg: developer keeps a branch for many days without merging to master) there could be a bigger merge conflict when they finally do merge to master. This causes integration delay.

Not merging often may lead to less communication between developers and cause incompatible features to be developed by different developers.

#### How to merge to master for feature branch

1. Start on master do a git pull --rebase to get latest changes from remote.
1. `git checkout -b new-branch` where `new-branch` is the name of your new feature branch name. This will create a new local branch.
1. Develop a new feature on local branch.
1. Periodically do `git pull --rebase origin master` this will get the latest changes from remote master and integrate it to your local branch so that you are not too far behind on your local branch. This reduces further merge conflicts.
1. `git add -p` and `git commit -m "Add..."`

When we decide to merge to master...

1. `git pull --rebase origin master` to pull latest changes from remote master
1. There could be merge conflicts, solve them.
1. `git checkout master`
1. `git merge new-branch` where `new-branch` is the name of your new feature branch name
1. Run all tests. If failing, fix the error, and amend the commit
1. `git push`
1. `git branch -d new-branch`. Delete the feature branch.

## Practice

We will clone a repo https://github.com/thoughtworks-jumpstart/git-team-pokemon for the lab.

Another lab for git ignore, merge conflicts, pull request: https://github.com/thoughtworks-jumpstart/git-newsroom

To experience merge hell for yourself, there is another lab: https://github.com/thoughtworks-jumpstart/merge-hell-simulation
