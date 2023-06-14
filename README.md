<!--
theme: gaia
class:
 - invert
headingDivider: 2 
paginate: true
-->

<!--
_class:
 - lead
 - invert
-->

# [Journey of Automation - Github, Galaxy, Fedora](https://spetrosi.github.io/release_automation_devconf2023)

[Pavel Cahyna](mailto:pcahyna@redhat.com), [Sergei Petrosian](mailto:spetrosi@redhat.com)

## Automation is more important than ever before in software project management

<!-- Being able to automate the low level, labor intensive parts of project management is critical. There are many tools in the Fedora and Github ecosystems that facilitate project management, such as GitHub workflows, Packit, and more. -->

Learn how the Linux System Roles(url) team leverages automation:
1. Automated Ansible role release and publish to Ansible Galaxy
<!-- 2. Automated Ansible collection build, publish and release to Galaxy -->
2. Automated Fedora RPM build and publish with Packit
<!--
Comments for the slide for the presenters
For slies syntax examples use https://github.com/ralexander-phi/marp-to-pages/blob/main/README.md and https://github.com/spetrosi/jak_psat_moderni_ucebnice/blob/dev/README.md
-->

## Automated GitHub Releases

1. A script executed manually to create a new tag in GitHub repository
    a. Identify a new semantic version
    b. Generate changelog using conventional commits
    c. Create a PR with updated changelog
2. GitHub workflow that tags and releases GitHub repository once the changelog PR is merged
3. GitHub workflow that publishes repository into an upstream hub for a new release
<!-- 3. Cron-like daily GitHub workflow that collects and publishes content from multiple repositories if any repository has an update -->

## Changelog Generation: Conventional Commits Format

![](img/semver.jpg)

Format:
`<type><!>: PR Title`
Example:
*feat: Support custom data and logs storage paths*

## Figure out the new semantic version

- **!** - **MAJOR** bump
- `feat` - **MINOR** bump
- `fix`, `ci`, `test`,… - **PATCH** bump

## Build changelog based on PR types

- `feat:` -> **New Features**
- `fix:` -> **Bug Fixes**
- else -> **Other Changes**

## GitHub Release Process using Conventional PR Titles

1. A developer runs the script
2. This script collect merged PRs since last release
3. Using conventional commits format, identifies version and generates a new changelog
4. Pushes a PR with the updated changelog

## Creating Releases

PR merge triggers workflow that creates GitHub tag and release, and publishes repository content to Ansible Galaxy

```yaml
name: Tag, release, and publish role based on CHANGELOG.md push
on:
  push:
    branches:
      - main
    paths:
      - CHANGELOG.md
jobs:
      - name: Create tag...
      - name: Create Release...
      - name: Publish role to Galaxy...
```

## Automated RPM Release with Packit

