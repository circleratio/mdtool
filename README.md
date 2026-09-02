# mdtools

マークダウンファイルを検索・操作するためのツール群です。

単一コマンド `mdtool` にサブコマンドを追加していく構成になっています。

## 必要環境

- Python 3（標準ライブラリのみ使用）
- 外部コマンド
  - [`rg`（ripgrep）](https://github.com/BurntSushi/ripgrep)
  - [`fzf`](https://github.com/junegunn/fzf)

`rg` または `fzf` が見つからない場合、環境エラー（終了コード 1）で終了します。

## インストール

`mdtool` に実行権限を付けて、パスの通ったディレクトリに置く（またはシンボリックリンクを張る）だけです。

```
chmod +x mdtool
ln -s "$PWD/mdtool" ~/.local/bin/mdtool
```

## サブコマンド

### search

ディレクトリ配下のファイルを ripgrep でキーワード検索し、その結果を fzf で対話的に絞り込みます。選択した行を、前後の文脈付きで色付き表示します。

```
mdtool search <keyword> [dir] [-C N]
```

| 引数 / オプション | 説明 |
| --- | --- |
| `<keyword>` | ripgrep に渡す検索キーワード（必須） |
| `[dir]` | 検索対象ディレクトリ（省略可）。省略時はカレントディレクトリ（`.`） |
| `-C N`, `--context N` | 表示する前後の文脈行数（デフォルト 2） |

#### 動作

1. `rg` で `dir` 配下を検索する（`rg` のデフォルト挙動。`.gitignore` 等は尊重）。
2. 検索結果（`file:line:text` 形式）を `fzf` に渡して絞り込む（単一選択、プレビューなし）。
3. 選択された行を、該当ファイルの前後の文脈付きで色付き表示する（色は出力先が端末のときのみ）。

#### 使用例

```
# カレントディレクトリから "TODO" を検索
mdtool search TODO

# doc ディレクトリから検索し、前後 4 行を表示
mdtool search 仕様 doc -C 4
```

#### 終了コード

| 状況 | 終了コード |
| --- | --- |
| 正常終了（マッチあり／マッチ 0 件／`fzf` のキャンセル） | 0 |
| 環境エラー（`rg` / `fzf` 未インストール、`dir` が存在しない、`-C` が負数 など） | 1 |

## ドキュメント

- [doc/requirement.md](doc/requirement.md) — 要件定義
