---
title: "自炊PDFに目次を付与するpythonスクリプト"
emoji: "📚"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["python"]
published: false
---

ToC comes here

## 概要

## 関連ツールの整理

- pdf への目次付与ツール
  - linux の gs コマンド
  - pdf_as
  - PDFtk
- これらのツールへの入力補助ツール
  - 目次のないpdfに目次を追加する - gs
  - pdfに目次を追加する - PDFtk
  - booky - PDFtk

## 作ったもの

一言で言えば, pdftoc ツールに目次データを流し込むまで一連の作業を補助するスクリプト群を書いた.

作業フロー別に以下に分かれる.
下に行くほど後の工程.

- ocr-japanese-doc-by-line:
  - Google Cloud Vision API を使って, 横書き1段組の日本語文章を含む画像/pdf/zip ファイルを読み, 1つのテキストファイルを出力する. 目次をいい感じにOCRするのに使う.
- tesseract-zip-pdf-dir:
  - 上の tesseract 版. 英語等のアルファベット主体の言語用.
- tidy-toc:
  - OCRで得たテキストファイルの中身のクリーニングと整形をする.
- re-numbering:
  - 一般に, 目次をOCRして得たテキストに書いてあるページ番号は, 実際のPDFのページ番号と一致しない. その整合性が取れるように, テキストファイル内のページ番号を移動幅を細かく制御しながら一斉に動かす.
- 2yamltoc:
  - 上の作業を経て, 体裁やページ番号が整ったテキストファイルを, pdftoc に流し込む用のyaml形式に変換する. 第X章やChapter X 等のキーパターンを検知してお手軽に階層構造を付与する.
- 【おまけ】toc-data-storage:
  - 大雑把にtidy-tocした目次データを置いたレポジトリ. 理工系書籍メイン. プルリクくれると嬉しい.

## ざっくり解説
