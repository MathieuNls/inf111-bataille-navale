# inf111-bataille-navale
TP3 - INF111 @ ETS

## Contribute

- Fork
- Open a pull request

## Build PDF

`pandoc -s -S --latex-engine=xelatex --filter pandoc-citeproc --number-sections --listings --csl="config/ieee.csl" --template="config/default.latex" -o sujet.md.pdf sujet.md`

## Dependencies

- Pandoc
- Pandoc-citeproc
- Latex