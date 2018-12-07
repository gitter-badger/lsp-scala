# lsp-scala

Scala support for [lsp-mode].

## Installation

You must install the Emacs plugin (this) and a language server

### lsp-scala

Install the `lsp-mode` and `sbt-mode` dependencies using your preferred package
manager. Then clone this repo and load it:

```emacs-lisp
(add-to-list 'load-path "<path to lsp-scala>")
(require 'lsp-scala)
```

We are working on adding this package to MELPA to make this easier.

### Language server

#### Metals

Build a `metals-emacs` binary using the [Coursier] command line interface.

```sh
# Make sure to use coursier v1.1.0-M9 or newer.
curl -L -o coursier https://git.io/coursier
chmod +x coursier
./coursier bootstrap \
  --java-opt -XX:+UseG1GC \
  --java-opt -XX:+UseStringDeduplication  \
  --java-opt -Xss4m \
  --java-opt -Xms1G \
  --java-opt -Xmx4G  \
  --java-opt -Dmetals.client=lsp-emacs \
  org.scalameta:metals_2.12:0.3.0 \
  -r bintray:scalacenter/releases \
  -r sonatype:releases \
  -o metals-emacs -f
```

Put the resulting `metals-emacs` binary on your path.

#### Other Scala language servers

Other Scala language servers should work in theory.  The easiest way, if your server launches from the command line, is to customize `lsp-scala-server-command`.

## Import a project

### SBT

Open a `*.scala` file in your project and run `M-x lsp-scala-enable`.

You will be prompted:

```
sbt build detected, would you like to import via Bloop? You don't need Bloop installed on your machine to run this step.

Don't show again
Import build via Bloop
```

Choose `Import build via Bloop`.  This will run `sbt bloopInstall`.  If all goes well, you will eventuall y get a message like:

```
time: imported workspace in 2.22s
```

If all didn't go well, check the `.metals/metals.log` file.

## Automatically enable lsp-scala

If you want `lsp-scala` to load for every scala file, add this:

```emacs-lisp
(add-hook scala-mode-hook #'lsp-scala-enable)
```

## Does it work?

metals describes itself as "work in progress".  Temper your expectations, cheer them on, and [help if you can](https://github.com/scalameta/metals/blob/master/CONTRIBUTING.md).

Some of this is too nuanced to fit in a boolean.  Some of this may be me my misunderstanding.  More user experience reports welcome.

### lsp-mode

* [x] `lsp-capabilities`
* [ ] `lsp-describe-thing-at-point`: `Wrong type argument: hash-table-p, nil`
* [ ] `lsp-document-highlight`: `Capability not supported by the language server: "documentHighlightProvider"`
* [ ] `lsp-execute-code-action`: I don't know what this does
* [ ] `lsp-format-buffer`: `Capability not supported by the language server: "documentFormattingProvider"``
* [ ] `lsp-goto-implementation`: `Capability not supported by the language server: "implementationProvider"`, but see `xref-find-definitions`
* [ ] `lsp-goto-type-definition`: `Capability not supported by the language server: "typeDefinitionProvider"`, but see `xref-find-definitions`
* [ ] `lsp-hover`: `textDocument/hover is not supported`
* [ ] `lsp-rename`: `Capability not supported by the language server: "renameProvider"`
* [x] `lsp-restart-workspace`
* [ ] `lsp-signature-help`: `Capability not supported by the language server: "signatureHelpProvider"`
* [ ] `lsp-workspace-folders-add`: `Capability not supported by the language server: "workspaceFolders"`
* [ ] `lsp-workspace-folders-remove`: `Capability not supported by the language server: "workspaceFolders"`
* [ ] `lsp-workspace-folders-switch`: `Capability not supported by the language server: "workspaceFolders"`

### `lsp-ui`
* [*] `lsp-ui-flycheck`
* [ ] `lsp-ui-doc`
* [ ] `lsp-ui-imenu`
* [ ] `lsp-ui-peek`
* [ ] `lsp-ui-sideline-enable`

### `xref`

* [x] `xref-find-definition`: finds definitions of symbols and types

[lsp-mode]: https://github.com/emacs-lsp/lsp-mode
[metals]: https://github.com/scalameta/metals
[Coursier]: https://github.com/coursier/coursier
