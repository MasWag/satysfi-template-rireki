satysfi-template-rireki
=======================

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

A minimal SATySFi template repository for writing a Japanese 履歴書. The design and contents mostly follows the template provided by Ministry of Health, Labour and Welfare of Japan (厚生労働省): https://www.mhlw.go.jp/stf/newpage_kouseisaiyou030416.html

This repository is intended to be used as a **GitHub template repository**.
Create a new repository from this template, edit `rireki.saty`, and build the PDF with SATySFi.

![](./images/rireki-1.svg)
![](./images/rireki-2.svg)

Requirements
------------

You need a working SATySFi environment.

The template depends on the following SATySFi packages:

- `base`
- `class-jlreq`
- `easytable`
- `figbox`
- `latexcmds`
- `parallel`
- `pagestyle`

Install them with:

```sh
opam install satysfi-base satysfi-class-jlreq satysfi-easytable satysfi-figbox satysfi-latexcmds satysfi-parallel satysfi-pagestyle
satyrographos install
```

Getting started
---------------

### 1. Create a new repository from this template

On GitHub, click **Use this template** and create a new repository.

### 2. Edit `rireki.saty`

Open `rireki.saty` and replace the sample information with your own data.
The public API is now composable: prepare small section records and place the sections you want inside `rireki-document '< ... >'`.

Available section blocks:

- `+rireki-header(header-data)`
- `+rireki-basic-info(basic-info-data)`
- `+rireki-contact-info(contact-info-data)`
- `+rireki-history(history-data)`
- `+rireki-dated-table(table-data)`
- `+rireki-license(license-data)`
- `+rireki-awards(awards-data)`
- `+rireki-degrees(degrees-data)`
- `+rireki-research-history(research-history-data)`
- `+rireki-freeform-box(freeform-data)`
- `+rireki-note`

The portrait is passed as a `FigBox.figbox`.
History rows are passed as `education-rows` / `work-rows`.
The basic and contact info blocks are separate, so you can omit `+rireki-contact-info(...)` entirely when you do not want a `連絡先` section.
License-like tables use `rows` with the same `(year, month, detail)` row shape as before.
Generic freeform boxes use the `freeform-box` record with `title`, `paragraphs`, and `style`, where `style` is either `SingleLargeCell` or `OneParagraphPerRow`.
Long history and dated-table content should be split by the caller across multiple `+rireki-history` / `+rireki-dated-table` calls and pages.
Use `rireki-empty-row` inside `education-rows`, `work-rows`, or `rows` when you want an intentionally blank dated-table row with the same height as ordinary rows.
Use `rireki-empty-freeform-row` inside `paragraphs` when you want an intentionally blank row in a `style = OneParagraphPerRow` freeform box.

The sample data is fictional and is included only so that the template works out of the box.

Minimal example:

```satysfi
rireki-document '<
  +rireki-header(header-data);
  +rireki-basic-info(basic-info-data);
  +rireki-contact-info(contact-info-data);
  +vskip(8pt);
  +rireki-history(history-data);
  +vskip(4pt);
  +rireki-note;
  +clearpage;
  +rireki-license(license-data);
  +vskip(8pt);
  +rireki-freeform-box(pr-box-data);
  +vskip(8pt);
  +rireki-freeform-box(request-box-data);
>
```

### 3. Build the PDF

Run:

```sh
satysfi rireki.saty -o rireki.pdf
```

This should generate `rireki.pdf` from the SATySFi source.

Files
-----

* `rireki.saty`: Main SATySFi source file to edit
* `rireki.satyh`: Helper definitions for the 履歴書 layout
* `portrait.pdf`: An example portrait image, originally from [Blue circled 9 on Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Blue_circled_9.svg)
* `README.md`: This file

Customization
-------------

The intended workflow is simple:

1. prepare section records in `rireki.saty`
   put top/name/photo data in `header-data`, basic address data in `basic-info-data`, optional contact data in `contact-info-data`, and so on
2. compose pages manually inside `rireki-document '< ... >'`
   use `+clearpage` to break pages and repeat `+rireki-history` / `+rireki-dated-table` when you want to split long tables
3. build the PDF

If you want to adjust the layout itself, edit `rireki.satyh`.

License
-------

This repository is licensed under the MIT License.
