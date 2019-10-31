# Release Drafter Config
binxhealth's Release-Drafter base config (based on https://github.com/jenkinsci/.github/)

[Release Drafter](https://github.com/toolmantim/release-drafter) is a tool and GitHub action which helps to automate 
management of releases notes. Basically, this tool generates changelog drafts using pull request metadata 
(commit headers, links, etc.) and then suggest a changelog draft in GitHub Releases.

## Usage

### Enabling Release Drafter in a repository

Create the file `.github/release-drafter.yml` in the master branch of your repository with the following content.

```yml
_extends: release-drafter-config
```

This will inherit the base release drafter config specific in this repository. All settings can be overridden in the 
repository's release-drafter.yml. You can write your own configuration from scratch if needed.
See the [Release Drafter Documentation](https://github.com/toolmantim/release-drafter/blob/master/README.md) for 
guidelines. If a change you need is a common use-case for all repositories, submit a pull request here.

Now create the Github Action file `.github/workflows/release-drafter.yml` in the master branch of your repository with 
the following content.

```yml
name: Release Drafter
on:
  push:
    branches:
      - master
jobs:
  update_draft_release:
    runs-on: ubuntu-latest
    steps:
      # Drafts your next Release notes as Pull Requests are merged into "master"
      - uses: toolmantim/release-drafter@v5.2.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Releasing a component with Release Drafter

1. Make sure that all pull requests in the release are properly labeled.
   See the [Global Configuration file](./.github/release-drafter.yml) for available labels
2. Go to `${YOUR_REPO}/releases` and click "Edit" on the draft release. 
3. Edit the tag, point it to a tag or a release commit created by the common release flow
* Tags represent a text field with auto-completion. Tag names can be also copy-pasted from `${YOUR_REPO}/tags`
* Recent commits can be selected from a dropdown
4. Edit the release name, if needed
5. Review and copy-edit the changelog
* First release draft will likely contain all history from the beginning of the repository,
   you will need to remove entries corresponding to PRs included in prior releases.
   If all new PRs are categorized, just delete everything before the first header.
* Release drafter is designed to add one entry per pull request.
   If a pull request includes multiple changes to be noted, manual editing will be needed
6. Click the _Publish_ button
