---
layout: post
title:  "Is there a good Linux software to annotate PDFs?"
date: 2015-05-29 
categories: 
    - Linux
    - PDF
    - Software
    - In English
author: Joris Muller
---

As a Mac OS user, I'm very happy to use the _Preview_ software to read and annotate my PDFs. I read a lot of PDF because I'm in an academic field. _Preview_ does exactly what I need: Highlight following the lines and keeping the text clear (not just an alpha layer), comments, add figures. Furthermost, when I'm editing a PDFs with _Preview_, I don't have to save it to an other format. But _Preview_ have a major drawback: it's not open source and don't exist on Linux.

I'm trying to switch to Linux then I need to find something close to _Preview_.

After a small research, I found a list on the site of [pdfreaders.org](http://pdfreaders.org/pdfreaders.en.html). The candidates are the following.

# Specification

The software I need have to meet the following specifications:

- Open standard PDFs, as one could find in scientific papers
- Annotate PDF with the following tools:
    - Highlight the text by selecting the text (not free forms) without affecting the readability (not only an alpha layer)
    - Underline
    - Write comments
    - Draw rectangles, and maybe free forms
- Save these annotations directly in the PDF. They must be readable with others PDFs readers, including Acrobat Reader and _Preview_. I need to share my annotations to colleagues.
- An Open Source software should be better but not essential. My priority is to be productive on my new Linux platform.

I don't need:

- Open strange PDF format, as forms or interactive one. For these very rare cases, I could use time to time a virtualized version of Adobe Acrobat Reader
- "cloud" features
- Conversion features
- Another coffee machine

# The challengers

## Acrobat Reader

Acrobat Reader is the default choice to read PDF. On my PC at work, it does the job pretty well. But I can't achieve my goal with it because beyond the fact it's a proprietary software, there is no recent Linux version. The last release, Adobe Acrobat Reader DC, exists only for Mac OS and Windows.

<img src="/assets/pdf_reader_post/no_acrobat_reader_linux.png" title="No DC for Linux"  style="display: block; margin: auto;" />

I searched a lot in their website, and I have the feeling they discontinue supporting all Linux versions. 

## Okular

[Okular](https://okular.kde.org/) seems to be a good candidate.

Highlight work perfectly, I could save my annotation in the PDF.

The only thing a little bit annoying is that one have to do save as to save the annotation. Can't use compulsively <kbd>ctrl</kbd>-<kbd>s</kbd> (by boss hate when I'm doing that).

<img src="https://okular.kde.org/images/screenies/okular-annotations.png" title="Okular screenshot"  style="display: block; margin: auto;" />

## Evince

[Evince](https://wiki.gnome.org/Apps/Evince) is just a PDF viewer; then it doesn't do the job.

## xournal

[Xournal](http://xournal.sourceforge.net/) is a nice software to take notes with features to open PDFs, but it use his own format to save annotation. Furthermost, the highlight tool make the text difficult to read.

# Conclusion

Okular is the way to go!
