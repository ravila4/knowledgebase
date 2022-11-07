
## Nvim-R plugin

Vim plugin for R integration.

- Start/Close R
- Send lines, selection, paragraphs, functions, blocks, entire file to R, knitr, pdflatex
- Object browser
- Help browser
- Completion
- Object introspection

## Key bindings

Note: The `<LocalLeader>` is '\\' by default.

Note: It is recommended the use of different keys for `<Leader>` and
`<LocalLeader>` to avoid clashes between filetype plugins and general plugins
key binds. See `|filetype-plugins|`, `|maplocalleader|` and
[Nvim-R-localleader](faq-and-tips#remap-the-localleader).

To use the plugin, open a `.R`, `.Rnw`, `.Rd`, `.Rmd` or `.Rrst` file with Vim and
type `<LocalLeader>rf`. Then, you will be able to use the plugin key bindings to
send commands to R.

This plugin has many key bindings, which correspond with menu entries. In the
list below, the backslash represents the `<LocalLeader>`. Not all menu items and
key bindings are enabled in all filetypes supported by the plugin (`r`,
`rnoweb`, `rhelp`, `rrst`, `rmd`).

### Start/Close

| Action             | Default shortcut |
| ------------------ | ---------------- |
| Start R (default)  | \\rf             |
| Start R (custom)   | \\rc             |
| Close R (no save)  | \\rq             |
| Stop (interrupt) R | :RStop           |

### Send

| Action                                            | Default shortcut |
| ------------------------------------------------- | ---------------- |
| File                                              | \\aa             |
| File (echo)                                       | \\ae             |
| File (open `.Rout`)                               | \\ao             |
| Block (cur)                                       | \\bb             |
| Block (cur, echo)                                 | \\be             |
| Block (cur, down)                                 | \\bd             |
| Block (cur, echo and down)                        | \\ba             |
| Chunk (cur)                                       | \\cc             |
| Chunk (cur, echo)                                 | \\ce             |
| Chunk (cur, down)                                 | \\cd             |
| Chunk (cur, echo and down)                        | \\ca             |
| Chunk (from first to here)                        | \\ch             |
| Function (cur)                                    | \\ff             |
| Function (cur, echo)                              | \\fe             |
| Function (cur and down)                           | \\fd             |
| Function (cur, echo and down)                     | \\fa             |
| Selection                                         | \\ss             |
| Selection (echo)                                  | \\se             |
| Selection (and down)                              | \\sd             |
| Selection (echo and down)                         | \\sa             |
| Selection (evaluate and insert output in new tab) | \\so             |
| Send motion region                                | \\m{motion}      |
| Paragraph                                         | \\pp             |
| Paragraph (echo)                                  | \\pe             |
| Paragraph (and down)                              | \\pd             |
| Paragraph (echo and down)                         | \\pa             |
| Line                                              | \\l              |
| Line (and down)                                   | \\d              |
| Line (and new one)                                | \\q              |
| Left part of line (cur)                           | \\r\<Left\>      |
| Right part of line (cur)                          | \\r\<Right\>     |
| Line (evaluate and insert the output a comment)   | \\o              |
| All lines above the current one                   | \\su             |

### Command

| Action                                          | Default shortcut |
| ----------------------------------------------- | ---------------- |
| List space                                      | \\rl             |
| Clear console                                   | \\rr             |
| Remove objects and clear console                | \\rm             |
| Print (cur)                                     | \\rp             |
| Names (cur)                                     | \\rn             |
| Structure (cur)                                 | \\rt             |
| View data.frame (cur) in new tab                | \\rv             |
| View data.frame (cur) in horizontal split       | \\vs             |
| View data.frame (cur) in vertical split         | \\vv             |
| View head(data.frame) (cur) in horizontal split | \\vh             |
| Run dput(cur) and show output in new tab        | \\td             |
| Arguments (cur)                                 | \\ra             |
| Example (cur)                                   | \\re             |
| Help (cur)                                      | \\rh             |
| Summary (cur)                                   | \\rs             |
| Plot (cur)                                      | \\rg             |
| Plot and summary (cur)                          | \\rb             |
| Set working directory (cur file path)           | \\rd             |
| Sweave (cur file)                               | \\sw             |
| Sweave and PDF (cur file)                       | \\sp             |
| Sweave, BibTeX and PDF (cur file) (Linux/Unix)  | \\sb             |
| Knit (cur file)                                 | \\kn             |
| Knit, BibTeX and PDF (cur file) (Linux/Unix)    | \\kb             |
| Knit and PDF (cur file)                         | \\kp             |
| Knit and Beamer PDF (cur file)                  | \\kl             |
| Knit and HTML (cur file, verbose)               | \\kh             |
| Knit and ODT (cur file)                         | \\ko             |
| Knit and Word Document (cur file)               | \\kw             |
| Markdown render (cur file)                      | \\kr             |
| Spin (cur file) (only .R)                       | \\ks             |
| Markdown render (cur file) (all in YAML)        | \\ka             |
| Quarto render (cur file)                        | \\qr             |
| Quarto preview (cur file)                       | \\qp             |
| Quarto stop preview (all files)                 | \\qs             |
| Open attachment of reference (Rmd, Rnoweb)      | \\oa             |
| Open PDF (cur file)                             | \\op             |
| Search forward (SyncTeX)                        | \\gp             |
| Go to LaTeX (SyncTeX)                           | \\gt             |
| Debug (function)                                | \\bg             |
| Undebug (function)                              | \\ud             |
| Build tags file (cur dir)                       | :RBuildTags      |

### Edit

| Action                              | Default shortcut |
| ----------------------------------- | ---------------- |
| Insert `<-`                         | \_               |
| Complete object name                | CTRL-X CTRL-O    |
| Indent (line)                       | ==               |
| Indent (selected lines)             | =                |
| Indent (whole buffer)               | gg=G             |
| Toggle comment (line, sel)          | \\xx             |
| Comment (line, sel)                 | \\xc             |
| Uncomment (line, sel)               | \\xu             |
| Add/Align right comment (line, sel) | \\;              |
| Go (next R chunk)                   | \\gn             |
| Go (previous R chunk)               | \\gN             |

### Object Browser

| Action               | Default shortcut |
| -------------------- | ---------------- |
| Open/Close           | \\ro             |
| Expand (all lists)   | \\r=             |
| Collapse (all lists) | \\r-             |
| Toggle (cur)         | Enter            |

### Help

| Action        | Default shortcut |
| ------------- | ---------------- |
| Help (plugin) |
| Help (R)      | :Rhelp           |

## Custom keybindings

To customize a key binding you could put something like this in your `vimrc`:

```vim
function! s:customNvimRMappings()
   nmap <buffer> <Leader>sr <Plug>RStart
   imap <buffer> <Leader>sr <Plug>RStart
   vmap <buffer> <Leader>sr <Plug>RStart
endfunction
augroup myNvimR
   au!
   autocmd filetype r call s:customNvimRMappings()
augroup end
```

## Commands

### Star/Close R

- RStart
- RCustomStart
- RClose
- RSaveClose

### Clear R console

- RClearAll
- RClearConsole

### Edit R code

- RSimpleComment
- RSimpleUnComment
- RToggleComment
- RRightComment
- RIndent
- RNextRChunk
- RPreviousRChunk

### Send code to R console

- RSendLine
- RSendAboveLines
- RSendSelection
- RSendMotion
- RSendSelAndInsertOutput
- RSendMBlock
- RSendParagraph
- RSendFunction
- RSendFile

### Send command to R

- RHelp
- RPlot
- RSPlot
- RShowArgs
- RShowEx
- RShowRout
- RObjectNames
- RObjectPr
- RObjectStr
- RViewDF (view data.frame or other object)
- RViewDFs (view object in split window)
- RViewDFv (view object in vertically split window)
- RViewDFa (view object in split window above the current one)
- RDputObj
- RSetwd
- RSummary
- RListSpace

### Support to Sweave and knitr

- RBibTeXK (Knitr)
- RKnit
- RMakeHTML
- RMakePDFK (Knitr)
- RMakePDFKb (.Rmd, beamer)
- RMakeODT (.Rmd, Open document)
- RMakeWord (.Rmd, Word document)
- RMakeRmd (rmarkdown default)
- RMakeAll (rmarkdown all in YAML)
- ROpenPDF
- RNextRChunk
- RPreviousRChunk

### Support to Quarto

- RQuartoRender
- RQuartoPreview
- RQuartoStop
