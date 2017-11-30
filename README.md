# PrintZongpai.css

**DISCLAIMER: Please be notified that this is still a draft and a very personal project**, meaning this might only suit the needs of mine, not others. However, I tried to make it as scalable as I can.

PrintZongpai.css and EPUBZongpai.css are an on-going project aimed to streamline book publishing workflow for vertical writing layouts. With the help of this CSS and its dependancies, authors should be able to write in a single source file (Markdown or HTML) and generate multiple outputs -- Web pages, a printable PDF, and an EPUB ebook.

Although rare in the world of the Web, vertical wrting is still commonly used by publishers in Japan, Taiwan, Hong Kong, Macau and enthusiasts worldwide.

## Getting Started

### Install dependancies

This project is depended on 2 services: [Pandoc](https://pandoc.org), [Vivliostyle](http://vivliostyle.com/en/) and [SASS](http://sass-lang.com).

#### Pandoc 

If you use a Mac, I recommend using [Homebrew](http://brew.sh/) to install Pandoc:

    brew install pandoc 

For more installation methods, please follow [Pandoc's instruction](https://pandoc.org/installing.html).

#### Vivliostyle

I use their [Google Chrome Extension](https://chrome.google.com/webstore/detail/vivliostyle/ffeiildjegeigkbobbakjjmfeacadbne).

## Use

### 1. Write

I write in [Pandoc Markdown](http://rmarkdown.rstudio.com/authoring_pandoc_markdown.html). You can also write directly in HTML. Here are some features and notices:

- Chapter

	Use `h2` for chapter titles. This will make sure all chapters start on the left page (PDF).

- Image caption

	Pandoc converts image `alt` text into visible caption. Here's an example in Pandoc Markdown:

		![圖說：你的圖說](./your/image/link.jpg)

- 縱中橫

	Give selected characters a `.zzh` class will rotate them 90 degree, suitable for short numbers. When writing in Pandoc Markdown, you can use the following marker:

		今天高溫達攝氏[33]{.zzh}度。

- Footnote

	As of today, only [reference-style footnotes](http://rmarkdown.rstudio.com/authoring_pandoc_markdown.html#footnotes) are supported. In HTML and PDF, all footnotes are placed at the end of the book. In EPUB, footnotes are placed at the end of each chapter.

### 2. Book Structure

Organize the book according to the following structure:

	MyBook
	|-- css
	|   |-- PrintZongpai.css
	|   |-- EPUBZongpai.css
	|-- content
	|   |-- 00meta.md
	|   |-- Chap00001.md
	|   |-- Chap00002.md
	|   |-- ....
	|-- image
	|   |-- image01.png
	|   |-- image02.png
	|   |-- ....
	|-- template.html

### 3. Metadata

I use a YAML metadata block and save it in a Markdown file -- `00meta.md`.

```YAML
---
title: "MyBook Title"
author: "My Name"
date: "2017-01-01"
lang: zh-Hant
cover-image: ./image/cover.jpg
stylesheet: "./css/EPUBZongpai.css"  # Stylesheet for EPUB
css: "./css/PrintZongpai.css"  # CSS for HTML and print
page-progression-direction: rtl
toc-title: "目次"
post: article  # HTML tag that wraps around main contents 
ibooks:
  specified-fonts: false  # Use iBook fonts
---
```

### 4. Template

My template is very similar to Pandoc's default template. It only adds two things:

1. an `<h2>` title for table of content if specified in metadata tag `toc-title`

2. all chapters are wrapped inside a HTML tag if specified in metadata `post`. For example, `post: article` in metadata will generate a HTML file with an `<article>` tag before Chapter 1 and a `</article>` at the end of the last chapter.

### 5. Page size and fonts

You can use [SASS](http://sass-lang.com) to change PDF page size and fonts. Simply modify `/scss/PrintZongpai.scss` and `scss/base/_page.scss`.

### 6. Generate

Use your command line client and `cd` into your book folder. 

#### HTML

	pandoc -s --file-scope --template=template.html --toc --toc-depth=2 content/*.md  -o <MyBook>.html

#### EPUB

	pandoc -s --toc --epub-chapter-level=2 --file-scope content/*.md -o <MyBook>.epub

#### Printable PDF

PDF is created with the help from Vivliostyle Chrome Extension. Here is how: 

1. Open the created HTML with Chrome.

2. Click on the Vivliostyle icon, and tick `Paginate` and `Spread View`.

3. Use Chrome's own Print function and set Margins as `None` and tick `Background and graphics`.

4. Save as PDF.

## Built With

* [CSS3](https://www.w3schools.com/css/css3_intro.asp)
* [CSS Paged Media](https://www.w3.org/TR/css3-page/)
* [SASS](http://sass-lang.com)
* [Pandoc](https://pandoc.org)
* [Vivliostyle Chrome Extension](https://chrome.google.com/webstore/detail/vivliostyle/ffeiildjegeigkbobbakjjmfeacadbne)

## Known Issues

- Table of contents doesn't show on iBooks .
- If footnotes span over multiple pages, the first footnote on the new page does not align with others.
- CSS `float: snap-block` + `float-reference: page` allow text to wrap around images, but somehow this messes up TOC and some images won't show.