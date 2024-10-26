[Repository Dispatch · Actions · GitHub Marketplace · GitHub](https://github.com/marketplace/actions/repository-dispatch)を使うことで、他のリポジトリへ`Repository dispatch`イベントを送ることができる。対象リポジトリでは`workflow`のトリガとして`repository_dispatch`で実行できる。

### イベント生成側
[パーソナルアクセストークン - GitHub Docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)を使ってPushトリガーなどで指定リポジトリへrepository-dispatchイベントを送信できます。
```yaml
      - name: Repository Dispatch
        uses: peter-evans/repository-dispatch@v3
        with:
          token: ${{ secrets.PAT }}
          repository: username/my-repo
          event-type: my-event
```

> [!info]
> 作成方法を含め下記がわかりやすい
> [【Git】personal access tokenを使用してGitHubへアクセスする #初心者向け - Qiita](https://qiita.com/YuukiYoshida/items/2e6b250d44bf1e0f5a0b)
### イベント受信側
指定イベント（`my-event`）でworkflowを起動する
```yaml
name: Repository Dispatch
on:
  repository_dispatch:
    types: [my-event]
jobs:
  myEvent:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          ref: ${{ github.event.client_payload.ref }}
      - run: echo ${{ github.event.client_payload.sha }}
```

## 応用
上記を組み合わせることで、ドキュメントのリポジトリへpushで別リポジトリのworkflowでdeployができるようになる。