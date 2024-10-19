
## Release Drafter

```yaml title=".github/workflows/release-drafter.yml"
name: Release Drafter

on:
  push:
    # branches to consider in the event; optional, defaults to all
    branches:
      - master
  # pull_request event is required only for autolabeler
  pull_request:
    # Only following types are handled by the action, but one can default to all as well
    types: [opened, reopened, synchronize]
  # pull_request_target event is required for autolabeler to support PRs from forks
  # pull_request_target:
  #   types: [opened, reopened, synchronize]
  workflow_dispatch

permissions:
  contents: read

jobs:
  update_release_draft:
    permissions:
      # write permission is required to create a github release
      contents: write
      # write permission is required for autolabeler
      # otherwise, read permission is required at least
      pull-requests: write
    runs-on: ubuntu-latest
    steps:
      # (Optional) GitHub Enterprise requires GHE_HOST variable set
      #- name: Set GHE_HOST
      #  run: |
      #    echo "GHE_HOST=${GITHUB_SERVER_URL##https:\/\/}" >> $GITHUB_ENV

      # Drafts your next Release notes as Pull Requests are merged into "master"
      - uses: release-drafter/release-drafter@v6
        # (Optional) specify config name to use, relative to .github/. Default: release-drafter.yml
        # with:
        #   config-name: my-config.yml
        #   disable-autolabeler: true
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 参考
- [github actionsでリリースにかかる付帯作業の自動化を行なう #GitHub - Qiita](https://qiita.com/itouoti/items/85f2cf796d1fb3217ca7)
- [GitHub のリリースノート自動生成機能を使う | 豆蔵デベロッパーサイト](https://developer.mamezou-tech.com/blogs/2022/03/11/github-automatically-generated-release-notes/)