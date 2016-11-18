# Collaborator Guide

**Contents**

- [Issues and Pull Requests](#issues-and-pull-requests)
- [Accepting Modifications](#accepting-modifications)
  - [Involving the PO/TC](#involving-the-potc)
- [Landing Pull Requests](#landing-pull-requests)
  - [I've made a huge mistake](#ive-made-a-huge-mistake)
- [Creating a Release](#creating-a-release)

This document contains information for Collaborators of this project regarding
maintaining the code, documentation and issues.  This is almost exactly the same
as the [Node.js Collaborator Guide](https://github.com/nodejs/node/blob/master/COLLABORATOR_GUIDE.md)
with changes to better fit our needs.

Collaborators should be familiar with the guidelines for new contributors in
[CONTRIBUTING.md](./CONTRIBUTING.md) and also understand the project governance
model as outlined in [GOVERNANCE.md](./GOVERNANCE.md).

Collaborators are users with write access to this repository and work directly
in the main repository.  Collaborators are responsible for merging in pull
requests and making releases.

Contributors are users with read access to this repository and work in their
individual forks of the main repository.


## Issues and Pull Requests

Courtesy should always be shown to individuals submitting issues and pull
requests to this project.

Collaborators should feel free to take full responsibility for managing issues
and pull requests they feel qualified to handle, as long as this is done while
being mindful of these guidelines, the opinions of other Collaborators and
guidance of the PO or TC as defined by the Governance model.

Collaborators may **close** any issue or pull request they believe is not
relevant for the future of this project.  Where this is unclear, the issue
should be left open for several days to allow for additional discussion.  Where
this does not yield input from project Collaborators or additional evidence that
the issue has relevance, the issue may be closed.  Remember that issues can
always be re-opened if necessary.


## Accepting Modifications

All modifications to project code and documentation should be performed via pull
requests, including modifications by Collaborators, PO, and TC members.

For small projects with a single Collaborator (aka, the PO), all modifications
are under the full responsibility of the PO.

All pull requests must be reviewed and accepted by a Collaborator with
sufficient expertise who is able to take full responsibility for the change.  In
the case of pull requests proposed by an existing Collaborator, an additional
Collaborator is required for sign-off.

In some cases, it may be necessary to summon a qualified Collaborator to a pull
request for review by @-mention.

If you are unsure about the modification and are not prepared to take full
responsibility for the change, defer to another Collaborator.

Before landing pull requests, sufficient time should be left for input from
other Collaborators.  Trivial changes may be landed after a shorter delay.

Where this is no disagreement amongst Collaborators, a pull request may be
landed given appropriate review.  Where there is discussion amongst
Collaborators, consensus should be sought if possible.  The lack of consensus
may indicate the need to elevate discussion to the PO or TC for resolution (see
below).

All bug fixes require a test case with demonstrates the defect.  The test should
*fail* before the change, and *pass* after the change.

All pull requests that modify executable code should be subjected to continuous
integration tests on the project CI server.


### Involving the PO/TC

Collaborators may opt to elevate pull requests or issues to the PO/TC for
discussion by assigning the ***tc-agenda***  or ***po-review*** tag.  This
should be done where a pull request:

- has a significant impact on the codebase,

- is inherently controversial; or

- has failed to reach consensus amongst the Collaborators who are actively
  participating in the discussion.

The PO/TC should server as the final arbiter where required.


## Landing Pull Requests

On GitHub, this has been made very easy.  When a Pull Request is ready to be merged in, 
click the "Squash and merge" button.

On BitBucket, this is not an option.  You can manually squash and merge the commits into
a single commit, but it is more trouble than it is worth.

Always modify the original commit message to include additional meta information
regarding the change process:

- A `Reviewed-By: Name <email>` line for yourself and any other Collaborators
  who have reviewed the change.

- A `PR-URL:` line that references the full URL of the original pull request
  being merged so it's easy to trace a commit back to the conversation that led
  up to that change.

- A `Fixes: X` line, where _X_ is either the full URL for an issue, and/or the
  hash and commit message if the commit fixes a bug in a previous commit.
  Multiple `Fixes:` lines may be added if appropriate.

See the commit log for examples if unsure exactly how to format your commit
messages.

Additionally:

- Double check PRs to make sure the person's _full name_ and email address are
  correct before merging.

- Except when updating dependencies, all commits should be self contained
  (meaning every commit should pass all tests).  This makes it much easier when
  bisecting to find a breaking change.



### I've made a huge mistake

With `git` there's a way to override remote trees by force pushing
(`git push -f`).  This should generally be seen as forbidden (since you're
rewriting history on a repository other people are working against) but its
allowed for simpler slip-ups such as typos in commit messages.  However, you are
only allowed to force push to any projects branch within 10 minutes from your
original push.  If someone else pushes to the branch or the 10 minute period
passes, consider the commit final.


## Creating a Release

Creating a release basically means bumping the package.json version number,
generating the changelog, and merging the code into the master branch, which
will generate a build that can be deployed to any environment, including
production.

```shell
$ git checkout develop
$ git fetch -p; git pull origin develop
$ jq .version package.json
0.29.0
$ git checkout -b release/0.30.0
$ vim package.json

... update the version from 0.29.0 to 0.30.0 in package.json ...

$ npm run generate-changelog

> REPO_NAME@0.29.0 generate-changelog /Users/jyoung/dev/REPO_NAME
> changelog-maker --group

* [[`47d94cbca5`](https://github.com/ORG_NAME/REPO_NAME/commit/47d94cbca5)] - update candidate info (AUTHOR)

... copy the commit lines, they start with * ...

$ vim CHANGELOG.md

... update the changelog, follow the conventions of previous entries, including spacing and capitalization ...

$ git commit -am 0.30.0
$ git checkout master
$ git merge release/0.30.0
$ git push origin master
$ git tag v0.30.0
$ git push origin v0.30.0
$ git checkout develop
$ git merge master
$ git push origin develop
```

Copy the CHANGELOG.md entry you made earlier.  Add the release notes to GitHub.
Goto https://github.com/ORG_NAME/REPO_NAME/tags and click
_**Add Release Notes**_ next to the tag that was pushed up earlier.

Publish the released version to npm (if needed).
