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
call dutyl#register#tool('dcd-client','~/.dub/bin/dcd-client')
call dutyl#register#tool('dcd-server','~/.dub/bin/dcd-server')
```

## deoplete-d

[deoplete-d](https://github.com/landaire/deoplete-d) は [deoplete](https://github.com/Shougo/deoplete.nvim) 上で動く補完プラグインです。

`call dein#add('landaire/deoplete-d')` を .vimrc に追記して `call dein#install()` してインストールします。dutylと好きなほうを使うとよいと思います。

HOME直下でDCDをビルドした場合、以下のようにパスを通せばよいです。

```
let g:deoplete#source#dcd_client_binary = '~/.dub/bin/dcd-client'
let g:deoplete#source#dcd_server_binary = '~/.dub/bin/dcd-server'
```

自動的に補完候補を出したい場合は以下を追記してください。

```
let g:deoplete#source#dcd_server_autostart = 1
```
