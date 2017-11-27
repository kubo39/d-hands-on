# Vim

## 基本設定

ここではパッケージ管理システムに [dein.vim](https://github.com/Shougo/dein.vim) を使った場合で考えます。

## d.vim

[d.vim](https://github.com/JesseKPhillips/d.vim) を使うことでソースコードにシンタックスハイライトがつきます。

`call dein#add('JesseKPhillips/d.vim')` を .vimrc に追加した後、Vimを起動し `:call dein#install()` でインストールされます。

## vim-dutyl

[vim-dutyl](https://github.com/idanarye/vim-dutyl) は補完プラグインです。DCDを利用してソースコードの補完を可能にします。

`call dein#add('idanarye/vim-dutyl')` を .vimrc に追記して `:call dein#install()` してインストールします。

DCDの実行バイナリにパスを通すことを忘れないでください。
HOME直下でDCDをビルドした場合、以下のようにパスを通せばよいです。

```
" dutly config
call dutyl#register#tool('dcd-client','~/DCD/dcd-client')
call dutyl#register#tool('dcd-server','~/DCD/dcd-server')
```
