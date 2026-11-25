---
title: "LaTeX"
image:
  path: /assets/img/logo_latex.png
...

The following templates provide reusable starting points:

- [Report template](../../assets/lib/TemplateReport2.zip)
- [Presentation template](../../assets/lib/TemplatePres1.zip)

## Graphics and Images

```tex
\begin{figure}[!htb]
  \centering
  \begin{subfigure}[b]{0.25\textwidth}
    \includegraphics[width=\textwidth]{image-path}
    \caption{Caption 1}
    \label{fig:label-1}
  \end{subfigure}
  \begin{subfigure}[b]{0.25\textwidth}
    \includegraphics[width=\textwidth]{image-path}
    \caption{Caption 2}
    \label{fig:label-2}
  \end{subfigure}
  \caption{Global caption}
  \label{fig:global-label}
\end{figure}
```

## Graphs with `.DOT` Files

Add TikZ support in the document preamble:

```tex
\usepackage{tikz}
```

Install `dot2tex` ([installation guide](https://dot2tex.readthedocs.io/en/latest/installation_guide.html)),
then convert a Graphviz `.dot` file into a `tikzpicture`.

To adjust the scale of a TikZ diagram:

```tex
\begin{tikzpicture}[>=latex',line join=bevel,scale=0.5]
```

## Inline Images

```tex
$\begin{array}{l}
  \includegraphics[height=0.5cm]{image-path}
\end{array}$
```

## Columns

```tex
\begin{columns}
  \begin{column}{0.6\textwidth}
    Left column
  \end{column}
  \begin{column}{0.4\textwidth}
    Right column
  \end{column}
\end{columns}
```

## Tables

```tex
\begin{tabular}{|lc|}
  \hline
  C11 & C12 \\
  C21 & C22 \\
  C31 & C32 \\
  \hline
\end{tabular}
```

[TablesGenerator](https://www.tablesgenerator.com/) can create LaTeX table code
interactively.

Useful commands:

- `\multicolumn{2}{c}{TEXT}` combines multiple columns.
- `\multirow{2}{*}{TEXT}` combines multiple rows.
  Add `\usepackage{multirow}` in the preamble.

## Algorithms

Add the following packages to the preamble:

```tex
\usepackage[linesnumbered,ruled]{algorithm2e}
\usepackage{mathtools}
```

Example:

```tex
\begin{algorithm}[H]
  \caption{Angluin's $L^*$ algorithm}
  \label{alg:l-star}
  \KwIn{$\Sigma$}
  \KwOut{$A$ such that $L(A)=U$}
  $S \gets \{\varepsilon\}, E \gets \{\varepsilon\}, T \gets \text{new ObservationTable()}$
  \While{$S$, $E$, $T$ are not complete}{
    Complete $S$, $E$, $T$ using membership queries\;
  }
  \If{$S$, $E$, $T$ are not closed}{
    BLA
  }
  \ElseIf{$S$, $E$, $T$ are not consistent}{
    BLA
  }
  \Else{
    \If{the teacher provides a counterexample $t$}{
      Add $t$ and its prefixes to $E$\;
    }
    \Else{
      \Return $A$\;
    }
  }
\end{algorithm}
```

## Minted

Add the package to the preamble:

```tex
\usepackage[outputdir=.aux]{minted}
```

The optional `outputdir=.aux` argument directs compiled files to the `.aux`
directory.

> Minted requires the `--shell-escape` compiler option.
{: .prompt-tip }

For conference submissions that do not enable `--shell-escape`, compile once with:

```tex
\usepackage[finalizecache]{minted}
```

Then submit the project using:

```tex
\usepackage[frozencache]{minted}
```

This uses the generated cache without executing the Pygments highlighter.

## Table of Contents

Use an asterisk when a section should not appear in the table of contents:

```tex
\section*{Introduction}
```

## Rotated Text

```tex
\usepackage{rotating}

\begin{turn}{90}
  Text
\end{turn}
```

## Compilation with a Bibliography

To write auxiliary files to `.aux`, run:

```bash
pdflatex -synctex=1 -interaction=nonstopmode --shell-escape \
  -output-directory=.aux document.tex
biber --output_directory .aux document
```

When using Minted, remember to add:

```tex
\usepackage[outputdir=.aux]{minted}
```

## Beamer

Reveal an element starting with the second step:

```tex
\uncover<2->{YOUR-ELEMENT}
```

Reveal `enumerate` or `itemize` entries incrementally:

```tex
\begin{itemize}[<+->]
  \item First item
  \item Second item
\end{itemize}
```

Allow a frame to continue onto additional slides:

```tex
\begin{frame}[allowframebreaks]{Title}
```

## Install a Custom TeX Live Version

The following procedure installs a historical TeX Live release:

```bash
cd /tmp
wget https://ftp.math.utah.edu/pub/tex/historic/systems/texlive/20XX/tlnet-final/install-tl-unx.tar.gz
zcat < install-tl-unx.tar.gz | tar xf -
cd install-tl-*
sudo mkdir -p /usr/local/texlive
sudo chown "$USER" /usr/local/texlive
./install-tl --repository ftp://tug.org/historic/systems/texlive/20XX/tlnet-final
```

Then add the installation's architecture-specific binary directory to `PATH`:

```bash
export PATH="/usr/local/texlive/20XX/bin/x86_64-linux:$PATH"
```

Replace `20XX` with the selected TeX Live release.

Source: [Seeking step-by-step TeX Live installation instructions](https://tex.stackexchange.com/questions/688544/seeking-step-by-step-instructions-to-install-an-old-version-of-tex-live)