=======
hca-dcp
=======

.. image:: https://github.com/DataBiosphere/hca-dcp/actions/workflows/main.yml/badge.svg
   :target: https://github.com/DataBiosphere/hca-dcp/actions/workflows/main.yml

Shared artifacts concerning the Human Cell Atlas (HCA) Data Coordination
Platform (DCP)

.. contents::

Design documents
================

`DCP/2 System Design`_

.. _DCP/2 System Design: docs/dcp2_system_design.rst

Edit the documents
==================

The documentation in this repository is formatted using reStructuredText
(``.rst``).

If you're new to reStructuredText, check out the `quick reference`_ to get
started, or the `reStructuredText specifications`_ for more specific
questions.

.. _quick reference: https://docutils.sourceforge.io/docs/user/rst/quickref.html
.. _reStructuredText specifications: https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html

Running the linter
------------------

We run a linter to ensure the RestructuredText files are correctly formatted. To
run this on you system, first set up a python virtual environment::

   python -m venv venv

Then activate the virtual environment by running::

   source venv/bin/activate

Install the linter::

   pip install rstcheck

As long as your virtual environment is active, you can run the linter with::

   rstcheck -r .

