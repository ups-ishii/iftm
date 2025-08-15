# Issue-Focused TODO Management (IFTM) 概要・使い方

## 概要

**Issue-Focused TODO Management (IFTM)** は、GitHub Issues で管理している Issue に対して、Cursor での作業効率化のために一時的な TODO ファイルを作成・管理する手法です。Issue が完了すると、対応する TODO ファイルは手動または自動的に削除され、プロジェクトの整理を保ちます。

## 基本コンセプト

- **1 つの Issue = 1 つの TODO ファイル**
- **一時的な存在**: Issue 完了時に削除
- **作業分解**: 複雑な Issue を細かいステップに分解
- **Cursor 特化**: IDE での作業に最適化

## ファイル命名規則

```
project-root/              # プロジェクトルート
├── TODO-issue-123.md    # Issue #123 用
├── TODO-issue-124.md    # Issue #124 用
├── docs/                 # ドキュメント
├── src/                  # ソースコード
└── README.md
```

## 使い方

### 1. **Issue 作成時のセットアップ**

```bash
# 1. GitHubでIssue #123を作成
# 2. 対応するTODOファイルを作成（プロジェクトルートに）
touch TODO-issue-123.md
```

```markdown
# TODO-issue-123.md

## [Issue #123] スクリーンキャプチャ機能の改善

### 作業分解（Cursor 用）

- [ ] 1. 現在のコード分析
  - `src/screen_capture.py` の既存実装を確認
  - 問題箇所の特定
- [ ] 2. 画像品質向上の実装
  - 解像度設定の追加
  - 画像圧縮アルゴリズムの最適化
- [ ] 3. エラーハンドリング強化
  - 例外処理の改善
  - ログ出力の実装
- [ ] 4. テストの追加
  - 単体テストの作成
  - 品質テストの実装

### 関連ファイル

- `src/screen_capture.py`
- `src/image_processor.py`
- `tests/test_screen_capture.py`

### 参考情報

- Issue #123 の詳細仕様
- 関連する技術ドキュメント

### 進捗状況

- **開始日**: 2024-12-20
- **担当者**: @username
- **優先度**: 高
- **現在の作業**: コード分析中
```

### 2. **開発進行中の更新**

```bash
# 作業完了ごとにチェックボックスを更新
# コミット時に進捗も記録
git add TODO-issue-123.md
git add src/screen_capture.py
git commit -m "feat: 画像品質向上を実装 - Issue #123

- 解像度設定の追加完了
- TODO-issue-123.md の進捗を更新"
```

### 3. **Issue 完了時のクリーンアップ**

```bash
# 1. Issue #123 をクローズ
# 2. TODOファイルを削除
git rm TODO-issue-123.md
git commit -m "docs: Issue #123 完了 - TODOファイルを削除"
git push origin main
```

## 運用フロー

```mermaid
graph TD
    A[Issue作成] --> B[TODO-issue-{番号}.md作成]
    B --> C[作業分解・記述]
    C --> D[開発作業]
    D --> E[進捗更新]
    E --> F{作業完了?}
    F -->|No| D
    F -->|Yes| G[Issueクローズ]
    G --> H[TODOファイル削除]
    H --> I[完了]
```

## テンプレート

### **基本テンプレート**

```markdown
# TODO-template.md

## [Issue #{番号}] {タイトル}

### 作業分解（Cursor 用）

- [ ] 1. {作業 1}
  - {詳細 1}
  - {詳細 2}
- [ ] 2. {作業 2}
  - {詳細 1}
  - {詳細 2}

### 関連ファイル

- `{ファイルパス1}`
- `{ファイルパス2}`

### 参考情報

- {参考情報 1}
- {参考情報 2}

### 進捗状況

- **開始日**: {YYYY-MM-DD}
- **担当者**: @{ユーザー名}
- **優先度**: {高/中/低}
- **現在の作業**: {作業内容}
```

## 自動化の可能性

### **GitHub Actions での自動クリーンアップ**

```yaml
# .github/workflows/cleanup-todos.yml
name: Cleanup Completed Issue TODOs
on:
  issues:
    types: [closed]

jobs:
  cleanup:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Remove TODO file for closed issue
        run: |
          ISSUE_NUMBER=${{ github.event.issue.number }}
          TODO_FILE="TODO-issue-${ISSUE_NUMBER}.md"
          if [ -f "$TODO_FILE" ]; then
            git rm "$TODO_FILE"
            git commit -m "docs: Issue #${ISSUE_NUMBER} 完了 - TODOファイルを削除"
            git push
          fi
```

## メリット

✅ **Issue 単位での作業管理が明確**  
✅ **Cursor での作業効率が向上**  
✅ **完了時の自動クリーンアップ**  
✅ **チーム間での作業状況共有が容易**  
✅ **GitHub Issues との連携が自然**  
✅ **複雑な Issue の作業分解が可能**

## 注意点

⚠️ **ファイル数の管理**: アクティブな Issue が多い場合の整理  
⚠️ **同時編集の回避**: 1 人で管理するか、編集権限を制限  
⚠️ **定期的な整理**: 古い TODO ファイルの確認

## 適用場面

- **複雑な Issue の作業分解が必要**
- **Cursor での作業効率化を図りたい**
- **チームでの作業状況共有を重視**
- **Issue 完了後の整理を自動化したい**

## まとめ

IFTM を採用することで、Issue 駆動開発がより効率的になり、Cursor での作業体験も大幅に向上します。1 つの Issue に集中して作業でき、完了時の自動クリーンアップにより、プロジェクトの整理も保たれます。

---

_このドキュメントは Issue-Focused TODO Management (IFTM) の概要・使い方を説明しています。_
