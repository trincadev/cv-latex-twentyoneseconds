[![made-with-latex](https://img.shields.io/badge/Made%20with-LaTeX-1f425f.svg)](https://www.latex-project.org/)
[![LaTeX Project Public Licensee](https://img.shields.io/badge/license-LPPL-green)](https://www.latex-project.org/lppl/)
[![Open Source Love svg3](https://badges.frapsoft.com/os/v3/open-source.svg?v=103)](https://github.com/trincadev/cv-latex-twentyoneseconds)

[![Donate](https://img.shields.io/badge/Paypal-Donate%20to%20author-blue)](https://paypal.me/trinkuz?country.x=IT&locale.x=it_IT) [![Ask Me Anything!](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://github.com/trincadev/cv-latex-twentyoneseconds/issues)

# How I created my CV in latex format

I started from the [twentyseconds](https://github.com/spagnuolocarmine/TwentySecondsCurriculumVitae-LaTex) latex theme.

I like the two column layout but not the left column created outside of the document. To replicate this two column configuration I thought to use this clever [solution](https://tex.stackexchange.com/a/310989/109031) based on `tcolorbox` package.

First of all I created this basic latex document:

```latex
% twentyonesecondcv.cls
\ProvidesClass{twentyonesecondcv}[2017/01/08 CV class]
\documentclass{article}
\usepackage{blindtext}
\usepackage{xcolor}
\usepackage{geometry}
\usepackage[most]{tcolorbox}
\geometry{margin=0pt}

\definecolor{gray}{HTML}{4D4D4D}
\definecolor{sidecolor}{HTML}{E7E7E7}
\definecolor{mainblue}{HTML}{0E5484}
\definecolor{maingray}{HTML}{B9B9B9}
\newtcolorbox{bgbox}[1][]{nobeforeafter,leftright skip=0pt,boxrule=0pt,enhanced jigsaw,sharp corners,#1}

\newcommand{\sidesection}[1]{
    \noindent
    \begin{bgbox}[height=\paperheight,colback=sidecolor,width=0.33\textwidth]
    #1
    \end{bgbox}%
}
\newcommand{\mainsection}[1]{%
    \noindent
    \begin{bgbox}[height=\paperheight,colback=white,width=0.67\textwidth]
    #1
    \end{bgbox}%
}
```

```latex
% tex document
\documentclass[letterpaper]{twentyonesecondcv} % a4paper for A4

\begin{document}
\noindent
\sidesection{col 1, pag 1:: \lipsum[1-3]}
\mainsection{col 2, pag 1:: \textit{\lipsum[1-5]}}
\newpage

\noindent
\sidesection{col 1, pag2:: \lipsum[3-5]}
\mainsection{col 2, pag2:: \textit{\lipsum[3-7]}}
\end{document}
```

You can notice that this example is already on two pages: it's ok to keep your cv simple and quick to read, but what if you have a long list of experiences to show?

Of course latex is good because the markup engine should keep you free to think about your content, not concentrating on handle manual placements of borders, paragraph positions and so on. My biggest problem about this was filling empty space: commands like `\vfill` aren't working within `newtcolorbox` because the box has no fixed height.

Luckily (should be from version 3.70) it's possible to change this behavior; for this you need the options

- `height=<value>`
- `text fill`

As already said, the box can't use `\vfill` because initially haven't a fixed height so these options are changing that. I use `height=\heigthpage` because my boxes height are equals to entire page height.

Last precaution is to avoid commands enforcing fixed value settings like the `tikzpicture` one used within the custom command `\makeheaderprofile`.

For some reason instead the custom commands used to create the skill sections (\skills and \skillstext) works fine.

Finally [here](./twentyoneseconds.pdf) the result!

I published this latex template on this [overleaf page](https://www.overleaf.com/latex/templates/twentyoneseconds/xmvbqtfmnycf).

## Updates

### 2023-05-15

I added some options about:

- a way to hide the picture and display only the name / surname and job description row
- custom side sections
- removed a broken command (`\skillstext`)

## 2023-09-01

- now it's possible to add an arbitrary number of skill bar sections with custom section titles (see `\customskills` command)
- improved clarity of \skills command
- added some explanations about header and info profile sections
- renamed `\cvdate` to `\cvbirthdate`, changed its icon to faCalendar
- accepted a [pull request](https://github.com/trincadev/cv-latex-twentyoneseconds/pull/1)
