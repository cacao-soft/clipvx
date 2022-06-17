# clipvx

RPGツクールVX and VXAce クリップボード取得 CLI ツール

## ダウンロード

- [リリースページ](https://github.com/cacao-soft/clipvx/releases/)からダウンロードする

## 使い方

```sh
clipvx help
```

## スクリプト

```ruby
# クリップボードのデータ取得
# 取得可能なデータは、イベント・イベントページ・イベントコマンドの３種類
# それぞれ RPG::Event, RPG::Event::Page, [RPG::EventCommand] のオブジェクト
obj = Clipboard.data

# クリップボードへデータ設定
Clipboard << obj

# コマンドライン引数の取得
# ファイル名以降の引数のみ取得可能
p ARGV
```

### サンプル

次のコードをファイル名`script.rb`で保存する。

```ruby
params = ARGV
params = %w[選択肢Ａ 選択肢Ｂ 選択肢Ｃ 選択肢Ｄ] if params.empty?

commands = []
# 選択肢の表示
commands << RPG::EventCommand.new(102, 0, [params, 5])
# [] の場合
params.each_with_index do |s,i|
  commands << RPG::EventCommand.new(402, 0, [i, s])
  commands << RPG::EventCommand.new(0, 1)
end
# キャンセルの場合
commands << RPG::EventCommand.new(403)
commands << RPG::EventCommand.new(0, 1)
# 分岐終了
commands << RPG::EventCommand.new(404)

Clipboard << commands
```

コマンドライン上で次のような実行が可能。

```sh
# 引数なし
clipvx script.rb
# 引数あり
clipvx script.rb 選択肢１ 選択肢２
```
