# 👍 Code Review 👎

## Contents

- [Why code reviewing](#why-code-reviewing)
- [What to look in a code review](#what-to-look-in-a-code-review)
- [As a Reviewer or Author](#as-a-reviewer-or-author)
  - [Assume competence](#assume-competence)
  - [Provide rationale or context](#provide-rationale-or-context)
  - [Consider how comments may be interpreted](#consider-how-comments-may-be-interpreted)
  - [Don't criticize the person](#don-t-criticize-the-person)
  - [Don't use harsh language](#don-t-use-harsh-language)
- [As a Reviewer](#as-a-reviewer)
  - [Provide specific and actionable feedback](#provide-specific-and-actionable-feedback)
  - [Clearly mark nitpicks and optional comments](#clearly-mark-nitpicks-and-optional-comments)
- [As an Author](#as-an-author)
  - [Naming your PR](#naming-your-pr)
  - [Clarify code or reply to the reviewer's comment in response to feedback](#clarify-code-or-reply-to-the-reviewer-s-comment-in-response-to-feedback)
- [References](#references)

---

## Why code reviewing

<https://google.github.io/eng-practices/review/reviewer/standard.html#conflicts>

## What to look in a code review

<https://google.github.io/eng-practices/review/reviewer/looking-for.html>

## As a Reviewer or Author

### Assume competence

An author’s implementation or a reviewer’s recommendation may be due to the other party having different context than you. Start by asking questions to gain understanding.

### Provide rationale or context

Such as a best practices document, a style guide, or a design document. This can help others understand your decision or provide mentorship.

### Consider how comments may be interpreted

Be mindful of the differing ways hyperbole, jokes, and emojis may be perceived.

- **BAD example**: `I prefer short names so I’d rather not change this. Unless you make me? :)`
- **GOOD example**: `Best practice suggests omitting obvious/generic terms. I’m not sure how to reconcile that advice with this request.`

### Don't criticize the person

Instead, discuss the code. Even the perception that a comment is about a person (e.g., due to using “you” or “your”) distracts from the goal of improving the code.

- **BAD example**: `Why are you using this approach? You’re adding unnecessary complexity.`
- **GOOD example**: `This concurrency model appears to be adding complexity to the code without any visible performance benefit.`

### Don't use harsh language

Code review comments with a negative tone are less likely to be useful.

## As a Reviewer

### Provide specific and actionable feedback

If you don’t have specific advice, sometimes it’s helpful to ask for clarification on why the author made a decision.

### Clearly mark nitpicks and optional comments

By using prefixes such as ‘Nit’, ‘Optional’ or ‘Suggestion’. This allows the author to better gauge the reviewer’s expectations.

## As an Author

### Naming your PR

`[optional scope] #<task id> Pull request title`

A **scope** may be provided to a PR title to provide additional contextual information and is contained within brackets.

Don't use generic and meaningless names such as `'fix issues'`, `'refactoring'` and so on. Try to be as much precise as you can humanly be when giving your PR a title.

E.g. `[WhatsApp Gateway] Add new message template type`.

### Clarify code or reply to the reviewer's comment in response to feedback

Failing to do so can signal a lack of receptiveness to implementing improvements to the code

- **BAD example**: `That makes sense in some cases but not here`
- **GOOD example**: `I added a comment about why it’s implemented that way`

> When disagreeing with feedback, explain the advantage of your approach

## References

- [The Anatomy of a Perfect Pull Request](https://medium.com/@hugooodias/the-anatomy-of-a-perfect-pull-request-567382bb6067)
- [The (written) unwritten guide to pull requests](https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests)
- [Best practices for good PR's](https://www.kenneth-truyers.net/2018/11/01/best-practices-good-pr/)
- [Effective Pull Requests: A Guide](https://www.braintreepayments.com/blog/effective-pull-requests-a-guide/)
- [Git Commit Best Practices](https://github.com/trein/dev-best-practices/wiki/Git-Commit-Best-Practices)
