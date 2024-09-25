# iclc-templates

A collection of templates for paper submissions, performance proposals, technical questionnaires and so on
that have been used for ICLC over the years.

```sh
cd 2025/papers/markdown
pandoc --template=pandoc/iclc.latex --citeproc --number-sections iclc2025.md -o iclc2025.pdf
```
