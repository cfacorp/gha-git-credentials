# Contributing

When contributing to this repository,
please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository
before making a change. 

Please note we have a code of conduct, please follow it
in all your interactions with the project.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Proposing enhancements, reporting bugs and other issues](#proposing-enhancements-reporting-bugs-and-other-issues)
- [Testing](#testing)
- [Pull Request Process](#pull-request-process)
- [Release process](#release-process)
  - [1. Publish code changes](#1-publish-code-changes)
  - [2. Advertise release candidate](#2-advertise-release-candidate)
  - [3. Test the release candidate](#3-test-the-release-candidate)
  - [4. Tag new release](#4-tag-new-release)
  - [5. Update docs](#5-update-docs)
  - [6. Publish release](#6-publish-release)
- [Code of Conduct](#code-of-conduct)
  - [Our Pledge](#our-pledge)
  - [Our Standards](#our-standards)
  - [Our Responsibilities](#our-responsibilities)
  - [Scope](#scope)
  - [Enforcement](#enforcement)
  - [Attribution](#attribution)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- generated with [DocToc](https://github.com/thlorenz/doctoc) -->

## Proposing enhancements, reporting bugs and other issues

Before suggesting an enhancement, new feature, or reporting a bug,
required change due to GitHub API change or any other issue
please browse or search project
[issues](https://github.com/cfacorp/gha-git-credentials/issues)
and [pull requests](https://github.com/cfacorp/gha-git-credentials/pulls).

If you find a matching issue please contribute there with a comment.

If you do not find any please feel free creating an issue or
opening a pull request.

Please consider including code fragments, logs and/or links
to support your bug report or enhancement whether it is
an issue or a pull request.

When proposing an enhancement via a pull request consider
implementation that would not break experience of the project
users, securing backward compatibility, if possible.

Feel free forking the project.

## Testing

Please test your proposal submitted via a pull request.
GitHub workflows allow specifying an action's version.
This might be a versioning tag, a commit or a branch name.

Testing tools and guidelines to consider:
- https://github.com/cfacorp/gha-git-credentials-test
- https://github.com/tschoffelen/gha
- https://github.com/nektos/act
- https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/creating-a-javascript-action#testing-out-your-action-in-a-workflow
- https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-commands-for-github-actions

## Pull Request Process

1. Ensure any install or build dependencies are removed before 
   the end of the layer when doing a build.
2. Update the [`README.md`](./README.md) with details of changes to the interface,
   this includes new environment variables, API, 
   useful file locations and container parameters if any.
3. Increase the version numbers in any examples files and
   the [`README.md`](./README.md), [`package.json`](package.json) to the new version that this
   Pull Request would represent.
   Update [`CHANGELOG.md`](./CHANGELOG.md) with version features description.
   The versioning scheme we use is [SemVer](http://semver.org/).
4. You may merge the Pull Request in once you have the sign-off
   of one other maintainer, or if you 
   do not have permission to do that, 
   you may request any maintainer to merge it for you.

Consider having `Resolves #<issue-reference>` in either of your commits
if there is any relevant issue open. This will close the issue once
the PR is approved and merged.

## Release process

1. Publish code changes
2. Advertise release candidate
3. Test release candidate
4. Tag new release
5. Update docs
6. Publish release

### 1. Publish code changes

This action deployment workflow uses itself to
build its own distribution package.

Deployment action is triggered once a commit introducing a change to
either of the files specified under `on` clause
in [`.github/workflows/deploy.yml`](./.github/workflows/deploy.yml)
is pushed to `master` branch on GitHub.

The action creates a distribution package, commits,
sets (forcedly) `rc` (Release Candidate) tag and
pushes the change.

### 2. Advertise release candidate

Invite maintainers, contributors and key users
to test the Release Candidate and
[report any bugs](https://github.com/cfacorp/gha-git-credentials/issues/new?assignees=&labels=bug&template=bug_report.md&title=%5BBUG%5D+Short+bug+description)
or [confirm that no issues are detected](https://github.com/cfacorp/gha-git-credentials/issues/new?assignees=&labels=rc-test-ok&template=release-candidate-test-ok.md&title=%5BRC%5D+Release+Candidate+test+-+OK).

### 3. Test the release candidate

- Trigger test at [gha-git-credentials-test](https://github.com/cfacorp/gha-git-credentials-test)
- Check [kottans/frontend-2022-homeworks actions log](https://github.com/kottans/frontend-2022-homeworks/actions).
  See recent daily `Update students' PR stats` workflow run. 
  It uses the `rc` version of the action.
  Make sure that reviewed workflow run time is relevant to the tested RC version.
- Run other tests as appropriate.

### 4. Tag new release

- Pull the latest changes from the remote
- Run `npm set-version-tags X Y Z` to set `vX.Y.Z`, `vX.Y-latest`,
  `vX-latest`, and `latest` tags 

### 5. Update docs

- `README.md` sections: Inputs, Outputs, Usage Example, Versions
- [`CHANGELOG.md`](./CHANGELOG.md)
- [`package.json/version`](./package.json)
- run `npm run generate-toc`

### 6. Publish release

[Create and publish a release](https://github.com/cfacorp/gha-git-credentials/releases)
publishing this Action to the GitHub Marketplace.

## Code of Conduct

### Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, gender identity and expression, level of experience,
nationality, personal appearance, race, religion, or sexual identity and
orientation.

### Our Standards

Examples of behavior that contributes to creating a positive environment
include:

* Using welcoming and inclusive language
* Being respectful of differing viewpoints and experiences
* Gracefully accepting constructive criticism
* Focusing on what is best for the community
* Showing empathy towards other community members

Examples of unacceptable behavior by participants include:

* The use of sexualized language or imagery and unwelcome sexual
  attention or advances
* Trolling, insulting/derogatory comments, and personal or political attacks
* Public or private harassment
* Publishing others' private information, such as a physical or electronic
  address, without explicit permission
* Other conduct which could reasonably be considered inappropriate in a
  professional setting

### Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable
behavior and are expected to take appropriate and fair corrective action in
response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or
reject comments, commits, code, wiki edits, issues, and other contributions
that are not aligned to this Code of Conduct, or to ban temporarily or
permanently any contributor for other behaviors that they deem inappropriate,
threatening, offensive, or harmful.

### Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.

### Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported by contacting the project team via [project issues](https://github.com/cfacorp/gha-git-credentials/issues).
All complaints will be reviewed and investigated and will result in a response that
is deemed necessary and appropriate to the circumstances. The project team is
obligated to maintain confidentiality with regard to the reporter of an incident.
Further details of specific enforcement policies may be posted separately.

Project maintainers who do not follow or enforce the Code of Conduct in good
faith may face temporary or permanent repercussions as determined by other
members of the project's leadership.

### Attribution

This Code of Conduct is adapted from the [Contributor Covenant][homepage], version 1.4,
available at [http://contributor-covenant.org/version/1/4][version]

[homepage]: http://contributor-covenant.org
[version]: http://contributor-covenant.org/version/1/4/
