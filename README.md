# ai-dotfiles

Claude Code 向けの個人用 APM (Agent Package Manager) プロジェクト。
`presets/` から用途別に一括 install できる。

## 構成

```
.
├── agents/        # サブエージェント定義
├── skills/        # スキル (SKILL.md ベース)
├── prompts/       # 再利用プロンプト / スラッシュコマンド
├── instructions/  # 常時適用インストラクション
├── presets/       # 用途別プリセット (apm.yml)
│   ├── daily/         # 日常業務向け
│   └── develop/       # 開発業務向け
└── apm.yml        # ルート定義
```

## agents

| Name | 用途 |
| --- | --- |
| `architect` | システム設計・スケーラビリティ・技術的意思決定の専門家。新機能計画や大規模リファクタ時に使用。 |
| `build-error-resolver` | ビルド/TypeScript エラーの最小差分修正に特化。アーキテクチャ変更はせずグリーン化に集中。 |
| `code-reviewer` | 品質・セキュリティ・メンテナンス性のレビュー専門。コード変更直後に必須で使う。 |
| `doc-updater` | コードベースの実態に合わせて `docs/CODEMAPS/*` / README / アーキ Doc を更新する。 |
| `orchestrator` | 複雑なワークフローを順次ステップへ分解し、各ステップ内で並列サブタスクを実行する。 |
| `planner` | 機能実装・リファクタリングなどに対して詳細な実装計画を一括生成する計画立案スペシャリスト。 |
| `refactor-cleaner` | knip / depcheck / ts-prune などでデッドコードを特定し、安全に削除する。 |
| `security-reviewer` | シークレット / SSRF / インジェクション / 暗号脆弱性 / OWASP Top 10 を検出する。 |
| `tech-docs-searcher` | 最新ドキュメント・ベストプラクティスの調査をオフロードしメインのトークンを節約する。 |

## skills

| Name | 用途 |
| --- | --- |
| `docs-review` | Metabase ライティングスタイルガイド準拠でドキュメント差分をレビューする。 |
| `docs-write` | Metabase スタイル (会話的 / 明快 / ユーザー視点) でドキュメントを執筆・編集する。 |
| `empirical-prompt-tuning` | 中立な executor に実走させ、両面評価しながら頭打ちまで反復してプロンプト/スキルを改善する方法論。 |
| `eval-harness` | 機能の期待振る舞いを `.claude/evals/<feature>.md` に文書化し、pass@k / pass^k で追跡する評価フレームワーク。 |
| `gws-calendar` | `gws` CLI で Google カレンダーの予定・カレンダーを操作する。 |
| `hono` | Hono Web アプリ構築の API/ルーティング/ミドルウェア/JSX/バリデーション/テスト/ストリーミング知識。 |
| `image-compressor` | `cwebp` / `pngquant` / `jpegoptim` を用いた画像変換・圧縮 (WebP 化、PNG/JPEG 最適化)。 |
| `iterative-retrieval` | サブエージェントへ渡すコンテキストを広いクエリから絞り込む 4 フェーズ手順。3〜5 ファイルへ収束させる。 |
| `playwright-cli` | Playwright CLI でブラウザ操作・テスト・コード生成を行う。 |
| `security-review` | OWASP 10 領域チェックリスト + TS/Next.js/Supabase コード例。認証・入力・シークレット等の実装時に使用。 |
| `strategic-compact` | ツールコール閾値で `/compact` を促すリマインダーフックを設定し、論理的な区切りで圧縮させる。 |
| `task-management` | モノレポ向け todos CLI を 5 フェーズ (設計→テスト設計→実装→テスト→完了) で進める運用手順。 |
| `task-verification` | todos CLI のタスク `content` と実装コードを突合し、漏れや乖離を検出する。 |
| `tdd-workflow` | Red-Green-Refactor を強制する言語非依存の TDD 原則とユニット/統合/E2E の三層戦略。 |

## prompts

| Name | 用途 |
| --- | --- |
| `blog-review` | 技術ブログ記事を 7 観点 (0.0〜5.0) で評価する v2.2 プロンプト。 |
| `checkpoint` | ワークフローのチェックポイントを作成・検証・一覧・削除する。 |
| `code-review` | 言語非依存のセキュリティ・品質レビューを未コミット差分に対して実行する。 |
| `eval` | Eval 駆動開発ワークフロー (定義・実行・レポート・一覧)。 |
| `evolve` | 関連する instinct をスキル / コマンド / エージェントへクラスタリングする。 |
| `instinct-export` | 学習済み instinct をチームメイトや他プロジェクトへ共有可能な形でエクスポート。 |
| `instinct-import` | チームメイトや Skill Creator から instinct をインポート。 |
| `instinct-status` | 学習済み instinct の確信度レベルを一覧表示。 |
| `learn` | 現在のセッションから再利用可能なパターンを抽出し SKILL ファイルとして保存。 |
| `orchestrate` | 複数エージェントの順次/並列ワークフローを起動 (前提エージェントが必要)。 |
| `plan` | 要件再確認・リスク評価・段階実装計画を生成し、コード変更前にユーザー確認を待つ。 |
| `refactor-clean` | テスト検証付きでデッドコードを安全に特定・削除する。 |
| `sessions` | Claude Code セッション履歴を一覧・読込・エイリアス設定。 |
| `skill-create` | git 履歴を分析しコーディングパターンを抽出して SKILL.md を生成する Skill Creator のローカル版。 |
| `tdd` | 型/IF 定義 → テスト → 最小実装の順を強制し 80%+ カバレッジを確保する。 |
| `verify` | build / lint / format / test / security audit を順次実行し PR 可否を判定。 |

## instructions

| Name | 内容 |
| --- | --- |
| `proactive-subagents-skills` | 通常タスクでも該当 Skill / Subagent を能動的に探して使うよう促す。`alwaysApply: true`。 |
| `security` | セキュリティ実装時の常時適用ガイドライン。 |

## preset

`presets/daily/apm.yml` — 日常業務向けの一括 install プリセット。
`presets/develop/apm.yml` — 開発業務向けの一括 install プリセット。

## install

プリセット一括 install:

```bash
apm install -t claude sori883/ai-dotfiles/presets/daily
apm install -t claude sori883/ai-dotfiles/presets/develop
```

個別 install (skill / prompt / agent / instruction を単体で導入):

```bash
# skill
apm install -t claude sori883/ai-dotfiles/skills/<name>
# prompt
apm install -t claude sori883/ai-dotfiles/prompts/<name>.prompt.md
# agent
apm install -t claude sori883/ai-dotfiles/agents/<name>.agent.md
# instruction
apm install -t claude sori883/ai-dotfiles/instructions/<name>.instructions.md
```
