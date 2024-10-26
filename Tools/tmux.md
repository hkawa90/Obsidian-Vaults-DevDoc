tmuxはターミナル・マルチプレクサであり、1つのスクリーンから多数のターミナルを作成し、アクセスし、制御することができる。

```sh
brew install tmux
```

## Configuration
`.tmux.conf`で設定する. 
TODO: status lineにgit の情報を表示するようにする
```
# prefixキーをC-aに変更する
set -g prefix C-a

# デフォルトのprefixキーC-bを解除する
unbind C-b

# 256色モードを有効にする
set-option -g default-terminal screen-256color
set -g terminal-overrides 'xterm:colors=256'

# ステータスラインの色を変更
setw -g status-style fg=colour255,bg=colour234

# マウス操作を有効にする
set-option -g mouse on

```