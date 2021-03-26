
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
to ensure it meets the needs of HCA data contributors, consumers and DCP downstream components.

The metadata schemas are versioned following semantic `versioning rules`_ that account for backwards compatibility.
However, compatibility of the schemas does not account for impact on the schema consumers, especially if different
versions of the same schemas must be maintained, indexed, displayed and/or interacted with in any form.

The metadata is updated through PRs in the `metadata-schema repository`_. The specific details can be found in the
documentation in the repository, but the overall flow can be described as follows:

- A ticket is opened describing an issue
- A member of the Metadata Team looks into the issue, there is a discussion and, if deemed important, a PR is created
  against the schema
- The PR is reviewed within a time frame specified by the type of change (Major/Minor/Patch). The review includes
  all the DCP components and test metadata is generated for each PR.
- Once the required metadata team reviewers approve the changes, the branch is merged into staging
- Once all the metadata changes required for a functional update are merged into staging, a PR will be created from the
  staging branch into master and test data will be created and linked to the PR. This PR should be reviewed by all
  DCP downstream components within 2 weeks.

In order to stay relevant to the HCA single cell scientific community, the HCA Metadata Schema must remain
agile, but the tools to interpret and present the [meta]data to the community must be kept in the loop to provide
users with a reliable system.


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
- Review the PRs in the established timeframe (2 weeks maximum)
- Test the metadata updates on their end


Metadata Schema Evolution
=========================


Updating the schema in GitHub
-----------------------------

|nn| For a list of schema changes based on types, please review the `evolution document`_ in the metadata schema
repository SOPs.

For a detailed procedure of how PRs are created, please review the `Release SOP`_, subsection ``Steps of the pre-release
process`` |ne|

When a metadata consumer creates a ticket in the metadata schema repository, the metadata team is informed and takes
a look at the requested change. After a discussion, if it's deemed important to the schema, a member of the metadata
schema will create a PR applying the changes to the desired schemas.

The affected schemas and its dependencies will all be versioned up, based on the type of change.


PR creation and review
----------------------
A member of the metadata team creates a PR with the changes specified in the ticket, targeting the staging branch.
This branch will be merged after wrangler approval.

These changes will be grouped together by functional groups, determined by the value they add as a whole vs the value
they add separately, making it so that more than one branch may be merged before trying to release the features.

Along with the changes, test metadata for that specific update is generated. This test data is generated in the `test
data repository`_ and linked in the PR under the section "**Test metadata:**" in the body.

One person from each of the 5 main DCP components will be tagged as a PR reviewer through random pullapprove pulls,
determined in the `pullapprove config file`_. The test [meta]data provided in the PR will be tested and each reviewer
will approve/request changes on the PR. The reviewers will also be responsible of pointing if they want changes or
suggest feedback on the schemas, which will then be discussed and, if deemed necessary, corrected by the metadata team.

The PRs from staging to master need to be reviewed within 2 weeks (10 working days). If the PR has not been reviewed by
the end of the first week, the metadata team will be responsible to ping the missing reviewers. After the 2 weeks, if
there are missing reviews, it will be understood that the update is accepted and the metadata team will proceed to
release the updates to production.


PR merging and release
----------------------

Once approved, the PR will be merged against the master branch and published in the `schema production server`_.

The detailed release process can be found in the `release SOP`_


Major changes and schema inconsistency between projects
-------------------------------------------------------

Inconsistencies in projects versions may cause issues when it comes to maintain them. Even if the latest version of a
schema is accepted, compatibility between several versions causes a high technical burden on the downstream end.

For that purpose, the metadata schema team will migrate, before each release, all the projects brokered into the DCP2
via the EBI ingestion system when there is, at least, 1 major change involved.

Minor and patch changes won't be migrated as they are assumed to be tested through the test metadata and minor changes
to the schema are backwards-compatible.


Test metadata
=============

The test metadata will be updated on every release PR (staging to master in the `metadata-schema repository`_) by the
metadata team. Details:

- Repository: `test data repository`_
- Folder: 'tests/'
- Data layout: Staging area exchange format
- Project UUID: 90bf705c-d891-5ce2-aa54-094488b445c6

Only one test will be maintained, with a stable project UUID, but for some releases there may be some extra projects
to test specific subgraphs.


.. _design principles: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/rationale.md#design-choices
.. _versioning rules: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/evolution.md
.. _metadata-schema repository: https://github.com/HumanCellAtlas/metadata-schema/
.. _evolution document: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/evolution.md
.. _Release SOP: https://github.com/HumanCellAtlas/metadata-schema/blob/master/docs/release_process.md
.. _schema production server: https://schema.data.humancellatlas.org/schemas/
.. _test data repository: https://github.com/HumanCellAtlas/schema-test-data
.. _pullapprove config file: https://github.com/HumanCellAtlas/metadata-schema/blob/master/.pullapprove.yml
