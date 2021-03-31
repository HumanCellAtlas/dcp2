===================
HumanCellAtlas/dcp2
===================

Shared artifacts concerning the Human Cell Atlas (HCA) Data Coordination
Platform (DCP)

.. image:: https://github.com/DataBiosphere/hca-dcp/actions/workflows/main.yml/badge.svg
   :target: https://github.com/DataBiosphere/hca-dcp/actions/workflows/main.yml

.. contents::




Specifications & design documents
=================================

`DCP/2 System Design`_

.. _DCP/2 System Design: docs/dcp2_system_design.rst





Modifying the documents
=======================

The documentation in this repository is formatted using reStructuredText
(``.rst``).

If you're new to reStructuredText, check out the `quick reference`_ to get
started, or the `reStructuredText specifications`_ for more specific
questions.

.. _quick reference: https://docutils.sourceforge.io/docs/user/rst/quickref.html
.. _reStructuredText specifications: https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html


Style guide
-----------

Hard-wrap paragraphs at 80 columns. Not doing so creates long lines that are
hard to review.

Each document must have a `document title`_.

.. _document title: https://docutils.sourceforge.io/docs/user/rst/quickstart.html#document-title-subtitle

Separate paragraphs with one empty line. Separate top level `sections`_ with
four empty lines, and lower level sections with two. The visual aid helps
delineate sections when reading the source. Like Markdown, reStructured Text
is a compromise between a complete abstraction of presentational concerns and
the fact that the document source is read by humans, too, not just machines.

.. _sections: https://docutils.sourceforge.io/docs/user/rst/quickstart.html#sections

Don't manually enumerate `footnotes`_. Use the automatic numbering using
``[#]_``. This makes it much easier to insert footnotes and move sections
around.

.. _footnotes: https://docutils.sourceforge.io/docs/user/rst/quickref.html#footnotes

Do manually enumerate ordered lists.

Define hyperlink targets in close proximity to their first use i.e. directly
below the paragraph or at the end of the sub section. This makes it easier to
move sections around and to match hyperlink and target in the source.

Emulate existing conventions.


Verifying the document syntax
-----------------------------

A Github action automatically performs a reStructuredText syntax check to help
ensure the documentation is correctly formatted. To run this check on your
system, it is recommended to first set up a Python virtual environment::

   python -m venv .venv

Then activate the virtual environment by running ::

   source .venv/bin/activate

Install the linter::

   pip install rstcheck

You might want to install the exact `same version`_ as the one referenced in the
Github action. As long as the virtual environment is active, the syntax check
can be performed by running ::

   rstcheck -r .

.. _same version: .github/workflows/main.yml#L18