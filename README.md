# tmux Configuration Repository

個人のtmux設定ファイルとドキュメントを管理するリポジトリです。

## 📁 ディレクトリ構成

```
tmux-config/
├── README.md                    # このファイル
├── configs/
│   ├── .tmux.conf              # メインの設定ファイル
│   └── .tmux-cheatsheet.txt    # チートシート
├── examples/
│   ├── basic.conf              # 基本設定例
│   ├── advanced.conf           # 高度な設定例
│   └── minimal.conf            # 最小限の設定
└── docs/
    ├── installation.md         # インストール手順
    ├── customization.md        # カスタマイズガイド
    └── keybindings.md          # キーバインド一覧
```

## 🚀 クイックスタート

### 1. 設定ファイルのインストール

```bash
# リポジトリをクローン
git clone <repository-url> ~/tmux-config

# 既存の設定をバックアップ（存在する場合）
cp ~/.tmux.conf ~/.tmux.conf.backup 2>/dev/null || true
cp ~/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt.backup 2>/dev/null || true

# 設定ファイルをホームディレクトリにコピー
cp ~/tmux-config/configs/.tmux.conf ~/.tmux.conf
cp ~/tmux-config/configs/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt

# 設定を反映
tmux source-file ~/.tmux.conf
```

### 2. 基本的な使用方法

```bash
# 新しいセッションを開始
tmux new-session -s work

# 既存のセッションにアタッチ
tmux attach-session -t work

# セッション一覧を確認
tmux list-sessions
```

## ⚙️ 主な設定内容

### プレフィックスキー
- デフォルトの `Ctrl+b` を `Ctrl+s` に変更
- より使いやすい位置に配置

### マウス操作
- マウス操作を有効化
- ダブルクリックでタイルレイアウトに変更
- マウスホイールでスクロール（通常/高速）

### ペイン管理
- Vimライクなキーバインド（h/j/k/l）
- 直感的な分割操作（`\` で水平分割、`-` で垂直分割）
- 視覚的なペインの境界線

### スクロール改善
- 履歴を50,000行に拡張
- 滑らかなスクロール機能
- Shift+マウスホイールで高速スクロール

### ステータスバー
- 上部配置で見やすく
- セッション名、パス、Gitブランチを表示
- アクティブウィンドウの強調表示

## 🎯 キーバインド一覧

### 基本操作
| キー | 動作 |
|------|------|
| `Ctrl+s d` | セッションからデタッチ |
| `Ctrl+s c` | 新しいウィンドウを作成 |
| `Ctrl+s \` | ペインを水平分割 |
| `Ctrl+s -` | ペインを垂直分割 |
| `Ctrl+s h/j/k/l` | ペイン間の移動（Vim風） |
| `Ctrl+s H/J/K/L` | ペインのリサイズ |
| `Ctrl+s /` | ヘルプ表示 |
| `Ctrl+s r` | 設定をリロード |

詳細なキーバインドは [docs/keybindings.md](docs/keybindings.md) を参照してください。

## 📚 ドキュメント

- [インストール手順](docs/installation.md) - 詳細なセットアップ手順
- [カスタマイズガイド](docs/customization.md) - 設定のカスタマイズ方法
- [キーバインド一覧](docs/keybindings.md) - 全キーバインドの説明

## 🔧 カスタマイズ

### 設定例の使用

```bash
# 基本設定を使用
cp examples/basic.conf ~/.tmux.conf

# 高度な設定を使用
cp examples/advanced.conf ~/.tmux.conf

# 最小限の設定を使用
cp examples/minimal.conf ~/.tmux.conf
```

### 自分好みにカスタマイズ

1. `~/.tmux.conf` を編集
2. `tmux source-file ~/.tmux.conf` で設定を反映
3. 必要に応じて `~/.tmux-cheatsheet.txt` も更新

## 🐛 トラブルシューティング

### 設定が反映されない場合

```bash
# tmuxサーバーを再起動
tmux kill-server
tmux new-session

# 設定ファイルを手動で再読み込み
tmux source-file ~/.tmux.conf
```

### マウス操作が効かない場合

```bash
# tmuxのバージョンを確認
tmux -V

# バージョンが古い場合はアップデート
# Ubuntu/Debian: sudo apt update && sudo apt upgrade tmux
# macOS: brew upgrade tmux
```

## 📋 動作環境

- tmux 3.0以降
- 対応OS: Linux, macOS, WSL
- 推奨ターミナル: iTerm2, Windows Terminal, GNOME Terminal

## 🤝 貢献

改善提案やバグ報告は Issue または Pull Request でお願いします。

## 📄 ライセンス

このリポジトリの内容は個人利用・学習目的で自由に使用できます。

---

**注意**: このリポジトリの設定は個人の好みに基づいています。使用前に内容を確認し、必要に応じて調整してください。