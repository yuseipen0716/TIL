# Claude Code　setup

## アカウント切り替え
まずは、下記のようにそれぞれのアカウントでログインを済ませておく。

```shell
# 私用
CLAUDE_CONFIG_DIR=~/.claude-personal claude

# 職場用
CLAUDE_CONFIG_DIR=~/.claude-work claude
```

以下を`~/.zshrc`に記載
```
# Claude Code アカウント切り替え設定
export CLAUDE_CONFIG_DIR=~/.claude-personal
alias claude-personal='CLAUDE_CONFIG_DIR=~/.claude-personal command claude'
alias claude-work='CLAUDE_CONFIG_DIR=~/.claude-work command claude'

# claude 単体を上書き
claude() {
  echo "⚠️  claude を直接実行することはできません。"
  echo "   個人用: personal-claude"
  echo "   会社用: work-claude"
  return 1
}
```

```
source ~/.zshrc
```
