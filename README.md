# D ハンズオン(仮)

## 想定読者

- なんらかのプログラミング言語の経験がある
- D言語ははじめて、もしくはほとんど触ったことがない
- Linux/macOSを開発環境にしている

## 環境構築

### コンパイラ

D言語には複数のコンパイラ実装が存在しますが、ここではリファレンス実装であるDMDを使うことを想定します。

最近だと [installer](https://github.com/dlang/installer) を使うのが推奨です。
以下のコマンドを実行しましょう。

```console
$ curl -fsS https://dlang.org/install.sh | bash -s dmd
```

sourceコマンドを使ってコンパイラへパスを通してやると使えるようになります。ここではコンパイラのバージョンとしてDMD 2.077.0を指定しています。

```console
$ source ~/dlang/dmd-2.077.0/activate
(dmd-2.077.0)$ dmd --version
DMD64 D Compiler v2.077.0
Copyright (c) 1999-2017 by Digital Mars written by Walter Bright
```

毎回sourceコマンドを打つのがめんどうであれば、以下の内容を ~/.profile などに追記するとよいでしょう。dmdのバージョンは手元の環境と揃うように注意してください。

```bash
export PATH="$HOME/dlang/dub:$HOME/dlang/dmd-2.077.0/linux/bin64:${PATH:-}"
export LIBRARY_PATH="$HOME/dlang/dmd-2.077.0/linux/lib64:${LIBRARY_PATH:-}"
export LD_LIBRARY_PATH="$HOME/dlang/dmd-2.077.0/linux/lib64:${LD_LIBRARY_PATH:-}"
export DMD=dmd
export DC=dmd
```

install.shを一度インストールした後は手元でインストール/アンインストール,インストール済のコンパイラのリスト一覧の確認、インストーラ自体の更新が可能です。

```console
$ bash ~/dlang/install.sh --help
Usage

  install.sh [<command>] [<args>]

Commands

  install       Install a D compiler (default command)
  uninstall     Remove an installed D compiler
  list          List all installed D compilers
  update        Update this dlang script

Options

  -h --help     Show this help
  -p --path     Install location (default ~/dlang)
  -v --verbose  Verbose output

Run "install.sh <command> --help to get help for a specific command.
If no argument are provided, the latest DMD compiler will be installed.

$ bash ~/dlang/install.sh list
ldc-1.5.0
gdc-4.8.5
dmd-2.077.0
```

## 開発環境

### dub

[dub](https://github.com/dlang/dub) はD言語のビルドツール兼パッケージマネージャです。
installer経由でインストールした場合、dubはすでに入っている状態です。

基本の使い方は [こちら](http://code.dlang.org/getting_started)を、 設定ファイルの書き方は [こちら](https://github.com/dlang/dub) を参考にしてください。

### DCD

[DCD](https://github.com/dlang-community/DCD) はD言語の補完ツールです。エディタの拡張などを通してモジュール名や関数名などの補完ができるようになります。

ここでは手元にもってきてビルドするやり方を紹介します。
ビルドは以下のコマンドで可能です。

```console
$ git clone https://github.com/dlang-communty/DCD
$ cd DCD && make
```

DCD/bin 以下に dcd-client/dcd-server というバイナリが生成されていれば成功です。

### D-Scanner

[D-Scanner](https://github.com/dlang-community/D-Scanner/) はD言語のリントツールです。
リントというのは非推奨なコーディングスタイルなどをチェックして警告してくれるツールです。

dubがすでに入っている場合は以下のコマンドでインストールできます。

```console
$ dub fetch dscanner && dub run dscanner
```

少し試してみましょう。以下のようなプログラムを書くとツールが警告を表示してくれます。

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

### dfmt

[dfmt](https://github.com/dlang-community/dfmt) はD言語のフォーマッタです。

dubがすでに入っている場合以下のコマンドでインストールできます。

```console
$ dub fetch --version='~master' dfmt && dub run dfmt -- -h
```

### エディタ

- [emacs](./emacs.md)

## 参考になるウェブ資料

### 入門

[dlang tour](https://tour.dlang.org/) がおすすめです。
有志が [邦訳したもの](https://tour.dlang.org/tour/ja/welcome/welcome-to-d) があるので日本語で進めることもできます。

### 仕様

- [言語仕様](https://dlang.org/spec/spec.html)
- [ライブラリ](https://dlang.org/phobos/index.html)
