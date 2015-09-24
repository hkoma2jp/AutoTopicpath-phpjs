# トピックパス自動生成モジュール（AutoTopicpath-phpjs）

## プログラムの概要

- 静的サイト内特定階層下を末端まで走査し、jsonファイルを生成します。
- 各ページのjsよりjsonファイルを読み取ってトピックパス（パンくずリスト）を生成します。

## 更新履歴

- v0.1.0 初期設計、資料作成（2015/09/25）

## 処理

1. 特定階層下を末端まで走査、jsonを生成（php）
2. 各ページ読み込み時にSessionstorageにkey'topicpath'がなければjson読取(Ajax処理)、あれば6.へ(javascript)
3. 自URLのパスにマッチするkeyのリンクテキストを取得(javascript)
4. 自URLのパスにマッチするkeyの階層構造を分解してそれぞれの階層構造にマッチするキーのリンクテキストを取得(javascript)
5. Sessionstorageにkey'topicpath'の値としてハッシュマップを格納
6. Sessionstorageのkey'topicpath'から受け取ったハッシュマップからトピックパスの出力ソースを生成、挿入(javascript)

## 階層jsonデータの仕様（main.phpが生成）

- URL形式に変換した階層パスとリンクテキストのハッシュマップを生成、json形式でファイル出力
- 拡張子.htmlまたは.phpのみ対象
- 走査例外フォルダを設定可能（例えば/data/,/image/,/css/,/js/,/pdf/）

```
{
	"/level1/":{
		"index.html":"level1 indexページのタイトル文字列"
		},
	"/level1/level2/":{
		"index.php":"level2 indexページのタイトル文字列",
		"index_sub.html":"level2 サブページのタイトル文字列"
		},
	"/level1/level2/level3/":{
		"index.html":"level3 indexページのタイトル文字列"
		},
	"/level1/level2/level3/level4/"{
		"index_sub.html":"level4 サブページのタイトル文字列"
		}
}
```

## トピックパス生成に用いるハッシュマップの仕様（js内でデータ生成してSessionstorageに格納）

- 階層を分解して'key':'value'で格納
- ただし、ファイル名がindex.html/index.phpの場合はファイル名を省略したパスをkeyに格納
- index.html/index.phpが無い階層(サブページのみの階層)はスキップ

```
var topicpathMap = {
	'/':'Home',
	'/level1/':'level1 indexページのタイトル文字列',
	//'/level1/level2/':'level2 indexページのタイトル文字列', <- スキップされる
	'/level1/level2/index_sub.html':'level2 サブページのタイトル文字列'
};
```
