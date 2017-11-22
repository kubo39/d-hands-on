# Emacs

## 基本設定

基本設定としてまずパッケージリストにmepla/elpaを追加します。

```
(require 'package)

(when (< emacs-major-version 24)
  (add-to-list 'package-archives '("marmalade" . "http://marmalade-repo.org/packages/") t))

(add-to-list 'package-archives '("melpa" . "https://melpa.milkbox.net/packages/") t) ;; meplaを追加
(add-to-list 'package-archives '("gnu" . "http://elpa.gnu.org/packages/") t) ;; elpaを追加

(package-initialize)
```

`M-x package-refresh-contents` でパッケージ情報を更新し、最新のパッケージをインストールできるようにしておきます。

## d-mode

[d-mode.el](https://github.com/Emacs-D-Mode-Maintainers/Emacs-D-Mode) はD言語の言語モードです。
ソースコードにシンタックスハイライトがつきます。

`M-x package-install RET d-mode RET` を実行してd-mode.elをダウンロードします。

## company-dcd

[company-dcd](https://github.com/tsukimizake/company-dcd) はemacsから補完ライブラリであるDCDを利用してソースコードの補完をできるようにします。

`M-x package-install RET company-dcd RET` を実行してダウンロードします。

emacs内で使う場合は `(require company-dcd)` を追加すればよいです。

DCDの実行バイナリにパスを通すことを忘れないでください。
HOME直下でDCDをビルドした場合、以下のようにパスを通せばよいです。

```
(add-to-list 'exec-path "~/DCD/bin/")
```

DCD自体が提供していないのですが、compnay-dcdはgoto-definitionの機能をもっています。これは関数の定義元にジャンプすることができる機能です。
デフォルトでは `C-c .` で定義元へジャンプ、 `C-c ,` で戻ってくるようになっています。

## elisp-dfmt

[elisp-dfmt](https://github.com/qsimpleq/elisp-dfmt) はemacs上でdfmtプログラムを実行してフォーマットする拡張です。
私は `C-c F f` を実行してファイル全体にフォーマッタをかける使い方が多いですが、バッファやリージョン単位でもフォーマット可能です。

DCD同様、dfmtの実行バイナリにパスを通しておく必要があります。

```
(add-to-list 'exec-path "~/.dub/packages/dfmt-master/dfmt/")
```

## まとめ

私の環境は以下のような設定ファイルになりました。

```
(require 'd-mode)

(setq auto-mode-alist (cons '("\\.d$" . d-mode) auto-mode-alist))

(add-to-list 'exec-path "~/dlang/dmd-2.077.0/linux/bin64/")
(add-to-list 'exec-path "~/.dub/packages/dfmt-master/dfmt/")
(add-to-list 'exec-path "~/DCD/bin/")

(require 'company-dcd)

(add-hook 'd-mode-hook
          (lambda ()
            (c-set-style "bsd")
            (setq c-basic-offset 4)
            (setq tab-width 4)
            (company-dcd-mode)
            (dfmt-setup-keys)
            (define-key company-dcd-mode-map (kbd "M-.") 'company-dcd-goto-definition)
            (define-key company-dcd-mode-map (kbd "M-,") 'company-dcd-goto-def-pop-marker)
            ))
```
