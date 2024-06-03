---
title: A Simple Git Workflow - Releaseflow
feed: show
---

[https://hasnode.byrayray.dev/a-simple-git-workflow-releaseflow](https://hasnode.byrayray.dev/a-simple-git-workflow-releaseflow)

I've seen a lot of organizations use the Gitflow strategy to use Git. It's not a bad strategy, but it could be way simpler. That's why I was happy to find Releaseflow. In this post, I want to tell you how it works, and why it could be a benefit for your organization.

> "[Git Releaseflow](https://devblogs.microsoft.com/devops/release-flow-how-we-do-branching-on-the-vsts-team/)" The name of this workflow is coming from a Microsoft. (Watch the talk on [Youtube](https://youtu.be/t_4lLR6F_yk?t=924)) Release flow is much simpler than [gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow). Where gitflow has many different types of branches, release flow has only 4 of them.

![[assets/NhHYvPmBA.png]]

## Git release flow supports

- One source of truth (main branch)
- Manage releases in a simple way
- From feature branch directly to the main branch
- A release after each sprint

![[assets/NhHYvPmBA 1.png]]

## Branches

In release flow, we have two types of branches.

1. Long life branches
2. Short life branches

### 1. Long-life branches

The long-life branch in the git release flow is the `main` or `master` branch. Every feature, hotfix, or release branch will be created from the main branch. Also, these branches will be merged into the main branch with a pull request.

Suppose there are different environments you need to deploy but differ from the production version. It's a good idea to introduce feature flags in your backend or frontend application.

### 2. Short life branches

These short-life branches look similar to the git-flow. But in this case, we have only 3 of them.

1. Feature
2. Hotfix
3. Release

### 2.1 Feature

Every time a developer needs to create a new feature, there will be a branch `feature/{PBI-number}-descriptive-name`. During the development phase, it needs to be regularly updated with the main branch to prevent it from being out-of-sync.

A pull request will be created and merged into the main branch after all comments are resolved when the feature is developed and tested.

### 2.2 Hotfix

Building software without introducing bugs is a dream, so bugs should be fixed as soon as possible.

A hotfix branch is created after the bug is described as good as possible on the scrum board. The hotfix branch will look like this `hotfix/{PBI-number}-descriptive-name` and will be merged back to the main branch with a pull request. So all the future releases get the bugfix as well.

### 2.3 Release

At the end of each sprint, there will be a new release branch like this `release/{prefix}-{sprint-number}. This branch is created from the main branch and deployed to production.

For example, sprint 29 is ended. We create a branch `release/sprint-29`. This branch will be deployed to production. A few weeks later, we completed sprint 30. We make a branch `release/sprint-30`. Deploy this branch to production and remove the previous release branch if we want. This will keep the branches clean.

### 2.4 High priority bugs on production

Sometimes we have a critical bug on production. If we need to fix that, we make a hotfix branch. Merge that via a pull request to the main branch. After that is merged, we can create a pull request and merge it in the release branch.

In this way, we ensure that our main branch always has bug fixes.

## Commits

To get more track of the work that is being developed and released. It's essential to have clear commit messages which include references to a PBI or task. For example, in this commit message below (replace 000000 for the PBI or task number).

```Plain
Changed the if statement to conditionally load a new feature on test environment \#000000
```

If you use Azure Devops, you will see the reference below the **Development** heading. Github has the same feature like this.

## Different environments

A lot of companies use different environments for multiple purposes. Don't try to fix this by having multiple flavors of releases. Try to fix this in your code by creating feature flags.

With feature flags, you can ensure that in a test environment, for example, turn on a newly developed feature that production users shouldn't be able to see yet.

## Thanks

![[assets/nBF6anHH4w.png]]

_After reading this story, I hope you learned something new or are inspired to create something new! 🤗 If so, consider subscribing via email (scroll to the top of this page) or follow me here on Hashnode._

> Did you know that you can create a [Developer blog like this one, yourself](https://hashnode.com/@devbyrayray/joinme)? It's entirely for free. 👍💰🎉🥳🔥

_If I left you with questions or something to say as a response, scroll down and type me a message. Please send me a_ [_DM on Twitter @DevByRayRay_](https://twitter.com/@devbyrayray) _when you want to keep it private. My DM's are always open 😁_

### Subscribe to my newsletter

Read articles from Dev By RayRay directly inside your inbox. Subscribe to the newsletter, and don't miss out.
