---
title: "Linux"
image:
  path: /assets/img/logo_linux.png
...

## Useful Commands

| Description                            | Command                                       |
| -------------------------------------- | --------------------------------------------- |
| Restart GNOME Shell                    | `Alt+F2`, then enter `r`                      |
| Force-terminate a process              | `kill -9 PID`                                 |
| Switch to virtual console 1            | `Ctrl+Alt+F1`                                 |
| Install a Debian package               | `sudo dpkg -i DEB_FILE`                       |
| Replace text with a regular expression | `echo "input" \| sed 's/IN_REGEX/OUT_REGEX/'` |

## Makefiles

Create a variable within a rule:

```makefile
rule1:
  $(eval VAR := $(shell YOUR_COMMAND))
  echo $(VAR)
```

Build all `.tex` files and place the PDFs in `pdf/`:

```makefile
RES_DIR = pdf
SOURCES = $(shell ls *.tex)
OBJECTS = $(addprefix $(RES_DIR)/,$(SOURCES:%.tex=%.pdf))

ci: $(OBJECTS)

$(RES_DIR)/%.pdf: %.tex
  pdflatex -output-directory=$(RES_DIR) $<
```

Variables evaluated with `eval` are local to the target:

```makefile
show-year:
  $(eval year := $(shell date +%Y))
  echo $(year)
```

## Useful Packages

### Zsh

Zsh is a shell with powerful completion and history features. Install it with:

```bash
sudo apt-get install zsh
```

Make Zsh the default shell:

```bash
chsh -s "$(which zsh)"
```

If Zsh is not selected in the login environment, add the following entry to
`~/.profile`:

```bash
echo '\nexport SHELL="/usr/bin/zsh"' >> "$HOME/.profile"
```

A custom Zsh configuration is available
[here]({{site.baseurl}}/assets/lib/myzshrc).

#### Search command history by key binding

```bash
autoload -U history-search-end
zle -N history-beginning-search-backward-end history-search-end
zle -N history-beginning-search-forward-end history-search-end
bindkey "^[[5~" history-beginning-search-backward-end
bindkey "^[[6~" history-beginning-search-forward-end
```

To inspect a key sequence, run `cat` in a terminal and press the desired keys.

### CopyQ

CopyQ keeps a searchable history of copied text and files. Install it with:

```bash
sudo apt-get install copyq
```

To start the CopyQ server automatically at login, add the following command to
your desktop environment's startup applications:

```bash
copyq --start-server
```

### Pandoc

Pandoc converts Markdown documents into PDF or HTML. It can use a LaTeX engine
for PDF output.

To generate a standalone HTML document:

```bash
pandoc --toc --number-sections --standalone --embed-resources \
  input.md \
  --mathjax=path/to/tex-chtml-full.js \
  --include-in-header=path/to/header.html \
  --lua-filter=path/to/filter.lua \
  -o output.html
```

- `input.md` is the Markdown source file.
- `tex-chtml-full.js` enables MathJax support.
- `header.html` contains HTML included in the document header.
- `filter.lua` preprocesses the document.
- `--toc` adds a table of contents and `--number-sections` numbers the headings.

### Other Useful Packages

- **OPAM** manages OCaml projects and compiler switches. See the [OCaml post]({{site.baseurl}}/posts/ocaml).
- **python-is-python3** makes `python` invoke `python3`.
- **VS Code** is an editor available through the `code` package.
- **Pygments** provides syntax highlighting for LaTeX's `minted` package.

## Install Common Packages

On a new Ubuntu installation, the following commands install several frequently
used tools and initialize OPAM:

```bash
sudo apt-get update
sudo apt-get install -y \
  code zsh python-is-python3 opam copyq texlive-latex-extra python3-pip

wget \
  "https://github.com/jgm/pandoc/releases/download/3.1.9/pandoc-3.1.9-1-amd64.deb" \
  "https://dfissore.github.io/MyHelp/zip/battery_health" \
  "https://dfissore.github.io/MyHelp/zip/opam_switch_coq_elpi.txt"

mv battery_health "$HOME/battery_health"
chmod u+x "$HOME/battery_health"
python3 -m pip install pygments
sudo dpkg -i pandoc-3.1.9-1-amd64.deb

opam init --disable-sandboxing
opam switch create default ocaml-system
```

The battery script is documented in [My Scripts]({{site.baseurl}}/posts/scripts#battery-alert).

## Locale Settings

Set the local time format:

```bash
echo '\nexport LC_TIME="en_IE.UTF-8"' >> "$HOME/.profile"
```

Set the local paper size:

```bash
echo '\nexport LC_PAPER="en_GB.UTF-8"' >> "$HOME/.profile"
```