# tmux設定のインストール手順

## 前提条件

### tmuxのインストール

tmuxがインストールされていない場合は、以下のコマンドでインストールしてください。

#### Ubuntu/Debian
```bash
sudo apt update
sudo apt install tmux
```

#### macOS (Homebrew)
```bash
brew install tmux
```

#### CentOS/RHEL
```bash
sudo yum install tmux
# または
sudo dnf install tmux
```

### バージョン確認

```bash
tmux -V
```

**推奨バージョン**: tmux 3.0以降

## インストール手順

### 1. リポジトリのクローン

```bash
cd ~
git clone <repository-url> tmux-config
```

### 2. 既存設定のバックアップ

既存の設定がある場合は、念のためバックアップを作成します：

```bash
# 設定ファイルのバックアップ
if [ -f ~/.tmux.conf ]; then
    cp ~/.tmux.conf ~/.tmux.conf.backup.$(date +%Y%m%d_%H%M%S)
    echo "既存の ~/.tmux.conf をバックアップしました"
fi

# チートシートのバックアップ
if [ -f ~/.tmux-cheatsheet.txt ]; then
    cp ~/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt.backup.$(date +%Y%m%d_%H%M%S)
    echo "既存の ~/.tmux-cheatsheet.txt をバックアップしました"
fi
```

### 3. 設定ファイルの配置

```bash
# メイン設定ファイルをコピー
cp ~/tmux-config/configs/.tmux.conf ~/.tmux.conf

# チートシートをコピー
cp ~/tmux-config/configs/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt

# 設定ファイルのパーミッション設定
chmod 644 ~/.tmux.conf ~/.tmux-cheatsheet.txt
```

### 4. 設定の反映

#### 新しいセッションを開始する場合
```bash
tmux new-session -s main
```

#### 既存のセッションで設定を再読み込みする場合
```bash
# tmuxセッション内で実行
tmux source-file ~/.tmux.conf
```

## 設定の確認

### 1. プレフィックスキーの確認

新しいプレフィックスキーが `Ctrl+s` に変更されていることを確認：

```bash
# tmux内で実行
# Ctrl+s : と入力してコマンドモードに入る
# 'show-options -g prefix' と入力してEnter
```

### 2. マウス操作の確認

- マウスでペインのクリック選択ができること
- マウスホイールでスクロールできること
- ダブルクリックでレイアウトが変更されること

### 3. ヘルプの確認

```bash
# Ctrl+s / でヘルプが表示されることを確認
```

## トラブルシューティング

### 設定が反映されない場合

1. **tmuxサーバーの再起動**
   ```bash
   tmux kill-server
   tmux new-session
   ```

2. **設定ファイルの構文チェック**
   ```bash
   tmux source-file ~/.tmux.conf
   ```

3. **エラーメッセージの確認**
   ```bash
   tmux source-file ~/.tmux.conf 2>&1 | head -20
   ```

### マウス操作が効かない場合

1. **tmuxのバージョンを確認**
   ```bash
   tmux -V
   ```
   
2. **バージョンが古い場合はアップデート**
   ```bash
   # Ubuntu/Debian
   sudo apt update && sudo apt upgrade tmux
   
   # macOS
   brew upgrade tmux
   ```

### プレフィックスキーが変更されない場合

1. **他の設定ファイルとの競合をチェック**
   ```bash
   # システム全体の設定ファイルをチェック
   ls -la /etc/tmux.conf 2>/dev/null || echo "システム設定なし"
   
   # ホームディレクトリの隠しファイルをチェック
   ls -la ~/.tmux*
   ```

2. **設定の読み込み順序を確認**
   ```bash
   # tmux内で実行
   tmux show-options -g | grep prefix
   ```

## 高度な設定

### シンボリックリンクを使用した管理

設定ファイルを直接コピーする代わりに、シンボリックリンクを使用して管理することもできます：

```bash
# 既存ファイルを削除
rm ~/.tmux.conf ~/.tmux-cheatsheet.txt

# シンボリックリンクを作成
ln -s ~/tmux-config/configs/.tmux.conf ~/.tmux.conf
ln -s ~/tmux-config/configs/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt
```

**メリット**:
- 設定ファイルを直接編集できる
- Gitで変更履歴を管理できる
- 複数のマシンで同じ設定を共有しやすい

### 自動インストールスクリプト

```bash
#!/bin/bash
# install.sh - 自動インストールスクリプト

set -e

echo "tmux設定のインストールを開始します..."

# バックアップ作成
if [ -f ~/.tmux.conf ]; then
    cp ~/.tmux.conf ~/.tmux.conf.backup.$(date +%Y%m%d_%H%M%S)
    echo "既存の設定をバックアップしました"
fi

# 設定ファイルのコピー
cp configs/.tmux.conf ~/.tmux.conf
cp configs/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt

# パーミッション設定
chmod 644 ~/.tmux.conf ~/.tmux-cheatsheet.txt

echo "インストールが完了しました！"
echo "新しいtmuxセッションを開始するか、既存のセッションで 'tmux source-file ~/.tmux.conf' を実行してください。"
```

## 次のステップ

1. [キーバインド一覧](keybindings.md) を確認
2. [カスタマイズガイド](customization.md) を読んで、自分好みに調整
3. 日常的に使用して、設定に慣れる