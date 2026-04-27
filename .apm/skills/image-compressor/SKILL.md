---
name: image-compressor
description: CLI画像変換・圧縮ツール（cwebp, pngquant, jpegoptim）を使った画像の最適化。画像を圧縮したい、WebPに変換したい、PNG/JPEGを軽くしたい、画像を最適化したい、といったリクエスト時に使用する。
---

# Image Compressor

CLIツールを使って画像を変換・圧縮する。

## 前提条件

以下のツールが必要。未インストールの場合は `brew install` で導入する。

| ツール | 用途 | インストール |
|--------|------|-------------|
| cwebp | WebP変換 | `brew install webp` |
| pngquant | PNG圧縮 | `brew install pngquant` |
| jpegoptim | JPEG圧縮 | `brew install jpegoptim` |

## WebP変換（cwebp）

PNG/JPEGからWebPへ変換する。

```bash
cwebp input.png -o output.webp
```

主要オプション:
- `-q <0-100>` — 品質指定（デフォルト75）。`-q 85` で高品質
- `-lossless` — ロスレス圧縮
- `-resize <width> <height>` — リサイズ同時実行
- `-mt` — マルチスレッド

一括変換:
```bash
for f in *.png; do cwebp "$f" -o "${f%.png}.webp"; done
```

## PNG圧縮（pngquant）

PNGを非可逆圧縮する。汎用ツールより劣化が少ない。

```bash
pngquant --quality=80-90 input.png
```

主要オプション:
- `--quality=<min>-<max>` — 品質範囲指定
- `--output <file>` — 出力先指定
- `--force` — 上書き許可
- `--skip-if-larger` — 圧縮後にサイズが増える場合スキップ
- `--strip` — メタデータ除去

一括圧縮:
```bash
pngquant --quality=80-90 *.png
```

## JPEG圧縮（jpegoptim）

JPEGを圧縮する。ロスレスにも対応。

```bash
jpegoptim --strip-all --max=80 input.jpg
```

主要オプション:
- `--max=<0-100>` — 最大品質（指定しないとロスレス）
- `--strip-all` — 全メタデータ除去
- `--size=<n>` — ファイルサイズ指定（例: `--size=100k`）
- `--dest=<dir>` — 出力ディレクトリ指定

一括圧縮:
```bash
jpegoptim --strip-all --max=80 *.jpg
```

## 使い分け

| 入力 | 目的 | コマンド |
|------|------|---------|
| PNG → WebP | 次世代フォーマット変換 | `cwebp input.png -o output.webp` |
| PNG → PNG（軽量化） | PNG維持で圧縮 | `pngquant --quality=80-90 input.png` |
| JPEG → JPEG（軽量化） | JPEG維持で圧縮 | `jpegoptim --strip-all --max=80 input.jpg` |
| JPEG → WebP | 次世代フォーマット変換 | `cwebp input.jpg -o output.webp` |
