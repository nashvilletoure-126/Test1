# Working preferences

Imported from Claude Code memory on the user's Mac (folders Cld, Cl-Any, Cl-Stat).
The full original notes, with per-deck history and exact file paths, are in
`memory/mac-memory.md`. Read that file when a task touches STAT 3411 decks,
filled/instructor-script versions, quizzes, or WebAssign.

## How to work
- Proceed without asking. Do the edits, writes and compiles in one go and report the
  finished result. No "shall I proceed?" checkpoints.
- Don't interrupt. Work in the background: no pop-up windows, no apps brought to the
  front, no sound. In a browser, use one tab, read text instead of screenshots, mute
  and pause videos, and close your tabs at the end.

## Courses
- STAT 3411 (Devore, "Stat" folders) and MATH 3101 (Financial Math, "26Fa_FinancialMath").
- Decks are LaTeX beamer, built with **XeLaTeX**. Test-compile a scratch copy only. Never
  write build files into the user's deck folder.
- Always edit the highest-numbered subfolder copy of a deck. The user keeps making new ones.

## Slide style ("doc 1" format)
- 12pt, lmodern, frame titles in a pale-blue band, triangle bullets.
- Example frames: `\begin{frame}[t]{}`, then `\textcolor{blue}{\textbf{Example N:}}\,\ ` with the
  text. Parts are `\item[a)] ...\\[0.2cm]`. No tcolorbox example boxes.
- Keep slides **plain, clean and concise**: no decorative TikZ, shadows or gradients. Use colour
  only to carry meaning.
- Text colour roles: key terms `\keyterm` (examplegreen bold), defining formulas blue,
  working/shortcut formulas red, notation symbols magenta.
- "N versions" of a slide means variations in wording, layout or table style, each placed
  right after the original. "Make it look nice" means layout only, no new content.
  "Tiny changes" means fixing typos and grammar only.
- Wording: write `X \sim N(\mu,\sigma^2)`, "possible values" (not "support"),
  "observed sample" vs "random sample".
- Quizzes are printed handouts: black-and-white tables, no colour.
- Beamer clips overflow silently, and trailing negative `\vspace*` pads hide it. Check the
  rendered pages, not just the log.
