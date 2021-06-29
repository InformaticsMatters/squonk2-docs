# Sphinx Coding Standard
Cosing standards for our Sphinx-based, ReStructured Text (RST) documentation.

>   Basic syntax for RST can be found in the **reStructuredText [Primer]**
    section of the [RST] site

## Documentation root
Use `docs` as the root directory for documentation, this is where your
`index.html` and `conf.py` (and `Makefile` if you're using it) files will
be found, keeping the project root clear for non-documentation project
management files (README, CI/CI, gitignore files etc.).

## Filenames
Use only lowercase alphanumeric characters and `-` (hyphen) symbol,
with filenames ending `.rst`.

## Line length
TBD

## Hyperlinks
Resist the use of _inline_ references like this: -

    `Link text <https://domain.invalid/>`_

Instead, collect links at the bottom of the page
(ideally in alphabetical order)
and use a separate link and target definition like this: -

    This is a paragraph that contains `a link`_.

    .. _a link: https://domain.invalid/

This has two benefits, it makes the referring text easier to read and
also provides a link that can be used more than once from the page.

## Sections
Heading structure is determined from the succession of headings in RST files.
So, as there's some ambiguity we adopt the style declared in the
ReStructured Text [Sections] documentation.

---

[primer]: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
[rst]: https://www.sphinx-doc.org/en/master/usage/restructuredtext/
[sections]: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#sections
