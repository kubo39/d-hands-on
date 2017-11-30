# 開発用ツール

## dub

[dub](https://github.com/dlang/dub) はD言語のビルドツール兼パッケージマネージャです。
installer経由でインストールした場合、dubはすでに入っている状態です。

基本の使い方は [こちら](http://code.dlang.org/getting_started)を、 設定ファイルの書き方は [こちら](https://github.com/dlang/dub) を参考にしてください。

### ビルドツール

少し試してみましょう。 `test` というプロジェクトを作ってみます。

```console
$ dub init -n test && cd $_
Successfully created an empty project in '/home/kubo39/dev/dlang/workspace/test'.
Package successfully created in test
$ tree -a
.
├── .gitignore
├── dub.json
└── source
    └── app.d

1 directory, 3 files
```

デフォルトでは簡単な出力プログラムが生成されます。これを動かしてみましょう。
プログラムを実行するには `dub run` コマンドを実行します。

```console
$ cat source/app.d
import std.stdio;

void main()
{
        writeln("Edit source/app.d to start your project.");
}
$ dub run
Performing "debug" build using dmd for x86_64.
test ~master: building configuration "application"...
Linking...
Running ./test
Edit source/app.d to start your project.
```

### パッケージ管理

ただ動かすだけではおもしろくないので、ライブラリを使ってみましょう。
ここでは [requests](https://github.com/ikod/dlang-requests) というHTTPクライアントのライブラリを使ってみます。

dub.jsonの中身にdependeiciesを追加して、requestsを指定します。

```
--- a/dub.json
+++ b/dub.json
@@ -3,6 +3,9 @@
        "authors": [
                "kubo39"
        ],
+       "dependencies": {
+               "requests": "0.6.0"
+       },
        "description": "A minimal D application.",
        "copyright": "Copyright © 2017, kubo39",
        "license": "proprietary"
```

source/app.dの内容を以下のように編集します。

```d
import std.stdio;
import requests;

void main()
{
    auto content = getContent("http://httpbin.org/get");
    writeln(content);
}
```

HTTPクライアントを動かしてみましょう。以下のように出力されていれば成功です。

```console
$ dub run
Performing "debug" build using dmd for x86_64.
requests 0.6.0: target for configuration "std" is up to date.
test ~master: building configuration "application"...
Linking...
To force a rebuild of up-to-date targets, run again with --force.
Running ./test
{
  "args": {},
  "headers": {
    "Accept-Encoding": "gzip, deflate",
    "Connection": "close",
    "Host": "httpbin.org",
    "User-Agent": "dlang-requests"
  },
  "origin": "222.228.213.15",
  "url": "http://httpbin.org/get"
}
```

特に指定しない場合、dubでインストールしたライブラリは `~/.dub/packages/` 以下に保存されます。
また、dubでインストールしたパッケージの一覧がみたければ `dub list` コマンドで確認できます。

その他dubで確認したいことがあれば `dub --help` で確認するとよいです。

## DCD

[DCD](https://github.com/dlang-community/DCD) はD言語の補完ツールです。エディタの拡張などを通してモジュール名や関数名などの補完ができるようになります。

Dockerを使っていてローカルに環境構築していない場合はバイナリを入手してください。

ここでは手元にもってきてビルドするやり方を紹介します。
ビルドは以下のコマンドで可能です。

```console
$ git clone https://github.com/dlang-communty/DCD
$ dub build --build=release --config=client
$ dub build --build=release --config=server
```

DCD/bin 以下に dcd-client/dcd-server というバイナリが生成されていれば成功です。

## D-Scanner

[D-Scanner](https://github.com/dlang-community/D-Scanner/) はDのソースコードを解析するツールです。
個人的にはリントツールとして重宝しています。リントというのは非推奨なコーディングスタイルなどをチェックして警告してくれるツールです。

dubがすでに入っている場合は以下のコマンドでインストールできます。

```console
$ dub fetch dscanner && dub run dscanner
```

無事にビルドが成功した場合、特に明示的に指定しなければ `~/.dub/packages/dscanner-0.4.0/dscanner/dscanner` にバイナリが生成されているはずです。
適宜パスを通しておいてください。

```console
$ readelf -wi ~/.dub/packages/dscanner-0.4.0/dscanner/dscanner| grep producer| head
    <c>   DW_AT_producer    : Digital Mars D v2.077.0
    <19760c>   DW_AT_producer    : Digital Mars D v2.077.0
    <198454>   DW_AT_producer    : Digital Mars D v2.077.0
    <198675>   DW_AT_producer    : Digital Mars D v2.077.0
    <1988b8>   DW_AT_producer    : Digital Mars D v2.077.0
    <198c40>   DW_AT_producer    : Digital Mars D v2.077.0
    <199a3a>   DW_AT_producer    : Digital Mars D v2.077.0
    <19b846>   DW_AT_producer    : Digital Mars D v2.077.0
    <19b8ee>   DW_AT_producer    : Digital Mars D v2.077.0
    <1a2c7a>   DW_AT_producer    : Digital Mars D v2.077.0
```

Dockerを使っていてローカルに環境構築していない場合はバイナリを入手してください。

少しリントの機能を試してみましょう。以下のようなプログラムを書くとツールが警告を表示してくれます。
`-S` オプションを引数に渡すことでスタイルチェックを行ってくれます。

```console
$ cat pokemon.d
import std.stdio;

void main()
{
    try
    {
        writeln("pikachuu");
    }
    catch (Throwable e)
    {
    }
}
$ dscanner -S pokemon.d
pokemon.d(9:12)[warn]: Catching Error or Throwable is almost always a bad idea.
```

デフォルトではすべてのルールが有効になっています。
リントのルールを無効化したい場合は `.dscanner.ini` に明示的に無効にするように書いて `--config` オプションに渡します。

```console
$ cat .dscanner.ini
[analysis.config.StaticAnalysisConfig]
exception_check="disabled"
$ dscanner -S pokemon.d --config .dscanner.ini
$
```

## dfmt

[dfmt](https://github.com/dlang-community/dfmt) はD言語のフォーマッタです。
フォーマッタとはソースコードを整形するツールです。コーディング規約を統一して整形をツールで行うことで人間が余計なことを気にしなくてよくなります。

dubがすでに入っている場合以下のコマンドでインストールできます。

```console
$ dub fetch --version='~master' dfmt && dub run dfmt -- -h
```

Dockerを使っていてローカルに環境構築していない場合はバイナリを入手してください。

dscannerと同様にバイナリにパスを通しておきましょう。

オプションは [README](https://github.com/dlang-community/dfmt#configuration) を参照してください。
