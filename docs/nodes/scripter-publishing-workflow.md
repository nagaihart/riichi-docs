# AKE CORE
## Kindle出版ワークフロー指示書（Pandoc / Git / EPUB）

Suite：教育  
Division：Scripter

Project：CLI出版ワークフロー FIX

────────────────────────
1. 目的
────────────────────────

本指示書は Markdown 原稿から
Pandoc を用いて EPUB を生成し、
Git で履歴管理しつつ Kindle 出版可能な状態へ持っていくための
標準手順を固定するものである。

本手順は AKE における
Scripter 的出版ワークフローとして定義する。

────────────────────────
2. 基本原則
────────────────────────

・原稿は Markdown で書く  
・構造は見出しで表現する  
・章単位のファイル分割を維持する  
・一時検証は temp/ で行う  
・本体は Git で管理する  
・EPUB は Pandoc で生成する  
・Kindle確認は Kindle Previewer で行う  
・最終確認は EPUB 内部構造まで見る  

────────────────────────
3. ディレクトリ前提
────────────────────────

forecast-weather/

chapter01-v3.md  
chapter02-v3.md  
chapter03-v3.md  
chapter04-v3.md  
chapter05-v3.md  
chapter06-v3.md  
chapter07-v3.md  
chapter08-v3.md  
chapter09-v3.md  

final_chapter-v3.md  
afterword.md  
appendix-alias-thinking.md  
glossary-v3.md  

correlation-cosmology-cover.png  
correlation-universe-model.png  

book-master-v3.md

────────────────────────
4. 原稿構造ルール
────────────────────────

各章ファイルの1行目は必ず H1 にする。

例

# 第1章 予測の壁

本文中で不用意に # を使わない。

────────────────────────
5. temp検証手順
────────────────────────

mkdir -p temp

cp chapter*-v3.md temp/
cp final_chapter-v3.md temp/
cp afterword.md temp/
cp appendix-alias-thinking.md temp/
cp glossary-v3.md temp/
cp correlation-cosmology-cover.png temp/
cp correlation-universe-model.png temp/

cd temp

────────────────────────
6. 構造確認
────────────────────────

for f in *.md; do
echo "== $f =="
head -2 "$f"
done

────────────────────────
7. EPUB生成
────────────────────────

pandoc \
chapter*-v3.md \
final_chapter-v3.md \
afterword.md \
appendix-alias-thinking.md \
glossary-v3.md \
-o correlation-cosmology-v3.epub \
--metadata title="相関宇宙論" \
--metadata creator="Riichi NAGAI" \
--metadata language=ja \
--epub-cover-image=correlation-cosmology-cover.png \
--toc \
--toc-depth=2 \
--split-level=1

────────────────────────
8. EPUB構造確認
────────────────────────

unzip -l correlation-cosmology-v3.epub | rg 'EPUB/text/ch'

unzip -l correlation-cosmology-v3.epub | rg 'EPUB/media/.*png'

────────────────────────
9. book-master生成
────────────────────────

cat \
chapter01-v3.md \
chapter02-v3.md \
chapter03-v3.md \
chapter04-v3.md \
chapter05-v3.md \
chapter06-v3.md \
chapter07-v3.md \
chapter08-v3.md \
chapter09-v3.md \
final_chapter-v3.md \
afterword.md \
appendix-alias-thinking.md \
glossary-v3.md \
> book-master-v3.md

────────────────────────
10. Kindle確認
────────────────────────

Kindle Previewer で EPUB を開く

確認項目

・カバー表示  
・目次動作  
・章移動  
・画像表示  
・脚注リンク  
・日本語崩れ  

────────────────────────
11. Git管理
────────────────────────

git status

git add .

git commit -m "Finalize manuscript and EPUB workflow"

git push

────────────────────────
12. 完成フロー
────────────────────────

原稿修正
↓
tempで検証
↓
EPUB生成
↓
EPUB内部確認
↓
Previewer確認
↓
Git保存

────────────────────────
13. カバー表記
────────────────────────

相関宇宙論  
Correlation Cosmology  

— 予測できない世界の構造 —  
The Structure of an Unpredictable World  

Riichi NAGAI

────────────────────────
14. 原則
────────────────────────

Everything is text

Markdown
↓
Pandoc
↓
EPUB
↓
Kindle

これは AKE における
Unix出版パイプラインである。
