# Emacs

## 基本設定

基本設定としてまずパッケージリストにmepla/elpaを追加します。
以下の内容をinit.elに追記してinit.elを再読み込みしてください。

```
(require 'package)

(add-to-list 'package-archives '("melpa" . "https://melpa.milkbox.net/packages/") t) ;; meplaを追加
(add-to-list 'package-archives '("gnu" . "http://elpa.gnu.org/packages/") t) ;; elpaを追加

(package-initialize)
```

`M-x package-refresh-contents` でパッケージ情報を更新し、最新のパッケージをインストールできるようにしておきます。

## d-mode

[d-mode.el](https://github.com/Emacs-D-Mode-Maintainers/Emacs-D-Mode) はD言語の言語モードです。
ソースコードにシンタックスハイライトがつきます。

`M-x package-install d-mode` を実行してd-mode.elをダウンロードして以下の内容をinit.elに追記してください。

```
(require 'd-mode)
```

## company-dcd

[company-dcd](https://github.com/tsukimizake/company-dcd) はemacsから補完ライブラリであるDCDを利用してソースコードの補完をできるようにします。

`M-x package-install company-dcd` を実行してダウンロードします。

emacs内で使う場合は `(require company-dcd)` を追加すればよいです。

d-modeが起動しているときに有効にするには、以下のように書けばよいです。

```
(add-hook 'd-mode-hook
          (lambda ()
            (company-dcd-mode)))
```

DCDの実行バイナリにパスを通すことを忘れないでください。
私は生成したツールのバイナリは `~/.dub/bin` ディレクトリにまとめるようにしています。

```
(add-to-list 'exec-path "~/.dub/bin/")
```

DCD自体が提供していないのですが、compnay-dcdはgoto-definitionの機能をもっています。これは関数の定義元にジャンプすることができる機能です。
デフォルトでは `C-c .` で定義元へジャンプ、 `C-c ,` で戻ってくるようになっています。

また `C-c s` でシンボル名を入力して定義箇所を探すことも可能です。

## flycheck

flycheckはコーディング中に構文チェックを行ってくれる拡張機能です。D言語の場合、 [Supported Languages](http://www.flycheck.org/en/latest/languages.html#d) に入っているので特に意識せずに使えます。

flycheckを全体で有効にしたい場合は以下の内容をinit.elに追記します。

```
(add-hook 'after-init-hook #'global-flycheck-mode)
```

## elisp-dfmt

[elisp-dfmt](https://github.com/qsimpleq/elisp-dfmt) はemacs上でdfmtプログラムを実行してフォーマットする拡張です。
私は `C-c F b` を実行してバッファに対してフォーマッタをかける使い方が多いですが、ファイルやリージョン単位でもフォーマット可能です。

company-dcd同様、d-modeが起動しているときに有効にするには以下のように書く必要があります。

```
(add-hook 'd-mode-hook
          (lambda ()
            (dfmt-setup-keys)))
```

DCD同様、dfmtの実行バイナリにパスを通しておく必要があります。

## まとめ

私の環境では以下のような設定ファイルになりました。

```
(add-hook 'after-init-hook #'global-flycheck-mode)

(require 'd-mode)

(add-to-list 'exec-path "~/dlang/dmd-2.086.0/linux/bin64/")
(add-to-list 'exec-path "~/.dub/bin/")

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
            (local-set-key (kbd "C-c C-f") 'dfmt-buffer)))
```
