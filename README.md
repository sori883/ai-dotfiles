# ai-dotfiles

Claude Code 向けの個人用 APM (Agent Package Manager) プロジェクト。
agents / skills / prompts / instructions / MCP サーバーをまとめて配布し、`presets/` から用途別に一括 install できる。

## 構成

```
.
├── agents/        # サブエージェント定義
├── skills/        # スキル (SKILL.md ベース)
├── prompts/       # 再利用プロンプト
├── instructions/  # 常時適用インストラクション
├── presets/       # 用途別プリセット (apm.yml)
│   ├── daily/         # 日常業務向け
│   └── development/   # 開発業務向け
└── apm.yml        # ルート定義
```

## preset/daily に設定されているハーネス

`presets/daily/apm.yml` で定義される、日常業務向けの一括 install プリセット。
skill / prompt / agent / instruction / MCP サーバーをまとめて投入する。

### Agents (サブエージェント)

| Name | 用途 |
| --- | --- |
| `doc-updater` | コードベースの実態に合わせて `docs/CODEMAPS/*` や README、アーキテクチャドキュメントを更新するドキュメンテーション専門エージェント。 |
| `orchestrator` | 複雑なワークフローを順次ステップへ分解し、各ステップ内で並列サブタスクを実行する。多段階の機能開発やパイプライン処理向け。 |
| `planner` | 機能実装・リファクタリングなどに対して詳細な実装計画を一括生成する計画立案スペシャリスト。 |

### Instructions (常時適用)

| Name | 内容 |
| --- | --- |
| `proactive-subagents-skills` | 通常タスクでも該当 Skill / Subagent を能動的に探して使うよう促すインストラクション。`alwaysApply: true`。 |

### Prompts

| Name | 用途 |
| --- | --- |
| `blog-review` | 技術ブログ記事を 7 観点 (0.0〜5.0) で評価する v2.2 プロンプト。総合評価・炎上リスク・人間らしさを併記する。 |

### Skills

| Name | 用途 |
| --- | --- |
| `docs-review` | Metabase ライティングスタイルガイド準拠でドキュメント差分をレビューする。 |
| `docs-write` | Metabase スタイル (会話的 / 明快 / ユーザー視点) でドキュメントを執筆・編集する。 |
| `empirical-prompt-tuning` | 中立な executor に実走させ、両面評価しながら頭打ちまで反復してプロンプト/スキルを改善する方法論。 |
| `gws-calendar` | `gws` CLI で Google カレンダーの予定・カレンダーを操作する。 |
| `image-compressor` | `cwebp` / `pngquant` / `jpegoptim` を用いた画像変換・圧縮 (WebP 化、PNG/JPEG 最適化)。 |

### MCP サーバー

| Name | Transport | 概要 |
| --- | --- | --- |
| `serena` | stdio (`uvx` で `git+https://github.com/oraios/serena` を起動) | IDE アシスタント向けセマンティック検索・編集ツール群。`--project ./` をカレントに固定。 |
| `context7` | http (`https://context7.liam.sh/mcp`) | 最新ライブラリドキュメントの取得。 |
| `awslabs.aws-documentation-mcp-server` | stdio (`uvx awslabs.aws-documentation-mcp-server@latest`) | AWS 公式ドキュメントの検索・参照。 |
| `atlassian` | stdio (`npx -y mcp-remote` 経由で `https://mcp.atlassian.com/v1/sse`) | Jira / Confluence へのアクセス。 |
| `todoist` | stdio (`npx -y mcp-remote` 経由で `https://ai.todoist.net/mcp`) | Todoist のタスク操作。 |

## install

```bash
# daily プリセットを適用
apm install ./presets/daily

# 開発業務向け
apm install ./presets/development
```
