# tmux カスタマイズガイド

## 基本的なカスタマイズ

### 設定ファイルの場所

- メイン設定: `~/.tmux.conf`
- チートシート: `~/.tmux-cheatsheet.txt`

### 設定の反映方法

```bash
# 設定ファイルを編集後、設定を反映
tmux source-file ~/.tmux.conf

# または、tmux内で
Ctrl+s r
```

## プレフィックスキーの変更

現在の設定では `Ctrl+s` を使用していますが、他のキーに変更することも可能です。

```bash
# Ctrl+a に変更する場合
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# Ctrl+z に変更する場合
set -g prefix C-z
unbind C-b
bind C-z send-prefix
```

## 色とテーマの カスタマイズ

### ステータスバーの色

```bash
# ステータスバーの背景色を変更
set -g status-bg colour235

# ステータスバーの文字色を変更
set -g status-fg colour255

# アクティブウィンドウの背景色
set -g window-status-current-bg colour166

# アクティブウィンドウの文字色
set -g window-status-current-fg colour15
```

### ペインの境界線

```bash
# 境界線の色を変更
set -g pane-border-style fg=colour240,bg=default
set -g pane-active-border-style fg=colour166,bg=default

# 境界線のスタイル
set -g pane-border-lines heavy    # heavy, single, double, simple
```

### カラーテーマの例

#### ダークテーマ

```bash
# 背景色を暗めに
set -g status-bg colour234
set -g pane-border-style fg=colour238
set -g pane-active-border-style fg=colour208

# ウィンドウの背景を暗めに
set -g window-style 'fg=colour247,bg=colour236'
set -g window-active-style 'fg=terminal,bg=terminal'
```

#### ライトテーマ

```bash
# 背景色を明るめに
set -g status-bg colour255
set -g status-fg colour232
set -g pane-border-style fg=colour252
set -g pane-active-border-style fg=colour33

# ウィンドウの背景を明るめに
set -g window-style 'fg=colour236,bg=colour253'
set -g window-active-style 'fg=terminal,bg=terminal'
```

## キーバインドのカスタマイズ

### 新しいキーバインドを追加

```bash
# Ctrl+s + e でペインを左右に分割
bind e split-window -h

# Ctrl+s + v でペインを上下に分割
bind v split-window -v

# Ctrl+s + R で設定をリロード（大文字のR）
bind R source-file ~/.tmux.conf \; display-message "Config reloaded!"
```

### 既存のキーバインドを変更

```bash
# デフォルトの % と " を無効化
unbind %
unbind '"'

# 新しい分割キーを設定
bind | split-window -h
bind - split-window -v
```

### マウス操作のカスタマイズ

```bash
# マウスドラッグでペインを選択しない
unbind -n MouseDrag1Pane

# 右クリックでメニューを表示
bind -n MouseDown3Pane display-menu -t = -x M -y M \
    "Split Horizontally" h "split-window -h" \
    "Split Vertically" v "split-window -v" \
    "Kill Pane" k "kill-pane"
```

## ステータスバーのカスタマイズ

### 現在の設定

```bash
# 上部に配置
set -g status-position top

# 左側の表示
set -g status-left '#[fg=white,bold]S: #S'

# 右側の表示
set -g status-right "#[fg=white,bold]#(echo '#{pane_current_path}' | sed 's|^/Users/username|~|') #[fg=yellow,bold](#(cd '#{pane_current_path}' && git rev-parse --abbrev-ref HEAD 2>/dev/null || echo '')) #[default]"
```

### カスタマイズ例

#### 詳細な情報を表示

```bash
# CPUとメモリ使用率を表示
set -g status-right-length 200
set -g status-right "#[fg=cyan]CPU: #(top -bn1 | grep 'Cpu(s)' | cut -d',' -f1 | cut -d':' -f2 | xargs)#[default] | #[fg=green]MEM: #(free -m | awk 'NR==2{printf \"%.1f%%\", $3*100/$2}')#[default] | #[fg=yellow]%Y-%m-%d %H:%M#[default]"
```

#### シンプルな表示

```bash
# 時間のみ表示
set -g status-right "#[fg=white,bold]%H:%M#[default]"

# 日付と時間
set -g status-right "#[fg=white,bold]%Y-%m-%d %H:%M#[default]"
```

#### バッテリー情報（macOS）

```bash
# バッテリー残量を表示
set -g status-right "#[fg=green]Battery: #(pmset -g batt | grep -Eo '[0-9]+%')#[default] | #[fg=white,bold]%H:%M#[default]"
```

### ウィンドウ名の表示

```bash
# 現在のコマンドを表示
setw -g window-status-format ' #I:#W#F '
setw -g window-status-current-format '#[fg=black,bg=yellow,bold] #I:#W#F #[default]'

# 自動的にウィンドウ名を更新
setw -g automatic-rename on
```

## 高度なカスタマイズ

### プラグインの使用

tmux Plugin Manager (TPM) を使用してプラグインを管理できます。

```bash
# TPMのインストール
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

# ~/.tmux.confに追加
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

# TPMを初期化
run '~/.tmux/plugins/tpm/tpm'
```

### 便利なプラグイン

#### tmux-resurrect（セッション保存）

```bash
set -g @plugin 'tmux-plugins/tmux-resurrect'

# Vimのセッションも保存
set -g @resurrect-strategy-vim 'session'

# プロセスを保存
set -g @resurrect-processes 'ssh mysql psql'
```

#### tmux-continuum（自動保存）

```bash
set -g @plugin 'tmux-plugins/tmux-continuum'

# 15分ごとに自動保存
set -g @continuum-restore 'on'
set -g @continuum-save-interval '15'
```

#### tmux-prefix-highlight（プレフィックス表示）

```bash
set -g @plugin 'tmux-plugins/tmux-prefix-highlight'

# ステータスバーに表示
set -g status-right '#{prefix_highlight} | %a %Y-%m-%d %H:%M'
```

### 条件分岐設定

```bash
# macOSの場合のみ適用
if-shell 'uname | grep -q Darwin' 'set -g default-command "reattach-to-user-namespace -l $SHELL"'

# tmuxのバージョンによる分岐
if-shell '[ $(echo "$(tmux -V | cut -d" " -f2 | tr -d "a-z") >= 2.1" | bc) -eq 1 ]' \
    'set -g mouse on' \
    'set -g mode-mouse on; set -g mouse-resize-pane on; set -g mouse-select-pane on; set -g mouse-select-window on'
```

### 環境変数の設定

```bash
# 環境変数を設定
set-environment -g 'SSH_AUTH_SOCK' ~/.ssh/ssh_auth_sock

# 全てのペインで環境変数を更新
set -g update-environment -r
```

## 用途別の設定例

### 開発者向け設定

```bash
# 開発用のキーバインド
bind-key C-t new-window -c "#{pane_current_path}" \; rename-window "test"
bind-key C-l new-window -c "#{pane_current_path}" \; rename-window "logs"

# Gitブランチを常に表示
set -g status-right "#[fg=yellow,bold](#(cd '#{pane_current_path}' && git rev-parse --abbrev-ref HEAD 2>/dev/null || echo 'no-git')) #[default]| #[fg=white,bold]%H:%M#[default]"

# 長時間実行されるコマンドを監視
set -g monitor-activity on
set -g visual-activity on
```

### システム管理者向け設定

```bash
# ログ監視用のペイン作成
bind-key L split-window -h "tail -f /var/log/syslog"

# システム情報を表示
set -g status-right "#[fg=red]Load: #(uptime | cut -d',' -f3- | cut -d':' -f2)#[default] | #[fg=white,bold]%H:%M#[default]"

# 警告色を強調
set -g message-style fg=white,bg=red,bold
```

### プレゼンテーション用設定

```bash
# 大きなフォントとシンプルな表示
set -g status-left-length 10
set -g status-right-length 10
set -g status-left "#S"
set -g status-right "%H:%M"

# 境界線を太く
set -g pane-border-lines heavy
set -g pane-active-border-style fg=red,bold
```

## パフォーマンスの最適化

### 描画の最適化

```bash
# 描画を高速化
set -g escape-time 10
set -g repeat-time 1000

# 履歴を制限
set -g history-limit 10000

# 不要なオプションを無効化
set -g monitor-activity off
set -g visual-activity off
```

### メモリ使用量の最適化

```bash
# バッファサイズを制限
set -g buffer-limit 20

# 自動的にウィンドウを破棄
set -g destroy-unattached on
```

## 設定のバックアップと復元

### 設定をエクスポート

```bash
# 現在の設定をファイルに保存
tmux show-options -g > tmux-global-options.conf
tmux show-options -w > tmux-window-options.conf
tmux list-keys > tmux-keybindings.conf
```

### 設定をインポート

```bash
# 設定ファイルから読み込み
tmux source-file tmux-global-options.conf
tmux source-file tmux-window-options.conf
```

## よくある問題と解決方法

### 文字化けする場合

```bash
# UTF-8を強制
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",*256col*:Tc"
```

### 色が正しく表示されない場合

```bash
# 256色サポートを有効化
set -g default-terminal "tmux-256color"
set -ga terminal-overrides ",xterm-256color:Tc"
```

### キーバインドが効かない場合

```bash
# 既存のバインドを確認
tmux list-keys | grep <key>

# 明示的にバインドを削除
unbind <key>
bind <key> <command>
```

## 参考になるリソース

- [tmux公式Wiki](https://github.com/tmux/tmux/wiki)
- [Awesome tmux](https://github.com/rothgar/awesome-tmux)
- [tmux Plugin Manager](https://github.com/tmux-plugins/tpm)
- [tmux Cheat Sheet](https://tmuxcheatsheet.com/)

## 設定の共有

設定をGitで管理し、複数のマシンで同じ設定を使用する方法：

```bash
# dotfilesリポジトリを作成
cd ~
git init dotfiles
cd dotfiles

# 設定ファイルを追加
cp ~/.tmux.conf .
cp ~/.tmux-cheatsheet.txt .

# Gitで管理
git add .
git commit -m "Initial tmux configuration"
git remote add origin <your-repository-url>
git push -u origin main
```

別のマシンでの設定：

```bash
# リポジトリをクローン
git clone <your-repository-url> ~/dotfiles

# シンボリックリンクを作成
ln -s ~/dotfiles/.tmux.conf ~/.tmux.conf
ln -s ~/dotfiles/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt
```