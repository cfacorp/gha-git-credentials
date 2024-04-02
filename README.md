# Git Credentials for GitHub workflows

[GitHub Action](https://github.com/features/actions) for setting up
git credentials.

This action can be useful when workflow provides for creating commits
(e.g. when publishing content) and/or pushing commits to remote repos.
[Action on the marketplace](https://github.com/marketplace/actions/configure-git-credentials).

Default values would work for the most cases.
However **token** parameter must be passed to the action explicitly.

GitHub requires the use of
a [Personal Access Token](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)
assigned to the calling workflow
[secrets](https://help.github.com/en/articles/virtual-environments-for-github-actions#creating-and-using-secrets-encrypted-variables)
for pushing to repositories other than the repo the workflow is triggered for.

Pushing to the current repository should work
with the always-available GitHub Token `{{ secrets.GITHUB_TOKEN }}`.

You may also want to override default git username and email.

Actor would also be overridden when pushing to a repo cloud other than GitHub.

This project deployment workflow uses very this action to build its own distribution package.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Inputs](#inputs)
- [Outputs](#outputs)
- [Usage Example](#usage-example)
- [Versions](#versions)
  - [What version to use?](#what-version-to-use)
  - [v2.1.1](#v211)
  - [v2.1.0](#v210)
  - [v2](#v2)
  - [v1](#v1)
- [Action Users](#action-users)
- [License](#license)
- [No affiliation with GitHub Inc.](#no-affiliation-with-github-inc)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- generated with [DocToc](https://github.com/thlorenz/doctoc) -->

## Inputs

| property name | value type | default value                              | description                                                                  |
|---------------|------------|--------------------------------------------|------------------------------------------------------------------------------|
| `global`      | boolean    | `false`                                    | global git config used to assign git user name, email and password when true |
| `name`        | string     | `"GitHub Action"`                          | value for `git config user.name`                                             |
| `email`       | string     | `"github-action@users.noreply.github.com"` | value for `git config user.email`                                            |
| `actor`       | string     | `github.actor`                             | value used to construct `GIT_USER`                                           |
| `token`       | string     | **n/a**, explicit value required           | **required** value for `git user.password` and `GIT_USER`                    |

The minimally required action configuration requires a token being explicitly specified.
Example below uses secrets.GITHUB_TOKEN available to the workflow as a token source.

```
- uses: cfacorp/gha-git-credentials@v2-latest
  with:
    token: '${{ secrets.GITHUB_TOKEN }}'
```

## Outputs

No outputs produced.

## Usage Example

```yaml
name: Deployment
on:
  push:
    branches:
      - master
jobs:
  publish:
    - uses: actions/checkout@v2
    # Publish to a branch in current repo using GITHUB_TOKEN and other default settings.
    - uses: cfacorp/gha-git-credentials@v2-latest
      with:
        token: '${{ secrets.GITHUB_TOKEN }}'
    - run: |
        yarn run build
        yarn run deploy
    # Publish to a branch in a different repo
    # using a PAT generated on that other repo.
    # Option `global` is set to true as the deployment script may create 
    # a temporary local repo for a build and we want it to reuse git user settings.
    # Option `actor` is assigned as per that different repo cloud user.
    - uses: cfacorp/gha-git-credentials@v2.1.1
      with:
        global: true
        name: 'Oleksiy Rudenko'
        email: 'oleksiy.rudenko@domain.com'
        actor: 'cfacorp'
        token: '${{ secrets.GL_PAT_WEB_CENTRAL }}'
    - run: |
        git remote add web-central https://gitlab.com/some-organization/website.git
        yarn run deploy web-central/master
```

You may want to set the `global` option true when committing
to multiple local repositories during the workflow run is anticipated.

> Note that the second use of `gha-git-credentials` refers to a specific
> version of the action (`@v2.1.1`). In fact `@v2-latest` would work here quite fine
> and is a better approach.

Check [What version to use?](#what-version-to-use) to choose proper
action version reference.

## Versions

### What version to use?

In workflow `.yml` file you are required to specify action version.
E.g. `uses: cfacorp/gha-git-credentials@v2`.
`@v2` here refers to a very specific version of the action script.

So, in this action the options are:
- `vX`, `vX.Y`, `vX.Y.Z` - specific version
- `vX-latest` - latest release within the major version
  (no breaking changes, backward compatibility)
- `latest` - latest release, major version change may occur
- `<branch-name>` - version as per given branch name, useful for testing
- `rc` - release candidate. Used for pre-prod testing within
  the [release process](./CONTRIBUTING.md#release-process).

Whenever non-breaking changes introduced, having backward compatibility secured,
a patch version is released and the relevant `vX-latest` tag is moved to point
at the latest release within current major version.

Using `vX-latest` is a recommended choice.

Below are the key features of the releases.
Check [CHANGELOG](./CHANGELOG.md) for details.

<details><summary>Notes on version tagging</summary>

**Q: What I would do differently?**

A: I would tag every single release with a full
semantic version notation
(i.e. `vX.Y.Z`, e.g. `v2.0.0`, `v2.0.1`, `v2.1.0`, `v2.1.3`, `v2.2.5` etc).

Then, taking the above examples into consideration,
extra tags would work as follows:
- `v2` points at the same commit `v2.2.5` does 
  (the latest release within `v2` scope). 
  Currently `v2` points at `v2.0.0` serving as a shorthand
  for the full semantic notation.
- `v2.1` points at the same commit `v2.1.3` does
  (the latest release within `v2.1` scope).
  Currently `v2.1` points exactly at `v2.1.0` serving as a shorthand
  for the full semantic notation.

**Q: Why not change the approach now?**

A: Current [action users](#action-users) may rely 
on current tagging approach. Changing version handling
and tagging approach may break their workflows
</details>

### v2.1.1

Configure action to use NodeJS v16 instead of v12.
[Reason](https://github.blog/changelog/2022-09-22-github-actions-all-actions-will-begin-running-on-node16-instead-of-node12/).

### v2.1.0

Introduce `global` option to configure global git user name, email and password.
Default is `false` for backward compatibility.

### v2
Changed the way `GIT_USER` env var is being assigned.

Reason:
[GitHub deprecated `add-path` and `set-env` commands](https://github.blog/changelog/2020-10-01-github-actions-deprecating-set-env-and-add-path-commands/).

### v1
Features:
- Configurable git user name, email and password for commits created in a course of the workflow
- Configurable actor and token (GIT_USER) to push updates from a workflow

## Action Users

As of November 06, 2022, [762 projects on GitHub depend on this GitHub action](https://github.com/cfacorp/gha-git-credentials/network/dependents).

**10 top rated**

| url                                                     | stars   |
|---------------------------------------------------------|---------|
| https://github.com/yewstack/yew                         | 25K     |
| https://github.com/lingui/js-lingui                     | 3.5K    |
| https://github.com/wickedest/Mergely                    | 1.0K    |
| https://github.com/CuyZ/Valinor                         | 725     |
| https://github.com/snowplow/snowplow-javascript-tracker | 495     |
| https://github.com/themoonisacheese/2bored2wait         | 368     |
| https://github.com/Lightning-AI/tutorials               | 171     |
| https://github.com/diegoazh/gmap-vue                    | 140     |
| https://github.com/Umkus/ip-index                       | 134     |
| https://github.com/cookiejar/cookietemple               | 128     |

_Stats built with [ghtopdep](https://github.com/github-tooling/ghtopdep)_

## License

Scripts and documentation in this project are released under the [MIT license](LICENSE).

## No affiliation with GitHub Inc.

GitHub are registered trademarks of GitHub, Inc.
GitHub name used in this project are for identification purposes only.
The project is not associated in any way with GitHub Inc.
and is not an official solution of GitHub Inc.
It was made available in order to facilitate the use of the site GitHub.
