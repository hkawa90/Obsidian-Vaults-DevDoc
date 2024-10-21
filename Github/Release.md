GitHub上で開発したソフトウェアをそのままGitHub上で配布したい場合に使います。リリースノートとソースコードのアーカイブが添付されます。

以下のRelease drafterを使うとpull requestと連動して、リリースノートのドラフト版を作成してくれます。
## Release Drafter
インストールは特段必要なく、下記設定ファイルをリポジトリ内に配置するだけです。

pull request が main にマージされるときに、次のリリース ノートの下書きを作成します。
- Pull request時のラベル(major/minor/patch)により次のバージョンを決定します。
- Pull request時のラベル(categoriesで指定)によってリリースノートを分類します。

```yaml title=".github/workflows/release-drafter.yml"
name: Release Drafter

on:
  push:
    # branches to consider in the event; optional, defaults to all
    branches:
      - main
  # pull_request event is required only for autolabeler
  pull_request:
    # Only following types are handled by the action, but one can default to all as well
    types: [opened, reopened, synchronize]
  # pull_request_target event is required for autolabeler to support PRs from forks
  # pull_request_target:
  #   types: [opened, reopened, synchronize]
  workflow_dispatch:

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

設定ファイルを`.github/release-drafter.yml`に作成しておく。
- tag-templateはバージョン番号のタグ名のテンプレートとなるのでバージョン番号自動付与に必要
- name-templateはリリースタイトルのテンプレートとなるので、リリースノートのドラフトのリリース名自動付与に必要

```yaml title=".github/release-drafter.yml"
name-template: 'v$RESOLVED_VERSION 🌈'
tag-template: 'v$RESOLVED_VERSION'
categories:
  - title: '🚀 Features'
    labels:
      - 'feature'
      - 'enhancement'
  - title: '🐛 Bug Fixes'
    labels:
      - 'fix'
      - 'bugfix'
      - 'bug'
      - 'security:fix'
  - title: '🧰 Maintenance'
    label: 'chore'
change-template: '- $TITLE @$AUTHOR (#$NUMBER)'
version-resolver:
  major:
    labels:
      - 'major'
  minor:
    labels:
      - 'minor'
  patch:
    labels:
      - 'patch'
  default: patch

template: |
  ## What’s Changed

  $CHANGES
```

![[Pasted image 20241020194747.png]]
次のプルリクエストに基づき、変更内容が追加される。
![[Pasted image 20241021110610.png]]
## 参考
- [github actionsでリリースにかかる付帯作業の自動化を行なう #GitHub - Qiita](https://qiita.com/itouoti/items/85f2cf796d1fb3217ca7)
- [GitHub のリリースノート自動生成機能を使う | 豆蔵デベロッパーサイト](https://developer.mamezou-tech.com/blogs/2022/03/11/github-automatically-generated-release-notes/)
- [release-drafterでリリースノートを自動で作る | 60%プロダクト開発ブログ](https://www.wantedly.com/companies/company_5381786/post_articles/458418)