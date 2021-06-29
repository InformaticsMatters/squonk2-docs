# Squonk 2 Documentation

[![build main](https://github.com/InformaticsMatters/squonk2-docs/actions/workflows/build-main.yaml/badge.svg)](https://github.com/InformaticsMatters/squonk2-docs/actions/workflows/build-main.yaml)

The [Sphinx]-based Squonk 2 documentation.

Building the documentation, from a suitably equipped virtual environment: -

    $ source ~/.venv/squonk2-docs/bin/activate
    $ pip install -r build-requirements.txt

    $ cd docs
    $ make html

You'll find the HTML `index.html` file in `docs/_build/html`.

---

[sphinx]: https://www.sphinx-doc.org/en/master/
