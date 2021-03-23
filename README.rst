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