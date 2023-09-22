Open Journals PDF Generator
===========================

Create a draft PDF of an Open Journals article.

Usage
-----

Add a file `.github/workflows/draft-pdf.yml` to your repo.

``` yaml
name: Draft PDF
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

This will build the paper.pdf and make it available as an artifact
after each build.

If you would like to store the PDF in your repository after the build, you need to (1) enable repository write access for your Actions under _Settings/Actions/General/WorkflowPermissions_ and (2) add a step which will `git add` and `git commit` it within the workflow. You can do the latter manually, or wiht a predefined action, e.g. `EndBug/add-and-commit`, by appending `steps` in `draft-pdf.yml` with:

``` yaml
    - name: Commit PDF to repository
      uses: EndBug/add-and-commit@v9
      with:
        message: '(auto) Paper PDF Draft'
        # This should be the path to the paper within your repo.
        add: 'paper.pdf' # 'paper/*.pdf' to commit all PDFs in the paper directory
```              

Inputs
------

The build can be configured by setting the following inputs.

### `journal`

The journal for to which this paper will be submitted. May be
either `joss` or `jose`. Defaults to `joss`.

### `paper-path`

Filename of the paper, relative to the project's root directory.
Defaults to `paper.md`.



