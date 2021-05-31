# 🔡 Git Flow and Code Versioning

## Overview

We follow some directives for our branches and how to work with them.

Keep reading to find out what those are.

## Branches

These all are the branches that you'll need to work on your repository:

- `master`, the main branch that represents the code in production. Direct commits are forbidden and it should only be updated by mergin the `develop` branch through **pull requests**;

- `develop`, originally created from the `master` branch, represents the most recent code. Direct commits are forbidden and it should only be updated by mergin the `feature` branch through **pull requests**;

- `feature`, originally created from the `develop` branch, represents a (whole or _just a part of a_) feature  in the product backlog. Once finished it should update `develop` branch through **pull request** and goes under **code review** for general quality assurance;

- `migration`, originally created from the `develop` branch, represents a technology migration or framework update. Once finished it should update `develop` branch through **pull request** and goes under **code review** for general quality assurance;

- `fix`, originally created from the `develop` branch, represents any adjustment, refactoring or change that doesn't fit as a new feature. Once finished it should update `develop` branch through **pull request** and goes under **code review** for general quality assurance;

- `hotfix`, originally created from the `master` branch, represents an incident, a bug in production that has to be handled with urgency. Once finished it should update `master` branch through **pull request**. As it represents a critical fix, it **doesn't need** to go under **code review**. As soon as this branch is merged with `master` a pull request will be automatically opened to `develop` to equalize the environments;

The picture below represents this model.

![image.png](https://i.imgur.com/tWOYSyc.png)

## Rules and Polices

- **Number of Reviewers** - minimum number of approvals in order to merge the pull request _(current number is 2)_;

- **Required Reviewers** - approval from the guardian of the project in order to merge the pull request;

- **Issue** - all pull requests must have a issue linked in order to be merged. These issues must have their state **different** than **'closed'**. Features, fixes and migrations can have issues of any type while hotfixes **must have** an **incident** or **bug** issue to it;

- **Comments** - before merging the pull request all comments raised by any user must be resolved;

- **Naming and git flow** - Pull requests of `feature`, `migration` and `fix` branches must target the **develop** branch, `hotfix`  must target **master** branch. All branches naming should **respect** the `kebab-case` and the following pattern `<type>/<short-description>`.
  - `feature/my-feature` ;
  - `fix/my-fix` ;
  - `hotfix/my-hotfix` ;
  - `migration/my-migration` ;
- **Quality Gate** - Submited code must meet the predefined criteria on **Quality Gate**

### Best Pratices

- Branch names, commits, pull request descriptions, everything should be written in **english**;
- **Be polite and don't ignore code review comments.** If someone commented on your pull request and you're not quite sure if you'd follow the suggestion or not, then reply the comment thread to clear any possible doubts or explain why you're not taking that suggestion. **Don't mark as resolved** if you're not going to resolve. If you don't agree or if it's going to be developed in a future branch then mark it as **Won't fix** and explain the reason;
- **Make small PRs**.  Wanna a fast review? Do this, it’s easier for the reviewers to understand the context and reason with the logic. A good pull request should not have more than **250** lines of code and must focus on only **one** thing. Remember the [single responsability principle](https://medium.com/@tbaragao/solid-s-r-p-single-responsibility-principle-2760ff4a7edc) that you consider while coding (_or at least you **should be considering**_)? Well, guess what, that principle doesn't applies only to coding itself but to making pull requests as well;
- **Write useful descriptions and titles**. Write a useful description in the details section, try to guide the reviewers through the code;
- **Related commits**. Adds to a PR only code related to one fix, change or feature, try not to mix it up;
- **Commit message**. Short summary of the changes for that commit. (50 chars as guideline). Check out [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/);

> **Important**
>
> Always aim to follow the pratices described above. Pull requests that don't follow the naming conventions will be **automatically blocked** from merging. Those too large also tend to be rejected.

### Creating your PR

First create an Issue, if it doesn't exist, then you can create the Pull Request.

### Reviewing code

It is expected that EVERYONE! review each other's code.

If you want to dig deeper in this subject, we've this article [here](/Code%20Review.md) that explains everything that you need to know about code revie, why and how to do it.

## References

- [The Anatomy of a Perfect Pull Request](https://medium.com/@hugooodias/the-anatomy-of-a-perfect-pull-request-567382bb6067)
- [The (written) unwritten guide to pull requests](https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests)
- [Best practices for good PR's](https://www.kenneth-truyers.net/2018/11/01/best-practices-good-pr/)
- [Effective Pull Requests: A Guide](https://www.braintreepayments.com/blog/effective-pull-requests-a-guide/)
- [Git Commit Best Practices](https://github.com/trein/dev-best-practices/wiki/Git-Commit-Best-Practices)
