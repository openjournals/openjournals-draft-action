Open Journals PDF Generator
===========================

Create a draft PDF of an Open Journals article.

Usage
-----

Add a file `.github/workflows/draft-pdf.yml` to your repo.

``` yaml
on: [push]

jobs:
  paper:
    runs-on: ubuntu-latest
    name: Paper Draft
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Build draft PDF
        uses: openjournals/openjournals-draft-action@master
        with:
          journal: joss
          # This should be the path to the paper within your repo.
          paper-path: paper.md
      - name: Upload
        uses: actions/upload-artifact@v1
        with:
          name: paper
          # This is the output path where Pandoc will write the compiled
          # PDF. Note, this should be the same directory as the input
          # paper.md
          path: paper.pdf
```

This will build the `paper.pdf` and make it available for download as an _Actions artifact_
after each build.

If you store your `paper` files in the same repository as your source code or any other files that are not included in the final rendering, you may want to limit the scope of when this action is triggered, to avoid duplicating the same PDF when unrelated push is made. You can do so, by replacing `on: [push]` in your `draft-pdf.yml` with:

``` yaml
on:
  push:
    paths:
      - paper/**
      - .github/workflows/draft-pdf.yml
```
to capture all changes to your paper files in `paper` directory, or with:
``` yaml
on:
  push:
    paths:
      - paper.md
      - paper.bib
      - assets/img1.png
      - assets/img2.png
      - .github/workflows/draft-pdf.yml
```
if you need to specify individual files.


Inputs
------

The build can be configured by setting the following inputs.

### `journal`

The journal for to which this paper will be submitted. May be
either `joss` or `jose`. Defaults to `joss`.

### `paper-path`

Filename of the paper, relative to the project's root directory.
Defaults to `paper.md`.



