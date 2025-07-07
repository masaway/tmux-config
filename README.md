# tmux Configuration Repository

## 🚀 クイックスタート

```bash
# 設定ファイルをコピー
cp configs/.tmux.conf ~/.tmux.conf
cp configs/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt

# WSL/Windows Terminal用IME最適化設定をコピー
mkdir -p ~/.config/tmux
cp configs/wsl-ime-fixes.conf ~/.config/tmux/

# 設定を反映
tmux source-file ~/.tmux.conf
```

## 📖 ヘルプ

tmux内で `Ctrl+s /` でヘルプをポップアップ表示できます。

**主なキーバインド:**
- プレフィックスキー: `Ctrl+s` (デフォルトの `Ctrl+b` から変更)
- ヘルプ表示: `Ctrl+s /`
- 設定リロード: `Ctrl+s r`
- 画面クリア（IME状態リセット）: `Ctrl+s Ctrl+l`

**特別機能:**
- **WSL/Windows Terminal IME最適化**: 日本語入力時の表示問題を自動で軽減
- **自動環境検出**: WSL環境を自動検出して最適化設定を適用