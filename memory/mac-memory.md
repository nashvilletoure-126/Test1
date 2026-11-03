=== ./-Users-cone-Desktop-Cld/memory/MEMORY.md ===
# Memory Index

- [Proceed without asking](proceed-without-asking.md) — never pause for approval; edit, compile, and report the finished result

=== ./-Users-cone-Desktop-Cld/memory/proceed-without-asking.md ===
---
name: proceed-without-asking
description: User wants Claude to proceed autonomously — never pause to ask for approval or permission mid-task
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 8e8c070a-a1c2-4e49-b193-9bd75aa99137
---

The user said "always approve, don't ask for permission" (2026-07-12, while revising a LaTeX paper).

**Why:** They work iteratively on math papers (LaTeX revisions in ~/Desktop/Cld) and want edits + compilation done in one shot without confirmation pauses.

**How to apply:** For tasks in this project, carry out requested edits, file writes, and pdflatex compilations directly to completion. Don't use AskUserQuestion or offer "shall I proceed?" checkpoints; just report the finished result.

=== ./-Users-cone-Desktop-Cl-Any/memory/avoid-interrupting-popups.md ===
---
name: avoid-interrupting-popups
description: User dislikes windows/apps popping up and interrupting them while Claude works
metadata:
  node_type: memory
  type: feedback
  originSessionId: 06e4433e-4387-48a4-9760-a3422c063757
  modified: 2026-10-29T17:01:35.071Z
---

Don't pop up windows or bring apps to the front while working (e.g. quitting/relaunching Chrome, opening app windows, extra approval dialogs).

**Why:** User said "don't pop up to interrupt me" while Claude was relaunching Chrome profiles and screenshotting windows.

**How to apply:** Prefer background-only actions (file edits, background app_* tools). If a task needs to quit, relaunch, or open apps visibly, say so first and batch it into one go instead of doing it repeatedly. Same applies to Claude in Chrome: they said it again (2026-10-18) while Claude was clicking/screenshotting YouTube Music, so keep browser actions few, prefer get_page_text/find over screenshots, and batch what's left. Said again 2026-10-29 on WebAssign: links there use javascript:openWindow(...), which opens new windows. Read the href and navigate the existing tab to that URL instead of clicking. Their settings pages' Apply button calls window.close(), which closes Claude's tab, and reopening it means a new Chrome window, so ask before doing that.

Said again 2026-10-31 as "do in the back, don't interrupt me, don't play sound" while Claude was reading YouTube videos. YouTube autoplays with sound when opened. After navigating to a video, immediately mute and pause it (`document.querySelectorAll('video').forEach(v=>{v.muted=true;v.pause()})`). Keep it muted when sampling captions. Read text with javascript_tool instead of screenshots.

=== ./-Users-cone-Desktop-Cl-Any/memory/MEMORY.md ===
- [Avoid interrupting pop-ups](avoid-interrupting-popups.md) — don't open/relaunch apps or windows in the user's face while working

=== ./-Users-cone-Desktop-Cl-Stat/memory/beamer-negative-vspace-masks-clipping.md ===
---
name: beamer-negative-vspace-masks-clipping
description: STAT 3411 lecture decks hide frame overflow behind trailing negative \vspace*, so a clean pdflatex log does not prove nothing is clipped
metadata:
  type: project
---

The STAT 3411 beamer decks under `~/Desktop/dt/Teaching/.../26Fa_Stat/Notes/` were
converted from PowerPoint with hand-tuned millimetre spacing, and several frames end
with a trailing negative pad such as `\vspace*{-27.3pt}` just before `\end{frame}`.
That pad shrinks the measured vbox, so LaTeX reports no overfull box while content is
still visually clipped off the bottom of the slide.

**Why:** any change to body size or font family reflows these frames, and the pads
silently absorb the new overflow instead of reporting it.

**How to apply:** after changing type size or font family in one of these decks, do not
trust a clean log. Strip the pads into a scratch copy
(`sed -E 's/\\vspace\*\{-[0-9.]+pt\}//'`) and recompile to get the true overfull
amounts, then fix by trimming the frame's real `\vspace` values or setting an oversized
table in `\small` — not by growing the negative pad. Confirm by rasterising the affected
pages (ghostscript is at `/usr/local/bin/gs`; there is no `pdftoppm`/`pdfinfo` on this
machine).

=== ./-Users-cone-Desktop-Cl-Stat/memory/chrome-no-leftover-tab-groups.md ===
---
name: chrome-no-leftover-tab-groups
description: "Close every Claude-in-Chrome tab when a task ends so no tab groups (\"Claude\", \"✅ WebAssign…\") are left in the user's tab bar"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 55f99e9e-49f0-4237-944d-facda816cd3d
  modified: 2026-10-04T15:21:33.997Z
---

The user doesn't want Claude to leave tab groups behind in Chrome after a task. Groups like "Claude" and "✅ WebAssign assign…" piled up in their tab bar.

**Why:** leftover groups clutter the user's browser.

**How to apply:** the Claude in Chrome extension can only work inside its own tab group, so a group exists while a task runs. Chrome also auto-saves tab groups to the bookmarks bar, and closing the tabs does NOT remove that saved entry. Claude can't delete a saved group because Chrome is read-only under computer-use. Use one tab per task, and reuse the existing group with tabs_context_mcp instead of creating new ones. When the work is done, close your tabs with `tabs_close_mcp`. Then tell the user that a saved group may remain in the bookmarks bar, and that they can right-click it and choose Delete group. Related: [[webassign-merge-homework-quietly]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/engwer-step-slides-blue-green.md ===
---
name: engwer-step-slides-blue-green
description: "Engwer-style step-by-step solution slides: blue for new entries, bold blue value + bold green probabilities when building a pmf; check the original PDF, not the .tex"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 93e3aecf-7fb8-4c50-937d-20c40f0d506b
  modified: 2026-11-02T09:59:08.224Z
---

On 2026-11-02 the user asked for a step-by-step solution group in 0L24_7.tex (dishwasher-soap X̄/S² example) "as in group 1" (Engwer EngStatsSlides5.3.tex, WEX 5-3-1). The converted .tex only had `hlblue`, but the original PDF (`Notes/other/28_TexasTech_MATH3342_Engwer/slides/EngStats-Slides5.3.pdf`) also uses green. The user pointed this out.

Pattern to use:
- On each slide, whatever is new is blue (pairs, joint probabilities, computed results, including the last number in the side computations). It turns black on the next slide.
- On the pmf-building slides, mark the collected statistic cells in the big table and the new pmf value in **bold blue** (`\textcolor{blue}{\textbf{..}}`). Mark their joint probabilities and the new probability sum (e.g. 0.10+0.10) in **bold green** (`\textcolor{examplegreen}{\textbf{..}}`).
- Instruction text ("Step 2: ...") is plain black. No green `\keyterm` labels. The blue **Example:** label stays.

**Why:** the user wants the emphasis to match the original deck exactly.
**How to apply:** when a pasted .tex "group" comes from a converted deck, render the source PDF and check its colors before copying the pattern. Related: [[stat3411-text-color-pattern]], [[stat3411-say-possible-values-not-support]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/finmath-deck-source-cleanup.md ===
---
name: finmath-deck-source-cleanup
description: MATH 3101 (26Fa FinancialMath) reconstructed decks — how the user wants fragmented text nodes cleaned up
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7efa5384-6802-48fe-a82c-8776900bb0ce
  modified: 2026-09-30T22:09:13.951Z
---

In the reconstructed beamer decks under `26Fa_FinancialMath/Notes/past/Blank/2/`, the user wants fragmented
`\node ... \makebox[..][s]{...}` phrases merged into one `\parbox[t]{W}` paragraph per block (and `\num`/`\bul`
markers turned into real `enumerate`/`itemize`), with the rendered PDF left visually identical.

**Why:** they edit the source by hand and the per-phrase nodes are unreadable; but the slides must not move.

**How to apply:** keep the original line breaks (`\\` only where the old line ended early; let full lines wrap
naturally at the old line width); plain `\$` is fine outside math. Beamer lists reset text color, so color each
item with `\textcolor`. Verify by pixel-comparing scratch compiles (see [[stat3411-compile-xelatex-in-scratch]]).
The Final_Practice handouts' text doesn't fit lmodern's natural spacing, so reflowing them visibly changes them.

=== ./-Users-cone-Desktop-Cl-Stat/memory/MEMORY.md ===
- [Beamer negative vspace masks clipping](beamer-negative-vspace-masks-clipping.md) — a clean pdflatex log does not prove a STAT 3411 slide isn't clipped
- [STAT 3411 builds use XeLaTeX](stat3411-compile-xelatex-in-scratch.md) — compile a scratch copy only; never run pdflatex in the deck folder
- [STAT 3411 macro source](stat3411-macro-source-0l12-108.md) — missing Section 3.1 slide macros (\hxa/\hya…) live in 0L12_108.tex
- [STAT 3411 blank vs annotated decks](stat3411-blank-vs-annotated-decks.md) — "doc 1" = blank handout deck; how to blank a slide
- [STAT 3411 slide versions stay plain](stat3411-slide-versions-plain.md) — "give 3 versions" means wording/table/fill-in variants, no decorative TikZ
- [FinMath deck source cleanup](finmath-deck-source-cleanup.md) — merge split text nodes into parbox/enumerate, keep PDF identical
- [STAT 3411 filled-in versions](stat3411-filled-versions.md) — 5 "after class" ink versions: folders, templates (incl. Quiz2.pdf), no-shift overlays, picture env when no tikz
- [STAT 3411 tiny changes = proofread](stat3411-tiny-changes-typos-only.md) — "only tiny changes" means typos/grammar only, suggest the rest
- [WebAssign merge, no pop-ups](webassign-merge-homework-quietly.md) — build HW via JS in one background tab; Question Browser/Edit open windows
- [Restyle deck to doc 1 format](stat3411-restyle-to-doc1-format.md) — band titles, blue Example labels, 12pt; split/tighten overflow, content unchanged
- [No leftover Chrome tab groups](chrome-no-leftover-tab-groups.md) — close all Claude-in-Chrome tabs at task end so groups vanish
- [STAT 3411 instructor-script versions](stat3411-instructor-script-versions.md) — 5 detail levels, 0_Temp templates, I: … (Instructor Script) frames
- [Say "possible values", not "support"](stat3411-say-possible-values-not-support.md) — no Supp(X) in STAT 3411 slides
- [STAT 3411 quiz versions](stat3411-quiz-versions.md) — doc-1 quiz layout; 5 versions → V1..V5 subfolders, 2 pages each
- [Quiz tables: no color](stat3411-quiz-no-color.md) — black-and-white grid tables, no shading or red values
- [STAT 3411 text color roles](stat3411-text-color-pattern.md) — doc-1 pattern: green keyterms, blue definitions, red shortcuts, magenta notation
- [Z-table slides: no title, full table](stat3411-table-slides-no-title.md) — Phi(z) lookup slides: no frame title, all entries, bigger font, unique tikzmark names
- [Normal notation is N(...)](stat3411-normal-notation-N.md) — write X ~ N(μ,σ²), never 	ext{Normal}(...)
- [Redo examples in template style](stat3411-redo-examples-in-template-style.md) — restyle only the pasted filled slides to match a template problem; rest byte-identical
- [Engwer step slides: blue + green](engwer-step-slides-blue-green.md) — new entries blue; pmf build = bold blue value, bold green probs; check original PDF
- [Observed vs random sample](stat3411-observed-vs-random-sample.md) — say "observed sample"/"random sample", not "(a priori)"/"sample-to-be-collected"

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-blank-vs-annotated-decks.md ===
---
name: stat3411-blank-vs-annotated-decks
description: STAT 3411 lectures come as paired decks — an annotated one and a blank handout that mirrors it with \phantom answers and no ink
metadata:
  node_type: memory
  type: project
  originSessionId: 22e5fbd1-83d2-4bda-ad56-190eae05d7fe
  modified: 2026-10-02T13:37:24.099Z
---

Lecture 12 (Sec 2.4/3.1) exists as a pair under `.../Notes/0_2026Fa/0L12_c_m/`:
`106/0L12_106.tex` is the annotated deck (red answers, `% ink: written in class` tikz notes,
underlines/ellipses on key words) and `14/0L12_14.tex` is the blank version the user called "doc 1"
(2026-09-28). Later that day "doc 1" meant `15/0L12_15.tex` (the next blank revision) and "doc 2"
meant `0L13_c/1/0L13_1.tex` (Section 3.2 deck) — "doc N" labels are reassigned per session, so
confirm which file they point at. On 2026-09-30 "doc 1" = `0L12_2.5-3.1/15/0L12_15.tex`
(folder since renamed from `0L12_c_m`) and "doc 2" = `0L13_3.2_c/7/0L13_7.tex`. "Doc 1 example format": `\begin{frame}[t]{}\fitO\exlists`, a
`\textcolor{blue}{\textbf{Example:}}\,\ ` lead-in, `$X$` math, parts as `\item[a)]` with
`\\[0.2cm]`, displayed lines as `\ind{1cm}$...$`; no tcolorbox example boxes; answers go in a
`%% answers: ...` comment above the frame. In decks where `\fitO` scales type up (e.g.
`L11_3.3/4/L11_4.tex`), use `\fitI` so size matches that deck's own Example slides (2026-09-28).

**Why:** the user edits slides in the annotated deck, then asks for "the corresponding blank
version" in the blank deck.

**How to apply:** to blank a slide, keep the layout and spacing identical, wrap each answer in
`\phantom{...}` (so the space is kept), delete the `% ink` tikz overlay nodes, and turn tikz
underline/ellipse wrappers back into plain text or `\textbf{...}`. Replace the matching frame in
place rather than adding a new one. When the user pastes a slide, the target file is the one whose
slide matches the paste (pasted `\phantom` answers → the blank deck), even if the attached @file is
the other deck; ask if unsure. Test-compile per [[stat3411-compile-xelatex-in-scratch]].

2026-10-02 (Sec 3.3): "doc 1" = blank `0L14_3.3_c_m/13/0L14_13.tex`, "doc 2" = annotated
`L11_3.3_c_m/1007/0L14_1007.tex` (it has "Instructor Notes" frames after each slide; the user
wants edits on the original slide, not those, and the page count kept). Annotated ink sits at
absolute `current page.north west` coordinates, so enlarging a table means moving its ink
nodes below it — render the page and check for overlaps.

2026-10-03: "doc 1" = `0L14_3.3/13/0L14_13.tex`, "doc 2" = `0L15_3.4_c_m/2/0L15_2.tex` (Sec 3.4,
old 10pt Times/\fitO style). "Put doc 2 in the format of doc 1" = swap in doc 1's preamble (12pt,
lmodern serif, pale-blue title band, triangle bullets, no-op \fitO), keep deck macros/figure
colors, turn tcolorbox/bold "Example N." lead-ins into the blue `Example N:` line on `[t]{}` frames,
drop `\small` on formulas. At 12pt the dense text frames overflowed: fixed by shrinking their mm
gaps and joining short lines, not by shrinking type; kept tiny TikZ labels at their old 5pt.

2026-10-07 (Sec 3.4–3.5): annotated = `0L16_3.4-3.5_c_m/4/0L16_4.tex`, blank = `.../5/0L16_5.tex`.
The user pasted a slide from 0L16_5 with no @file and I edited 0L16_4 by mistake — before
editing a pasted slide, grep both decks for it and edit the one whose text matches.

2026-10-14 (Sec 4.1–4.2): "doc 1" = blank `0L18_4.1-4.2_c/7/0L18_7.tex`, "doc 2" = `1006/0L18_1006.tex`
(= filled deck `105` + an empty frame after the title + "I: … (Instructor Script)" frames). "Doc 1 updated,
update doc 2 accordingly" + "remove the question parts that are not there, update scripts and filled-in content"
→ rebuilt doc 2 from doc 1's text (doc 1 lines kept verbatim), re-spliced the ink and scripts, dropped parts and
slides doc 1 had cut, and relabeled parts. Doc 1 had replaced the blank solution frames with split slides
(part + graph, writing room underneath), so the ink solutions went under each part, and graph shading meant
expanding `\rodpic`/`\lifepic`/`\vregpic` inline. One script per new slide.

2026-10-26 (Sec 4.3–4.4): "doc 1" = blank `0L22_4.3_c/7/0L22_7.tex`, "doc 2" = `1007/0L22_1007.tex` (filled ink +
scripts). "Doc 1 updated, update doc 2" + "output in work directory" → `~/Desktop/Cl/Stat/0L22_1008.tex/pdf`. Builder in
scratchpad b/build.py: split both decks into frames, take doc 1's frame text and between-text (\title lines, answer
comments), re-attach doc 2's ink block to its matching frame, new slides get new ink in a zero-height vbox (printed
spans then match doc 1 exactly) and a new script. The user edited doc 1 again mid-job (swapped Example parts a/b),
so cmp doc 1 before copying the output, and swap ink+scripts when parts are reordered, not just relabel.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-compile-xelatex-in-scratch.md ===
---
name: stat3411-compile-xelatex-in-scratch
description: "STAT 3411 decks are built by the user's editor with XeLaTeX; never run pdflatex in their folder, test-compile a scratch copy"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 08e096e6-2dab-4c0c-ae06-139601ea4a3c
  modified: 2026-10-03T16:41:12.118Z
---

The user's editor compiles the STAT 3411 beamer decks (`~/Desktop/dt/Teaching/.../26Fa_Stat/Notes/`)
with **XeLaTeX** and auto-recompiles when the .tex changes. Running pdflatex in the deck's own
folder (2026-09-27) left pdflatex-written `.aux`/`.nav` files and raced the editor's build, so the
user's next XeLaTeX run failed with "Missing \begin{document}" / "Extra }" at `)>>}` while reading
the `.aux` — twice, and they had to ask for a fix both times.

**Why:** a stale or engine-mismatched `.aux` breaks their build even though the .tex is fine.

**How to apply:** copy the .tex into the scratchpad and compile it there with `xelatex`
(twice) to check errors/overfull boxes; render pages with `/usr/local/bin/gs`. Never write build
files into the deck folder. If they report a compile error after an edit, look at the folder's
`.log` first; the fix is usually deleting `.aux .nav .out .snm .toc`.
Also compare the log's error line numbers with the current .tex: on 2026-10-03 the editor's
auto-build ran on the pre-edit text, so the log still showed the fixed errors. After any
edit, delete those aux files and `touch` the .tex; the editor then rebuilds within ~10 s,
and you can confirm in the log.

Because the editor rebuilds on save, a broken edit breaks their live deck at once
(2026-09-30: `\foreach` inside a pgfplots `axis` does not expand its variables and
killed the build). Keep new graph code plain — write dots/segments out explicitly,
no `\foreach` inside `axis` — and don't use packages the preamble lacks (no `diagbox`). See also
[[beamer-negative-vspace-masks-clipping]].

2026-10-23 (0L21_5.tex): the user's editor does NOT always reload after I write the .tex. When the user
saved after editing in the editor (a \phantom on one line), its stale buffer replaced my last two edits
(three new slides + a colour change). The user then said "it is lost in the file, restore it". Fix: diff the
deck against my scratch copy (scratchpad n5/), re-apply my missing edits and keep the user's own new edits.
After each edit, keep a scratch copy of the deck so a stale save can be merged back.

2026-10-26 (0L22_4.3_c): mid-session the user copied 5/0L22_5.tex to a new subfolder 6/0L22_6.tex and
kept working there, so my next edit to 5/ looked "not changed yet". Before each edit, check the deck's
parent folder for a newer numbered subfolder and edit the highest-numbered copy.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-filled-versions.md ===
---
name: stat3411-filled-versions
description: "How to make the 5 'filled in after class' versions of a STAT 3411 blank deck — folder layout, template PDFs, layout-preserving tricks"
metadata:
  node_type: memory
  type: project
  originSessionId: 70ec3be6-f13a-4d5d-90c1-45e8df3464d2
  modified: 2026-10-26T12:51:15.436Z
---

Recurring request: fill a blank STAT 3411 deck as the instructor would by Apple Pencil in class,
5 versions with ink levels 1-5, one named as closest to the templates. The templates are the PDFs in
`.../26Fa_Stat/Notes/other/Bishnu/filled/` (`1.2 2.pdf`, `1.3 3.pdf`, `1.4 2.pdf`, `4.3.pdf`, `Quiz2.pdf`, `Practice Exam 1.pdf` (added 2026-10-16, the template for filling practice exams)). Their style:
pink "Solution:", blue work, then formula, plug-in and boxed answer, short pink or green notes on
concept slides. Output goes to `~/Desktop/Cl/Stat/<deck>_filled/` with the subfolders
`V1_least_ink V2_light V3_CLOSEST_TO_TEMPLATE V4_detailed V5_most_detailed` plus a README.
Earlier runs: `0L12_13_filled`, `0L13_8_filled`, `0L13_9_filled`, `0L14_10_filled` (2026-10-02),
`0L15_9_filled` (Sec 3.4, 2026-10-05; generator kept nowhere, rebuilt from the V3 tex pattern),
`0L16_5_filled` (Sec 3.4-3.5 hypergeometric / neg. binomial, 2026-10-07), `L14_10_filled` (Sec 4.1 pdfs, 2026-10-09; used a scope `shift={(current page.north west)},y=-1cm` so ink coords are page cm, y down; ghost needs `opacity=0,text opacity=0` per scope, not an `every picture` style). Fast method used there: one Python
list of ink items `(lo, hi, page, x_cm, y_cm_baseline, colour, text)` emitted into all 5 versions;
`O17_5_Q2_3_filled` (Quiz 2 review deck, 2026-10-12; template `Bishnu/filled/Quiz2.pdf` = one ink line under each part, values written into the table cells; that deck has NO tikz, so the ink went into a kernel `picture` (`\put(x,y){\rlap{..}}`, unitlength 1cm) in the same zero-height vbox, with each frame's origin measured from a marker build; the user allows extra blank pages for overflow work, and V4/V5 used them).
`Quiz2_14_filled` (2026-10-15; the printed amsart quiz `Quizzes/Q2/14/Quiz2_14.tex`, which has no frames: the ink went in `\AddToHookNext{shipout/foreground}{\unitlength=1bp \put(x,-y_baseline){...}}`, one block per page placed in the body, with x/y in PDF bp from top-left; added only `\usepackage{xcolor}`; extra pages go at the end so printed page numbers hold; the generator was saved as `make_versions.py` in that folder; LM under XeLaTeX renders ``..'' badly, so avoid TeX quotes in ink).
`0L19_7_filled` (Sec 4.2 cdf / E(X) / V(X), 2026-10-16; generator `make_versions.py` kept in that folder: items `(lo,hi,page,...)`, extra page keyed `'N+'`; a `\newcommand` with `#1` inside a frame breaks beamer, so boxed answers are inlined by regex; keep extra digits when a later answer squares a rounded mean).
`0L20_5_filled` (Sec 4.2 E/V/percentile/median, 2026-10-21; same generator pattern as 0L19_7. The user edited the source deck *while* the job ran (spacing on the Median slide, Example label colours), so cmp the source before finishing and rebuild from the latest; ghost-diff against a fresh blank built from the same copy).
`0L21_6_filled` (Sec 4.3 normal / Phi(z) / Examples 1-5, 2026-10-23; generator `make_versions.py` in that folder adds `GEO` per page (axis y, peak, centre, sigma in page cm of the printed N(282,40^2) curves) with SHADE/MARK helpers for shading the event and marking x-values in ink, as the 4.3.pdf template does; inner `opacity=` keys are zeroed in ghost mode, otherwise the fills show in the ghost build).
`Exam_1_Practice_7_filled` (2026-10-16; printed article exam `Exam/E1/Practice/7/`, 5 problems over 5 pages: one `\AddToHook{shipout/foreground}{\ifnum\value{page}=N ...\fi}` block per problem, put just before its `\item`. This is safer than AddToHookNext, which can attach to the wrong page when TeX reads ahead. In the foreground, `\normalsize` looks tiny next to a `\large` body, so all ink is `\large`. Binomials use the course table B(x;n,p). Generator `make_versions.py` is in that folder.)
`Exam_1_Practice_7_filled_PE1_style` (2026-10-16, redone after the user added `Practice Exam 1.pdf`; that template's look: "Solutions" on the Name line, work from the left margin x≈62bp in large writing (\Large), Q1 pink and the rest purple (130,33,140), ticks after answers, blue boxes, red "← from the table", green side work. Rectangles and lines use picture-mode `\framebox`/`\line`. A white ghost no longer works when ink overlaps print, so check the printed span origins against the blank instead.)
`Exam_1_key_1_filled` (2026-10-21; `Exam/E1/key/1/Exam_1_key_1.tex`, same PE1-style generator adapted; anchors are the `% ----------` comment lines above each `\item`; Lecture Time is printed there, so no dash. To check positions, compare only black spans (`color==0`), because the box() ink keeps its colour in a ghost build.)
`0L22_6_filled` (Sec 4.3 inverse normal + 4.4 exponential/gamma, 2026-10-26; generator `make_versions.py` in that folder, GEO keyed by page or '2a'/'2b' for two curves on one page, SHADE/MARK take z. The user edited the deck between my Read and my build (dropped part c), so build from a fresh copy and cmp it at the end. Keep green z-work below the arrow tail, not across it.)
`ProblemSheet_4_filled` (2026-10-28; printed amsart worksheet `ProblemSheet/4/`, 4 problems over 2 pages, normal / inverse normal / exponential / gamma; PE1-style generator `make_versions.py` in that folder plus a `tk` kind that draws tikz curves (`overlay`, x=y=1bp) inside the foreground hook: hatched normal curves, x and μ marks, green z-work; the extra page is laid out per level, so V4 has no gaps; to check positions, compare the black spans with the blank sheet.)
`0L24_9_filled` (Sec 5.3 statistics / random samples / sampling distribution of X-bar and S^2, 2026-11-02; the templates were then at `Notes/0_2026Fa/0_Bishnu/filled/` (same 4 PDFs). The only true blanks were the `\phantom` products on the Careful Examination slide and Example 7 a)/b) (E, V of X and X-bar); the Example 6 solution is printed step by step, so ink there is only side notes. Two Example 7 statement slides: a) ink went on the second (just before b). Generator `make_versions.py` in that folder = the 0L22_6 one minus the curve helpers; first draft had 3 notes running past 16 cm and 2 overlaps, so check span x1 < 15.85 cm and view V4 pages.)
get positions from the blank PDF with PyMuPDF (`fitz`, installed), split frames by regex (one frame = one page).

**Why:** the user wants the deck exactly as it looks after class. Printed content must not move.

**How to apply:** use absolute `remember picture` overlays, placed in a
`\par\nointerlineskip\vbox to 0pt{\hbox{...}\vss}` before `\end{frame}`, so centred frames don't
shift. A multi-line node anchored `base` sits on its *last* line. "Ignore the phantom" means treating
`\phantom` cells as empty and writing values in (ink colour, via `\rlap{ink}\phantom{...}` so width holds). When a column's phantoms were wider than the
values (e.g. all `\phantom{11.25}`), wrap each value in `\makebox[\inkcellw]` (set with `\settowidth`
inside the frame; there is no calc package). Check the layout by building a ghost copy with
`opacity=0` and pixel-diffing it against the blank deck. Decks without `inkblue`/`inkmagenta` in the preamble: `\definecolor` them on the first line inside each inked
frame. To circle a table entry, put a zero-width ring before it (`\makebox[0pt][l]{\tikz...\phantom{0.47178}...}0.47178`);
any TikZ in a cell still shifts later cells by <0.1pt (PDF rounding, invisible). A fully blank frame
(`\begin{frame}\end{frame}`) is writing space for the previous example; V1 leaves it blank. See [[stat3411-compile-xelatex-in-scratch]],
[[stat3411-blank-vs-annotated-decks]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-instructor-script-versions.md ===
---
name: stat3411-instructor-script-versions
description: "How to make the 5 \"instructor script slide\" versions of a STAT 3411 deck — templates, frame format, folder layout, levels"
metadata:
  node_type: memory
  type: project
  originSessionId: c4acc650-a1a3-4a61-975e-3ae4bacf6163
  modified: 2026-11-02T13:23:31.763Z
---

Recurring request: add an instructor script slide after every original slide of a STAT 3411 deck,
5 versions at detail levels 1-5, one named as closest to the templates. The templates are the
`.tex` files in `.../26Fa_Stat/Notes/0_2026Fa/0_Temp/T_*/1/` (T_0L5, T_0L13, T_L1, T_L2, T_L5, T_L8_1018).
Their style: `\begin{frame}[t]{I: <Title> (Instructor Script)}` (T_0L5/T_L5 say `(Instructor Note)`, T_L8 says `Instructor Notes: <Title>`) + `\scriptframe` (10pt, no indent),
short spoken paragraphs, displayed math for key steps, one red `\Q{...}` question followed by its answer,
~100-150 words. Copy the `\Q` and `\scriptframe` defs into the preamble before `\begin{document}`.

Output: `~/Desktop/Cl/Stat/<deck>_script_5versions/` with subfolders `V1_detail1_least V2_detail2
V3_detail3_MATCHES_TEMPLATE V4_detail4 V5_detail5_most` (tex+pdf) and a README. Runs: `0L15_106`,
`0L16_106` (2026-10-07), `L14_110` (Sec 4.1, 2026-10-09), `0L18_105` (Sec 4.1-4.2, 2026-10-14; builder kept the text as V1/V2/V3 lists + V4 extra-before-\Q + V5 = V3 slide + extras slide), `L15_107` (Sec 4.2 cdf/E/V, 2026-10-16; V4 overflowed twice at ~150 words with 3 displays, fixed by shortening the extra to one line), `0L20_107_2` (Sec 4.2 E/V/percentiles/median, 2026-10-21; templates now also include T_0L13, whose `\Q` uses dark red `redHdr`; ink-only "Note:"/"Meaning:" frames got their own scripts; 19 scripts, no overflow first try with V3 ~125 words and 2 displays), `O17_5_Q2_104` (Quiz 2 review, 2026-10-12; slides with no frame title get a short script title, and slides whose ink is only a `\AddToHookNext{shipout/foreground}` still render correctly with a script frame spliced in after them). `0L21_108` (Sec 4.3 normal, 2026-10-23): user now asked for a script after the TITLE slide too (review of 4.1-4.2 + preview); 18 scripts, builder in scratchpad s21/ (scripts.py dict v1/v2/v3/extra/q2 + build.py), no overflow first try (V4 max bottom 8.17 cm). `0L22_107` (Sec 4.3 inverse normal + 4.4 gamma family, 2026-10-26): deck had TWO title slides (4.3 cont., 4.4), both got scripts; 19 scripts, builder scratchpad s22/ reused s21 build.py/check.py via sed (SRC, frame count, file prefix), no overflow first try (V4 max bottom 7.90 cm). `0L24_110` (Sec 5.3 statistics/sampling distributions, 2026-11-02): 40 frames incl. title, 24 of them untitled Example 6 step-build frames (each pair/grouping got its own short distinct script, e.g. "I: Step 3: Pair $(10,15)$"); builder scratchpad s24/ (scripts.py add(t,v1,v2,v3,ex,q2,cl) + build.py + check.py, which also confirms the non-script page text equals the original PDF); no overflow first try (V4 max 8.41 cm). Old scratchpads get wiped, so expect to rewrite the builder. Measured levels: V1 ~27 words, V2 ~45, V3 ~120, V4 ~150 (fills the slide), V5 two
script slides per original (`, 1/2` and `, 2/2` in the title). No script for title pages unless asked (0L21_108 asked for one). Keep script titles short: a title that wraps the band ("Examples of Continuous Random Variables") breaks the look; check page text line 1 contains "(Instructor Script".

Follow-up request (2026-10-07, `0L16_1008`): "add natural questions to the script slides, in red" —
added 1–2 extra `\Q{}` + short answer per script slide (predict/guess, your-turn table lookups,
"would X surprise you?"), making displays inline where a slide was full. Output to the work dir as
`<deck>_scriptQ.tex` (with scripts) and `<deck>_noscript.tex` (script frames stripped), and list the questions in chat. Repeated 2026-10-16 on `L15_1009` (Sec 4.2, 34 new Qs, 22 script frames): 3 pages overflowed after adding 2 Q+A, fixed by inlining one display each; also output the PDFs alongside.

**How to apply:** keep the script text in a Python dict and splice frames in by regex. Beamer clips
overflow silently, so check each script page with PyMuPDF using an unbounded clip rectangle (text bottom
above ~8.75 cm). See [[stat3411-compile-xelatex-in-scratch]], [[stat3411-filled-versions]].
Repeated 2026-10-21 on `0L20_1008_2` (Sec 4.2 E/V/percentiles/median, 19 script frames, 27 new Qs incl. one that replaced a sentence): no overflow first try (max bottom 8.58 cm) after inlining one display on the Example 10 script; output `<deck>_scriptQ`/`<deck>_noscript` tex+pdf.
Repeated 2026-10-23 on `0L21_1010` (Sec 4.3 normal, 18 script frames incl. title, 30 new Qs, builder scratchpad q21/build.py with anchor-text EDITS list): 3 pages hit 8.74-8.79 cm; fixed by inlining 2 displays and dropping 1 duplicate Q, max then 8.64 cm. "Before computing / guess first" Qs go before the computation, "your turn" Qs after the answer.
Follow-up 2026-10-23 on `0L21_1010`: "mark the sentence where the instructor raises their head to engage students, in dark green" — one passage per script frame wrapped in `\Look{...}` (`lookgreen` RGB 0,100,0, `{\color..}` so it can span lines), never touching the \Q lines or original slides. Picks: real-world examples, "why" questions to the room, "in words" takeaways, interpreted results ("95.54% of students…"). Output `<deck>_engage.tex/pdf` in work dir; page text identical to the original, so no overflow risk.
Repeated 2026-10-23 on `0L21_1011` (= the user's edited 1010 deck, unmarked): 18 `\Look` passages, builder scratchpad e1011/build.py (start/end anchor pairs, asserts the anchor is unique AND inside an Instructor Script frame — the 68.26% sentence also appears on the original slide). Swapped in reassurance/recall lines ("We will not compute with this formula directly…", "the table only gives areas to the left… total area is 1", "how do we connect the two?").
Repeated 2026-10-26 on `0L22_1007` (Sec 4.3 inverse normal + 4.4 gamma, 19 script frames, 37 new Qs, builder scratchpad q22/build.py with is_in_script() anchor check + INLINE display list): measured originals first (5.6-7.0 cm, so 2 Q+A fit after inlining 4 displays); only the Gamma Family frame overflowed (8.95 cm) and needed one Q dropped — a shorter Q doesn't help unless it saves a whole line. Nonscript = strip every `\begin{frame}[t]{I:` frame plus all "INSTRUCTOR SCRIPT FOR" banners (some banners sit above non-script frames); verified its page text equals the original's non-script pages.
Repeated 2026-10-28 on `0L23_1006` (Sec 4.4 gamma/cdf/chi-squared, 8 script frames, 16 new Qs, builder scratchpad q23/build.py copied from q22): originals 6.2-6.8 cm; only the Gamma Distribution frame overflowed (8.88 cm), fixed by inlining its pdf display; max then 8.64 cm. Gotcha: anchors with a line break need a non-raw string ("...\n\t..."), r"\n" never matches.
Repeated 2026-10-28 on `0L23_1006` (Sec 4.4 gamma/cdf/chi-squared, 8 script frames): builder scratchpad e23/build.py; one anchor ("About 54% of calls…") also sits in the original slide's ink node, so the builder now picks the single hit inside an `I:` frame. Verified page text identical to the original and green spans via PyMuPDF color 0x006400.
Repeated 2026-11-02 on `0L24_1010` (Sec 5.3 statistics/sampling distributions, 40 script frames incl. title, 47 new Qs, builder scratchpad q24/build.py with end()/before()/repl() edits by frame-title prefix): inlined 3 displays (Random Samples, Examples 2-3, Most Common Statistics) up front; only Random Samples hit 8.72 cm and was fixed by shortening its Q to save a line; max then 8.64 cm. Rule of thumb: one Q+A adds ~1.0-1.5 cm, so 2 Qs on frames under ~5.6 cm, 1 Q otherwise.
Repeated 2026-11-02 on `0L24_1010` (Sec 5.3, 40 script frames incl. title): 40 `\Look` passages, builder scratchpad e24/build.py (PICKS start/end-or-None pairs; asserts one hit inside an `I:` frame, one pick per frame, no \Q/display/paragraph break inside). Verified with PyMuPDF: page text identical to the original, non-script pages pixel-identical, green 0x006400 on all 40 script pages only.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-macro-source-0l12-108.md ===
---
name: stat3411-macro-source-0l12-108
description: Section 3.1 slide macros missing from a deck (e.g. \hxa/\hya coin-flip highlights) are defined in the preamble of L9_3.1_c_m/108/0L12_108.tex
metadata:
  node_type: memory
  type: reference
  originSessionId: 258aa313-998d-40ef-8b8c-5007b955672e
  modified: 2026-09-28T12:44:36.871Z
---

When a slide the user pastes into a Section 3.1 deck (e.g. `0L12_c_m/1008_cl/0L12_1008.tex`) uses a
macro the target preamble lacks, look first in
`~/Desktop/dt/Teaching/Teaching/UMD_T/26Fa/26Fa_Stat/Notes/0_2026Fa/L9_3.1_c_m/108/0L12_108.tex`.
Its preamble holds the coin-flip highlight variants (`\hxa`/`\hya` = version a, plain coloured
letters in lectureblue / fbred; `\hxb`… = highlighter-box versions) plus `inkblue`/`inkmagenta`.
Copy the needed definitions verbatim so the decks stay in sync. See [[stat3411-compile-xelatex-in-scratch]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-normal-notation-N.md ===
---
name: stat3411-normal-notation-N
description: "In STAT 3411 slides write normal distributions as N(mu, sigma^2), never Normal(...)"
metadata:
  type: feedback
---

Write a normal random variable as `X \sim N(\mu,\sigma^2)` (e.g. `N(1,0.25)`), not `\text{Normal}(...)`.
The word "Normal" in prose or titles ("a Normal pdf", "Normal rv's") is fine.

**Why:** the user asked on 2026-10-23 (0L21_5.tex, the μ/σ² plot captions) to replace Normal(...) with N(...).

**How to apply:** use N(...) in any new or edited STAT 3411 slide; colour it magenta per [[stat3411-text-color-pattern]] when it is the notation line.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-observed-vs-random-sample.md ===
---
name: stat3411-observed-vs-random-sample
description: "STAT 3411 Ch.5 slides say \"observed sample\" (lowercase x) vs \"random sample\" (capital X_i), never \"(a priori) sample\" or \"sample-to-be-collected\""
metadata:
  node_type: memory
  type: feedback
  originSessionId: 211b9496-d6cd-4a84-b1f1-bf65337324cc
  modified: 2026-11-02T12:09:20.821Z
---

On 2026-11-02 (0L24_8.tex, Sec 5.3) the user replaced "(a priori) sample" / "sample-to-be-collected"
with **observed sample** (x: x_1,…,x_n, numbers, already collected) vs **random sample**
(X_1,…,X_n, random variables, not yet collected). The "Random Samples" slide now reads "inference
methods need a random sample X_1,…,X_n to satisfy two conditions" (identical, independent), and the
statistics table header is OBSERVED SAMPLE | RANDOM SAMPLE.

**Why:** they picked this pair from a list of suggested names; it matches "observed value" for statistics.

**How to apply:** use these two terms in any new STAT 3411 slide or version; pair with
[[stat3411-say-possible-values-not-support]] style wording.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-quiz-no-color.md ===
---
name: stat3411-quiz-no-color
description: "STAT 3411 quiz tables are black-and-white grids — no shading, no red/colored values, no xcolor"
metadata:
  node_type: memory
  type: feedback
  originSessionId: f604546a-03fd-4490-b941-2e41da0c72e5
  modified: 2026-10-11T22:16:08.800Z
---

On 2026-10-11 the user asked for quiz pmf tables "like" a WebAssign screenshot (gray label column,
red values). They then had me remove the shading, then the red, and said "don't use color".

**Why:** the quizzes are printed handouts.

**How to apply:** when copying a table style from a screenshot, take only the layout (full grid,
centered cells, bold $x$ / $p(x)$ labels, taller rows) and leave out every color. Load `array`
for column types, not `[table]{xcolor}`. See [[stat3411-quiz-versions]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-quiz-versions.md ===
---
name: stat3411-quiz-versions
description: "STAT 3411 quizzes copy the doc-1 amsart layout (Bishnu Q2); \"5 versions\" = folder with V1..V5 subfolders, each with its own tex+pdf"
metadata:
  node_type: memory
  type: project
  originSessionId: f604546a-03fd-4490-b941-2e41da0c72e5
  modified: 2026-10-11T21:40:21.488Z
---

On 2026-10-11 the user had me build Quiz 2 (Sections 3.2–3.3) from HW5 problems, styled like
`Quizzes/Bishnu/Q2/1/2411_F_26_1_1.tex` (amsart 12pt, underlined Name/title header, `\item[(1)]`
numbering, `\vspace` answer areas, 2 pages, 3 problems). They then asked for "5 more versions,
different in each": made `Quiz2_Sections_3.2-3.3_5versions/V1..V5/Quiz2_Vk.{tex,pdf}` in the
work directory, with a new context and new numbers per version and the same problem types.

**Why:** they print the versions as separate quiz forms.

**How to apply:** keep each version at exactly 2 pages (check the page-1 overflow with a render).
Fill the doc-1 blank pmf table with as many columns as the variable has values, and use clean
numbers (check them with Python). Compile in scratch per [[stat3411-compile-xelatex-in-scratch]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-redo-examples-in-template-style.md ===
---
name: stat3411-redo-examples-in-template-style
description: "Redo the ink on a few already-filled slides to match a specific template problem, 5 versions, other slides untouched"
metadata:
  node_type: memory
  type: project
  originSessionId: 3d0f2b01-80fb-40c7-8273-3d703e20c44d
  modified: 2026-10-23T16:12:22.162Z
---

On 2026-10-23 the user pasted two frames of an already-filled deck (`0L21_109.tex`, Examples 4-5, ink in ordinary
type in `% In-class additions` overlays) with the request "make the solution in the style of the corresponding problem in the pdf"
(`0_Bishnu/filled/1.4 2.pdf`), plus the usual 5-version fill-in prompt. Matching: the tex Example 4 (300-350) went to the PDF's
Example 4, and the tex Example 5 (within 1 sigma) went to the PDF's Example 1 (the 68-95-99.7 rule). The previous ink in those
frames, including the shading/marks scopes inside the figure tikz, was replaced. All other frames were copied byte for byte.
Output went to `~/Desktop/Cl/Stat/0L21_109_filled/`, with `make_versions.py` there. Named nodes chained with `anchor=base west`
let the red arrows go from each P(X<=x) term down to its z.

**Why:** the user wants only the named slides restyled, with the rest of the filled deck untouched.

**How to apply:** replace whole frames by marker comment. Then pixel-compare the untouched pages against a scratch build
of the source. See [[stat3411-filled-versions]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-restyle-to-doc1-format.md ===
---
name: stat3411-restyle-to-doc1-format
description: "How \"put doc 2 in the format of doc 1\" was done for STAT 3411 decks (band titles, blue Example labels, 12pt) without touching content"
metadata:
  node_type: memory
  type: project
  originSessionId: 81a2e509-fc3a-43e3-9e42-c6c53df9c753
  modified: 2026-10-04T13:21:45.435Z
---

On 2026-10-04 the user asked to restyle the Section 4.1 deck (L14_4.1/4/L14_4.tex, "doc 2")
into the format of the Section 3.3 deck (0L14_3.3/13/0L14_13.tex, "doc 1"): titles in the
pale-blue band, examples starting with the blue label, same font size/style, and the
content left unchanged.

Recipe used:
- Use doc 1's preamble as-is: 12pt, lmodern, band frametitle, triangle bullets, `\fitO` as a no-op, `\exlists`. Keep only the old deck's own macros and its Plt figure colours.
- Example frames: `\begin{frame}[t]{}\fitO\exlists` then `\textcolor{blue}{\textbf{Example N:}}\,\ text`. Replace tcolorbox "Example." boxes the same way. Solution/continuation frames also get no title. Theory frames keep their title in the band; trailing periods in titles were dropped.
- Delete trailing `\vspace*{±pt}` pads (see [[beamer-negative-vspace-masks-clipping]]). Fix the overflow caused by the bigger type by zeroing hand-set `\vspace{mm}` gaps or by splitting the frame at a natural break. Check by word-diffing the old and new bodies.

**Why:** the old 10pt Times decks overflow at 12pt, and the user said "don't change the content".
**How to apply:** use this recipe for the next "format of doc 1" request; compile in scratch as in [[stat3411-compile-xelatex-in-scratch]]. Related: [[stat3411-blank-vs-annotated-decks]].

2026-10-05: template = `0L14_3.3/13/0L14_13.tex`, doc 2 = `0L15_3.4_c_m/8/0L15_8.tex`. Examples were
numbered 1..7 in deck order, parts became `Find` + `\begin{itemize}\item[a)] ...\\[0.2cm]` (from
`enumerate[(a)]`; the template itself has no parts — that style comes from sibling `0L14_3.3/5`),
`\textbf{Find:}` → plain `Find`, and a bulleted example's first bullet moved onto the label line.
An example frame that had a topic title kept it in the band. The user said "make suggested changes when
you think appropriate", so apply small style fixes like these directly and list them in the reply.

2026-10-06: template = `0L15_3.4/11/0L15_11.tex`, doc 2 = `L13_3.5-3.6_c_m/4/L13_4.tex` (Sec 3.5–3.6).
Same recipe; unnumbered tcolorbox examples and old "Example 4/5/6/9/10" were renumbered 1–9 in
deck order (textbook refs like "(Ex. 71)" kept). Frames holding a derivation plus an example were
split at the example so every example opens its own `[t]{}` frame (two overflowed at 12pt anyway).

2026-10-10: template = `0L17_3.5-4.1/9/0L17_9.tex`, doc 2 = `L15_4.2_c/3/L15_3.tex` (Sec 4.2). Same recipe
(doc 1 preamble lines 1–113 + the deck's own macros). 8 tcolorbox examples + old duplicate-numbered
examples renumbered 1–23 in deck order; in-text cross-refs ("For the Example 7…") updated to the new
numbers. No overflow at 12pt. Editor did not auto-rebuild (file not open), so the scratch XeLaTeX PDF
was copied into the folder.

2026-10-10 (batch): same template applied to L16_4.3/3 and L17_4.4/3 ("highest-numbered subfolder"
of each). L18_5.3-5.4/2 was done too, then the user said "don't do L18" — it was restored to the original. Extra rules used: drop per-frame `\small` on itemize/formulas and `\sffamily`;
remove L18's `bodybox` text frames (kept frameboxes around figures/formulas); hand-set a)/b) lines →
`\item[a)] …\\[0.2cm]`; "Use Table A…"/"Use the following formula…" step slides keep an unnumbered
**Example:** label. Gotcha: at 12pt `\tiny` tikz labels grow (5→6pt) and LM math is wider, so the L17
legend overflowed — fixed with `\fontsize{5}{6}` and a wider legend box. In python re.sub replacement
strings, escape backslashes (`\t`,`\v`,`\b` became control chars once).

2026-10-10 (L18_3, L19_2, L20–L25): same recipe applied to L18_5.3-5.4/3, L19_6.1/2, L20_7.1/2,
L21_7.2/1 … L25_8.2/1. Notes: the PDFs shipped in L20–L25 folders were stale (different example
text than the .tex), so compare against a scratch build of the original .tex, not the folder PDF.
Frames may use `\fitI`/`\fitII` — define them as no-ops. Grey callout `\colorbox{black!8}` text
around example questions was dropped; H0/Ha boxes, computation frameboxes and figure boxes kept.
Figures whose `\small`/`\scriptsize` labels collide at 12pt get pinned with `\fontsize{9}{11}` /
`\fontsize{7}{8.5}`. Pre-existing defect left alone and reported: `\cibar` bars with
`\hspace*{-0.75cm}` (L20) and the L22 t-CI/Z-CI bars clip their left labels off the slide.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-say-possible-values-not-support.md ===
---
name: stat3411-say-possible-values-not-support
description: "In STAT 3411 slides, write \"possible values\" instead of \"support\"/Supp(X)"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 76742e60-af90-4198-ad58-dad92a5c8849
  modified: 2026-10-09T11:45:32.219Z
---

On 2026-10-09 (0L17_5.tex, Section 4.1 "Continuous r.v.'s & Their Supports" versions) the user said
"don't use support, use Possible values". The new versions dropped the Supp(X) notation and the
support definition; their titles, table headers and notes say "Possible values".

**Why:** the course doesn't use the term "support" with students.

**How to apply:** in new or rewritten STAT 3411 slides, write "possible values of X", not "support"
or Supp(X). Leave the user's own original frames alone unless asked. Related: [[stat3411-slide-versions-plain]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-slide-versions-plain.md ===
---
name: stat3411-slide-versions-plain
description: "When asked for N alternative versions of a STAT 3411 slide, keep every version plain — no decorative TikZ pictures, shading or braces"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 88a6eb69-a7a7-4a64-91d1-366fd1f1171f
  modified: 2026-10-04T23:11:45.139Z
---

When the user asks to "improve / make cleaner / give 3 versions" of a STAT 3411 slide, every
version must stay plain. On 2026-09-28 a version that drew the five computer boards as shaded
TikZ boxes with "defective"/"good" braces (Ex. 15) got "don't make fancy"; it was replaced by a
single text line. Also accepted in that session: booktabs or boxed `|l|ccc|` tables, blank
pgfplots axes for students to draw on, `\phantom` fill-in cells.

**Why:** they want clean, concise teaching slides, not illustrations.

**How to apply:** vary versions by wording, table style and fill-in layout — not by added
graphics or colour fills. Put new versions right after the original frame, one slide each. The
user then deletes the ones they don't want; match the version they kept on the next slide.
On 2026-10-02, "make this slide more complete" (the E(aX+b) slide) first got crowded versions
(underbraced proof, temperature example, parenthetical asides). The user answered "keep it clear
and clean", "and concise", "could be relevant comments". So when adding to a slide, add 2-3
one-line comment bullets (special cases, what the rule means, a common mistake). No proofs or
worked examples unless asked.

Exception (2026-10-04, cdf-table lookup slide in 0L15_5.tex): the user asked to "color both the
row and column" so it's clear which entry is read. Light colortbl shading (column lectureblue!15,
rows examplerust!20, crossing cell lectureblue!45) or coloured text is fine for that purpose;
`\usepackage{colortbl}` is now in that deck's preamble (installed in TeX Live 2026).

See [[stat3411-blank-vs-annotated-decks]], [[stat3411-compile-xelatex-in-scratch]].

Second exception (2026-10-05, "Tree Representation" slide in 0L15_8.tex): after plain versions,
the user asked for "3 more versions, add some colors, make it clear". Colour that carries meaning
worked: D/defective in exampleberry, G/good in examplegreen (branches, letters, key), lectureblue
headers, paleblue column bands. The `\dgtreecolor` macro in that preamble does this. So plain is
the default, and when colour is requested, use it to encode categories, not to decorate.
A third round on that same slide was "don't make it so plain, but also not so fancy". The answer was
light structure: circled D/G nodes (12% fill), pale-blue rounded header tags, a rounded pale-blue key
box, and alternating pale-blue row stripes on a list. The styles are `circnodes` and `headtag` in that
preamble. Still no shadows, gradients or pictures.

Kept on 2026-10-05 (0L15_8.tex "Using the Multiplication Rule"): of 3 versions, the user kept the
one with bullets + formula on the left and a plain black tree on the right where the relevant path
is drawn thick in lectureblue (`hlpath` style, `\twotree` / `\threetree` macros in that preamble).
Use that look for later tree slides in this deck.
`\threetree` leaves sit at y = ±3.1, ±2.3, ±1.3, ±0.5 (DDD at top); put leaf-side labels at x ≈ 6.85+.

2026-10-09 (0L17_7.tex "Recall: Discrete vs. Continuous"): asked to "improve the part about chap 3, 4",
versions that summarised each chapter (pmf/sums vs pdf/areas, binomial examples) got "don't mention the
content of chap 3, 4". A chapter pointer should stay a pointer: improve its wording/placement
("Studied in Chapter 3 (done)", a right-aligned chapter tag, a Chapter column), don't preview material.

2026-10-09 (0L17_7.tex uniform-pdf example, "make the problem part and the graph part the same height
level"): of 3 layouts the user kept "columns[c] + graph shortened to the list's height + items spaced
\\[0.35cm]" over "pdf moved into left column" and "[T] with graph top level with Find". Lead with that
layout for later problem-list-plus-graph slides.

2026-10-21 (0L20_3.tex, "add a picture to illustrate the 100p th percentile, to the right"): of 3
versions (columns 0.56/0.42, text left unchanged, picture right), the user deleted the minimal one
(shaded pdf, label "p") and kept two: pdf with dashed line at eta(p) and both areas labelled
("area = p", arrow to "area = 1 - p"), and a stacked pdf-over-cdf pair (dashed p -> eta(p) on the cdf).
Pictures use the deck's \gcurve/\gshade/gcdf (Gamma(3)) and areaFa!35 shading. The Median slide
followed those two looks plus a skewed-vs-symmetric stacked pair.
Later the same day the user cut further, keeping only the percentile "both areas labelled" slide and
the median skewed-vs-symmetric slide, and asked for the 90th-percentile example picture "using pdf"
(not a cdf panel). So for percentile/median pictures, lead with the pdf and labelled areas; skip the cdf.

2026-10-23 (0L21_5.tex "Normal Random Variables" examples list): of 3 versions the user kept the plain
shortened-bullet one, then cut it further themself: 4 examples (dropped GPAs, Brownian motion) and the
closing line became "The normal distribution is the basis for many statistical methods." (no Ch. 5–9
pointer). So for intro/motivation slides, aim even shorter than a first "concise" pass.

2026-10-26 (0L22_5.tex "The Gamma Distribution", Sec 4.4): of 3 versions the user kept the one with
"written X ~ Gamma(α, β)" in the definition line, a one-sentence "where α>0 is the shape … β>0 is the
scale parameter" under the pdf, a one-line comment bullet, and E(X), V(X) in an \fbox. They changed
\textbf{gamma distribution} to \keyterm{…} and added an "\item Mean and variance:" bullet above the box.
So use \keyterm for defined terms in this deck's new versions.
Same deck, gamma-cdf slide: of 3 bullet versions the user kept "definition first" (P(X≤x)=F(x/β;α),
F(x;α) integral displayed, \keyterm{incomplete gamma function}, a no-closed-form bullet, "read from
Table A.4 or software").

2026-10-26 (0L22_7.tex exponential pdf & cdf plots): "give only 3 curves … make it look more clear … 3 versions"
→ λ = 0.5, 1, 2 as pgfplots expressions (no yellow), and mid-task the user said "remove the grey rectangle
background": don't use the ggplot grey panel (ggstyle fill) on new plot versions; use white with light black!12 grid.

2026-11-01 (0L24_5.tex "Population and Sample", "add a quick example at the end, 3 versions"): of
bullets / table / fill-in-blanks, the user kept the bullets layout but with the light-bulb example and
empty "Population:" / "Sample:" bullets (blanks for class). For quick definition examples, lead with
example + labelled bullets left blank.
Same deck, "A Sampling Distribution" ("improve, consider adding relevant content"): of comments-bullets /
definition + population-vs-sampling comparison table / center-spread-shape bullets, the user kept the
two-bullet definition + booktabs comparison table (Describes / Random variable / One value per).
2026-11-01 evening: the user moved the 5.3 deck to 0L24_5.3_c/6/0L24_6.tex (renamed file, not just folder);
the population-distribution versions were all removed there. Always look for the newest numbered folder's .tex.

2026-11-02 (0L24_7.tex dishwasher-soap X-bar/S^2 example): after 12 "change the setting and numbers" versions
the user kept only the streaming-plan one, then asked "change the numbers … keep the setting": same wording,
only the pmf table changes, and "the setting" meant the kept streaming-plan slide, not the original dishwasher one
(when the user has kept a version, "keep the setting" refers to that version). Pick values/probs so the X-bar and S^2 tables come out clean (n=2: s^2=(x1-x2)^2/2),
and give an answer key in chat. When inserting with `sed 'Nr'`, print line N first and make sure it is the
frame's `\end{frame}`, because the user may have deleted slides since the last edit (an insert one line too
high once broke the live build).

2026-11-02 (0L24_9.tex, Example 7 b) pmf slide, "improve, make it look nice, text on top, give 3 versions"):
mid-task the user said "don't add new content" after I drafted a "We use this pmf to find …" line and a
"Check: probabilities sum to 1" line. For "make it look nice" requests, vary only layout/wording/table style
([t] frame, centered table, grid vs. ||-header); add comment bullets or questions only when the user asks
for more content.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-table-slides-no-title.md ===
---
name: stat3411-table-slides-no-title
description: "Phi(z) table-lookup slides in STAT 3411 decks get no frame title (max space), full table kept, tikzmark names must be unique"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 1c80ea1f-10b4-4b2d-8022-230db047f4e9
  modified: 2026-10-23T10:46:48.994Z
---

On 2026-10-23 (0L21_3.tex, Sec 4.3), for the copied Engwer "Computing Phi(z)" table slides the user said
"don't use title for the slide so that the usable space of the slide is bigger", "make the table bigger so
that it is visible", "don't remove entries from the table", and wanted a line stating Phi(1.45)=... .
They had already stripped the titles and deleted the excerpt-table versions I made for Phi(-1.23).

**Why:** the full 31-row z table only becomes readable if it gets the whole slide height.

**How to apply:** for z-table slides use `\begin{frame}` with no title, keep all rows/columns, grow the
font as far as height allows (about 5pt beside a pdf plot, about 6pt with no plot), and add a one-line
Phi(z)=P(Z<=z)~value statement. Give every `\tikzmark` a name unique in the whole deck. The Engwer slides
reused ctop/rl/cell on several frames, so the green marks landed on the wrong cells. Fix that by adding a
suffix per slide. See [[stat3411-slide-versions-plain]], [[stat3411-compile-xelatex-in-scratch]].

2026-10-23 (0L21_5.tex, "combine" the Phi(z) intro slide with the table): splitting the 31-row table into
two side-by-side halves does not fit. Each half is about 8 cm wide even at 6pt, and the text width is 14.9 cm, so they stack.
What fits: no title + one-line note on top at 5.8/6.6pt; or table in a 0.6 column (6/7.2pt, or 5.2/6.0pt with
a frame title) beside a short note. A full-width one-line note must stay under about 14 cm, or it wraps and pushes row 3.0 off the slide.

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-text-color-pattern.md ===
---
name: stat3411-text-color-pattern
description: "Text color roles from Bishnu Section_4.3 deck (\"doc 1\" on 2026-10-21) used to recolor STAT 3411 slides"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 19b0a05a-4d52-4980-ac97-7c00c079716c
  modified: 2026-10-21T12:46:38.422Z
---

When asked to make a deck's text colors "match the pattern of doc 1" (Notes/other/Bishnu/4.3/Section_4.3.tex), use these roles with xcolor named colors:
- key terms: \keyterm = examplegreen bold
- main defining formula: blue (e.g. E(X)=∫x f(x)dx, F(μ̃)=0.5)
- working/shortcut formula: red (e.g. V(X)=E(X²)−[E(X)]²)
- notation symbols: magenta (μ, μ_X, η(p), μ̃, (SD))
- small annotations like "random variable": red; key phrases ("to the left"): \textbf black
- leave Example labels and figures untouched

**Why:** user asked on 2026-10-21 to recolor 0L20_5.tex this way, keeping \textcolor{blue}{\textbf{Example n:}}.
**How to apply:** color only, no layout change; verify with a scratch XeLaTeX build ([[stat3411-compile-xelatex-in-scratch]]).

=== ./-Users-cone-Desktop-Cl-Stat/memory/stat3411-tiny-changes-typos-only.md ===
---
name: stat3411-tiny-changes-typos-only
description: "\"Improve the slide, only tiny changes\" on a STAT 3411 deck means fix typos and grammar only — no wording, layout, math-mode or content edits"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3bb76992-41c7-4602-b826-d72f686979f9
  modified: 2026-10-02T12:55:32.535Z
---

On 2026-10-02 the user asked to "improve the slide, make only small changes, maybe only tiny
changes" for 0L14_12.tex. A pass that also converted plain-text X/p(x) to math mode, added
"in general", changed a frame to `[t]`, extended a bullet and changed σ to σ_Y was rejected:
"don't change the content, only fix typos and grammar". The accepted result was 3 lines
(missing commas around a "denoted by" clause, "is denoted by … is given" → "denoted by … , is
given", "definition for" → "definition of", missing colon).

**Why:** they want the deck to stay as written; a "tiny changes" request is a proofread.

**How to apply:** for such requests, edit only spelling, grammar and punctuation. Mention other
possible improvements (math-mode consistency, ambiguous symbols) in the reply as suggestions,
not edits. Output to the work directory and show a diff-style change list.
See [[stat3411-slide-versions-plain]].

=== ./-Users-cone-Desktop-Cl-Stat/memory/webassign-merge-homework-quietly.md ===
---
name: webassign-merge-homework-quietly
description: How to merge WebAssign assignments (STAT 3411) in Chrome without popping windows in front of the user
metadata:
  node_type: memory
  type: feedback
  originSessionId: 75ca77f1-a9ff-4fb6-af38-f92c0eb89eff
  modified: 2026-10-29T17:16:51.271Z
---

The user works on other things while WebAssign tasks run and said "don't pop up to distract me". Avoid clicking EDIT ASSIGNMENT in the previewer (opens a new tab) and the legacy editor's "Question Browser" (opens a popup window).

**Why:** windows and tabs coming to the front interrupt the user's other work.

**How to apply:** use one background tab and JS. Open `/v4cgi/faculty.pl?action=assignments/edit&aid=<id>`. QIDs show in the page's innerText after about 2s. Click the "Duplicate" button with JS to get an unsaved copy with aid=0, which keeps the template (26Fa_Stats) and settings. Then call `Assignment.Editor.getInstance().contents.updateContents('<qid,qid,...>', aid, 'both','both','short','total', undefined,'edit')`. Then set `#assignment_total` to 10 and call `contents.distributePoints()`. Set the `assignmentName` field, then click Save with JS. Format convention: "HW n", total 10 points split equally, problems in Devore order.

**Merged-HW naming (2026-10-23, HW 8):** duplicate HW 4 (aid 26945354, "HW 4 (Sections 2.3-2.5)") as the format source. Name the merge "HW n (Sections a-b)", e.g. "HW 8 (Sections 4.2-4.3)", with total 10 even if the sources had odd totals. After Save, the page header and URL still show the source assignment. Read the hidden `[name=aid]` field to get the new aid, then reload that aid to verify.

**Optional Practice copies (2026-10-23):** "Optional Practice n (Sections a-b)" duplicates the current HW n, with total 0 points like Optional Practice 5 (aid 26964900) and Optional Practice 7. Re-read HW n first, because the user often trims it right after the merge and trashes copies that are out of date.

**"All the other problems" practice sheets (2026-10-29, OP 9 aid 27012398):** list a section's pool with questions/search (textbook 1329, code "DevoreStat9 4.3.", then click the first "Search" button with JS). Exclude by Devore number, not QID (a0 copies count as used), and use one version per number, skipping .SA variants. The user confirmed: leave out the 4.3.ST.* / SYS.* add-on items.

**Gotchas (2026-10-09):**
- Read the assignment's current question list and "Last Saved" time fresh before each edit. The user edits assignments between requests (they trimmed HW 6 to 7 questions with a 5.81 total), and a stale QID list would overwrite their work. Keep their total.
- Click Save, then wait about 6s before navigating. Navigating right away cancels the save.
- The page uses MooTools, so `new Event()` throws an error. Fire change events with `document.createEvent('HTMLEvents')`.
- To edit a publisher question: open `faculty.pl?action=questions/edit&qid=<id>` and click Duplicate with JS. Then edit the `question`, `answer` and `code` (name) textareas and Save. The new QID appears in the header. Naming: append "a0" to the Devore name, e.g. "DevoreStat9 3.4.049.S.MI.a0" (the user said "anumber", which means a0, a1, …). MI questions include a Master It tutorial: remove the part from the premise and from its <step>s too, and drop the matching answer-key lines (one line per <_> blank, in order).

**Removing parts from a question (2026-10-16, HW 7):**
- Remove whole `<div class="subblock">` blocks, finding where each ends by counting nested divs. Do this in the main premise and in the MI `<premise>`. Also remove each `<step>` for that part, plus any unlabeled steps that follow it. Then relabel the remaining sublabels a, b, c… in order, including the "(x)" label inside each kept step.
- Answer key: a pulldown answer spans several lines. To group lines, strip the leading `<EQN #Tut>`; a line starts a new answer only if it then begins with `<SECTION>` or `<EQN`. Before saving, check that the number of groups equals the number of `<_>` blanks.
- Map the question before cutting. List the sublabel, `<step>` and `</premise>` positions, with a running `<_>` count at each, to see which answer lines each part owns. Also search for "part (x)" cross-references in the kept steps, relabel them, and check the answer variables ($ansa, $t_zc …) still follow the order of the remaining parts. The Assignment Preview loads the MI tutorial only on click, so check the tutorial in the source.
- When a removed part asked for a constant, e.g. "find k", and the user says "give k", write its value straight into the formula, e.g. `\frac{<eqn $ansa>}{x^{$beta}}`. Do not add a "where k = …" line. Do it in the MI premise too, using the tutorial's own `$t_` variable, and reword kept steps that say "From part (a), we determined…".
- Rebuild the answer key from the original's grouped lines (fetch the original edit page with DOMParser), then check that the group count equals the `<_>` count before saving.
- Multi-Mode questions (`[name=mode]`, e.g. "AN") have one mode letter per answer-key `<SECTION>`. If you remove a `<SECTION>` or the section that a letter covered, update the mode string, plus the hidden `defaultMode`, so they match. If every remaining answer is numeric, just set it to "N". When this is wrong, the question renders but the preview shows no key values (seen 2026-10-23 on 4.2.022.a0). After saving an edit, always check that the preview shows the key values.
- Swapping in new questions with updateContents resets the assignment total, which counts parts. Set `#assignment_total` back to the user's own total, then save.
- The Chrome tool blocks returned text that looks like cookies (perl `$x = …;`). For reading only, show `=`/`;` as look-alike characters, or return just an outline. Keep the real text when editing.
- A `v=` session link can be stale and redirect to Cengage logout. Plain `faculty.pl` works once the user signs in again.

