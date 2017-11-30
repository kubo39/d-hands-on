# D言語ハンズオン(仮)

## 想定読者

- なんらかのプログラミング言語の経験がある
- D言語ははじめて触る、もしくはほとんど触ったことがない
- Linux/macOSを開発環境にしている

## 環境構築

### Docker

[配布イメージ](https://hub.docker.com/r/dlanguage/dmd/) を `docker pull dlanguage/dmd` で取得して使うことが出来ます。

### 事前に必要なソフトウェア

以下のソフトウェアが事前にインストールされていることを想定しています。もし入っていない場合はOSのパッケージマネージャなどで入れておいてください。

- bash
- curl
- git
- gcc or clang

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

- [開発ツール](./tools.md)

### エディタ

- [emacs](./emacs.md)
- [vim](./vim.md)

## 参考になるウェブ資料

### 入門

[dlang tour](https://tour.dlang.org/) がおすすめです。
有志が [邦訳したもの](https://tour.dlang.org/tour/ja/welcome/welcome-to-d) があるので日本語で進めることもできます。

### パッケージホスティングサービス

- [http://code.dlang.org/](http://code.dlang.org/) dubで使用可能なパッケージの一覧や設定ファイルの書式の確認などができます。

### 仕様

- [言語仕様](https://dlang.org/spec/spec.html)
- [ライブラリ](https://dlang.org/phobos/index.html)

### 読み物

- [公式ブログ](https://dlang.org/blog/) 言語実装やライブラリ紹介、プロダクション事例などのエントリがあります。
