
.. role:: raw-html(raw)
   :format: html
.. |nn| replace:: :raw-html:`<blockquote>`
.. |ne| replace:: :raw-html:`</blockquote>`

==============================
DCP2 Metadata Schema Evolution
==============================

.. contents::

Overview
========

The HCA metadata standard is developed in a transparent and open manner so that the whole HCA community can participate
in the process. It follows a set of `design principles`_ that serve as a foundation for the metadata standard in order
to ensure it meets the needs of HCA data contributors and consumers.

.. _design principles: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/rationale.md#design-choices

The metadata schemas are versioned each following semantic `versioning rules`_ that account for backwards compatibility.
However, compatibility of the schemas does not account for impact on the schema consumers, especially if different
versions of the same schemas must be maintained, indexed, displayed and/or interacted with in any form.

.. _versioning rules: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/evolution.md

The metadata is updated through PRs in the `metadata-schema repository`_. The specific details can be found in the
documentation in the repository, but the overall flow can be described as follows:

- A ticket is opened describing an issue
- A member of the Metadata Team looks into the issue, there is a discussion and, if deemed important, a PR is created
  against the schema
- The PR is reviewed within a time frame specified by the type of change (Major/Minor/Patch). The review includes
  all the DCP components and test metadata is generated for each PR.
- The branch is merged into develop
- Each week, the update gets one step upwards, until it hits production (``master``) 4 weeks later. At this point,
  the update is considered released.

In order to keep the path with the HCA single cell scientific community, the HCA Metadata Schema must remain
agile, but the tools to interpret and present the [meta]data to the community must be kept in the loop to provide
users with a reliable system.

.. _metadata-schema repository: https://github.com/HumanCellAtlas/metadata-schema/


High Level Goals
================

- Make it easier to combine data across HCA projects
- Make it simpler to collect metadata for HCA projects
- Reduce complexity of the schema towards users
- Increase clarity of the schema towards users
- Increase the trust the community has in our ability to build standards
- Users get an accurate representation of the “full” amount of HCA data
- Support Managed Access Data
- Support Contributor Generated Matrices: Representation, linking.
- Allow new single cell techniques in the DCP


Responsibilities
================

Metadata producers
------------------
- Generate and provide with test metadata that reflects the current updates in a format suitable for the schema
  consumers
- Generate one-off use-case test metadata for scenarios (e.g. new subgraph) that suit user stories implementation
- Check on metadata consumers when time frames are not being followed for PR reviewing
- Migrate projects on major changes before a release.

Metadata consumers
------------------
- Review the PRs in the timeframe stablished
- Test the metadata updates on their end


Metadata Schema Evolution
=========================


Updating the schema in GitHub
-----------------------------

|nn| For a list of schema changes based on types, please review the `evolution document`_ in the metadata schema
repository SOPs.

For a detailed procedure of how PRs are created, please review the `Release SOP`_, subsection ``Steps of the pre-release
process`` |ne|

.. _evolution document: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/evolution.md
.. _Release SOP: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/release_process.md

When a metadata consumer creates a ticket in the metadata schema repository, the metadata team is informed and takes
a look at the requested change. After a discussion, if it's deemed important to the schema, a member of the metadata
schema will create a PR applying the changes to the desired schemas.

The affected schemas and its dependencies will all be versioned up, based on the type of change.


PR creation and review
----------------------
A member of the metadata team creates a PR with the changes included, targeting the develop branch.

Along with the changes, test metadata for that specific update is generated, and it will cover all the possible use
cases/values when options are limited (e.g. enum fields). **Unless specifically asked to test a use case, ontologised
fields metadata won't cover every possible use case**.

One person from each of the 5 main DCP components will be tagged as a PR reviewer through random pullapprove pulls. The
test [meta]data provided in the PR will be tested and each reviewer will approve/request changes on the PR.

Depending on the type of change, the reviewers will be given a time frame for testing:

- Major change: 5 working days
- Minor change: 3 working days

Patches do not need to be reviewed by the metadata consumers, as they do not affect instances of the schema.



PR merging and release
----------------------

Once approved, the PR will be merged against the develop branch and published in the `schema dev server`_. Each week,
the previous environment is merged against the next one, making it effectively 4 weeks between when an update is
released to develop and when it's released to production.

The detailed release process can be found in the `release SOP`_

.. _schema dev server: https://schema.dev.data.humancellatlas.org/schemas/


Major changes and schema inconsistency between projects
-------------------------------------------------------

Inconsistencies in projects versions may cause issues when it comes to maintain them. Even if the latest version of a
schema is accepted, compatibility between several versions causes a high technical burden on the downstream end.

For that purpose, the metadata schema team will migrate, before each release, all the projects brokered into the DCP2
via the EBI ingestion system when there is, at least, 1 major change involved.

Minor and patch changes won't be migrated as they are assumed to be tested through the test metadata and be backwards
compatible.


Test metadata
=============

The test metadata will be updated on every PR by the metadata team. Details:

- Repository: `metadata-schema repository`_
- Folder: 'infrastructure_testing_files/DCP2_test_metadata/'
- Data layout: Staging area exchange format

Under the specified folder, there will be one folder per project metadata test. Only one test will be maintained,
with a stable project UUID, but for some releases there will be some extra projects to test specific user stories.

