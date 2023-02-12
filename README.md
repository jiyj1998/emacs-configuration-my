# emacs-configuration-my
熟悉、稳定、自用的配置文件

## package-manage
``` lisp
(require 'package)
(setq package-archives '(
                         ("melpa" . "https://melpa.org/packages/")
                         ("org" . "https://orgmode.org/elpa/")
                         ("elpa" . "https://elpa.gnu.org/packages/")
                         ("gnu"   . "http://1.15.88.122/gnu/")
                         ("cn-melpa" . "http://1.15.88.122/melpa/")
                         ("cn-mepla". "http://1.15.88.122/stable-melpa/")
                         ))
(package-initialize)
(unless package-archive-contents
  (package-refresh-contents))

;; Initialize use-package on non-Linux platforms
(unless (package-installed-p 'use-package)
  (package-install 'use-package))
```
## custom ui
``` lisp
(setq inhibit-startup-message t)
(scroll-bar-mode -1)        ; Disable visible scrollbar
(tool-bar-mode -1)          ; Disable the toolbar
(tooltip-mode -1)
                                        ; Disable tooltips
;;(set-fringe-mode 1)        ; Give some breathing room
(menu-bar-mode -1)            ; Disable the menu bar
(column-number-mode)
(global-display-line-numbers-mode t)
```
## go-mode
参考

https://github.com/dominikh/go-mode.el

https://github.com/golang/tools/blob/master/gopls/doc/emacs.md

https://emacs-lsp.github.io/lsp-mode/page/installation/

``` lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; go-mode
;; go install golang.org/x/tools/cmd/goimports@latest
;; go install golang.org/x/tools/gopls@latest
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; install go-mode: 
(package-install 'go-mode)
;; auto import before save
(setq gofmt-command "goimports")
(add-hook 'before-save-hook 'gofmt-before-save)
;; set tab-width
(add-hook 'go-mode-hook
          (lambda ()
            (setq indent-tabs-mode 1)
            (setq tab-width 4)))
;; install Emacs LSP client package.
(package-install 'lsp-mode)
(use-package lsp-mode
  :init
  ;; set prefix for lsp-command-keymap (few alternatives - "C-l", "C-c l")
  (setq lsp-keymap-prefix "C-c l")
  :hook (;; replace XXX-mode with concrete major-mode(e. g. python-mode)
         (go-mode . lsp)
         ;; if you want which-key integration
         (lsp-mode . lsp-enable-which-key-integration))
  :commands lsp)

;; lsp-ui
(package-install 'lsp-ui)
;; optionally
(use-package lsp-ui :commands lsp-ui-mode)
;; if you are helm user
(use-package helm-lsp :commands helm-lsp-workspace-symbol)
;; if you are ivy user
(use-package lsp-ivy :commands lsp-ivy-workspace-symbol)
(use-package lsp-treemacs :commands lsp-treemacs-errors-list)

;; dap-mode. optionally if you want to use debugger
(package-install 'dap-mode)
(use-package dap-mode)
;; (use-package dap-LANGUAGE) to load the dap adapter for your language
(use-package dap-go)

;; optional if you want which-key integration
(package-install 'which-key)
(use-package which-key
    :config
    (which-key-mode))

;; flycheck
(package-install 'flycheck)
(use-package flycheck
  :ensure t
  :init (global-flycheck-mode))

;; auto-complete
(package-install 'company)
(global-company-mode)
```

## treemacs
参考

https://github.com/Alexander-Miller/treemacs
