
.. role:: raw-html(raw)
   :format: html
.. |nn| replace:: :raw-html:`<blockquote>`
.. |ne| replace:: :raw-html:`</blockquote>`




==========================
DCP/2 Operating Procedures
==========================

|nn| Text in a block quote (like this) is not normative. It was included
for informational purposes only and does not bear on the implementation.
|ne|

.. contents::





Changing the HCA metadata schema
================================

Changes to the HCA metadata schema, even seemingly minor ones, can have
significant impact on the individual components of the DCP system. The DCP
consortium therefore uses a review process that ensures all teams in the
consortium approve of a schema change **before** it 

- affects the master branch of the `metadata-schema`_ repository,

- is published on `schema.humancellatlas.org`_, and

- is used by metadata staged for import into the production instance of TDR

.. _schema.humancellatlas.org: https://schema.humancellatlas.org/a

.. _metadata-schema: https://github.com/HumanCellAtlas/metadata-schema

.. _schema-test-data: https://github.com/HumanCellAtlas/schema-test-data

.. _dcp2: https://github.com/HumanCellAtlas/dcp2

.. _DCP/2 system design: dcp2_system_design.rst


Sizing schema changes
---------------------

A *schema change* is a set of related modifications to one or more of the 
JSONSchema documents in the `metadata-schema`_ repository. That set must be
chosen carefully. If a schema change *can* be made smaller by excluding a
particular change to a schema document without sacrificing the desired end-user
value, it *should* be made smaller, so as to not overwhelm the reviewers with a
mix of orthogonal concerns. On the other hand, the set of changes shoudn't be
too small either: all schema document changes that are necessary to achieve the
desired end-user value should be combined in one overall schema change. If
certain end user value is achieved in a series of many small schema changes,
reviewers will likely not be able to see the big picture until they review the
changes at the end of that series at which point it may be too late to reverse
course.

Some schema changes warrant an update to the `DCP/2 System Design`_
specification. Whether they do or not is established during the review process
of *pull requests* (PRs) against the `metadata-schema` and `schema-test-data`
repositories. Specification updates are done as a PR against the `dcp2`_
repository.


Schema change authorship
------------------------

Anyone can author a PR. The PRs for one schema change don't all need to be by
the same author, as long as there are no competing and open PRs in the same
repository and for the same schema change. One person *should* author the PRs
against the `metadata-schema`_ and `dcp2`_ repositories. That person is referred
to as the *schema change author*. The schema change author can delegate
authorship of PRs to other members of the DCP consortium.


Schema change procedure
-----------------------

The overall procedure for authoring and reviewing a schema change consists of
the following steps:

1)  If it is considered unnecessary to update the specification and a
    specification update wasn't explicitly requested, proceed to step 4.

2)  Prepare a PR against the `dcp2`_ repository, updating the `DCP/2 System
    Design`_.

3)  Initiate a `Pull request review`_ of your PR. Once the PR has been approved
    by the DCP, move on to the next step. Don't merge the PR just yet.

4)  Prepare a PR against the ``develop`` branch of the `metadata-schema`_
    repository. It is OK to perform this step concurrently with the preceding
    steps.

5)  Initiate a `Pull request review`_ of your PR. Once the PR has been approved
    by the DCP, move on to the next step. Don't merge the PR just yet.

    Note that the above steps can be performed for several independent schema
    changes in parallel.
   
6)  Merge the `metadata-schema`_ repository PR into the ``develop`` branch and
    from there into the ``staging`` branch.

7)  Prepare a PR against the `Test metadata`_ in the `schema-test-data`_
    repository. Don't merge the PR just yet.

8)  Initiate a `Pull request review`_ of your PR. Once the PR has been approved
    by the DCP, move on to the next step. As part of the review, the Azul team
    updates their unit tests to validate the updated test metadata. If a test
    fails, they either improve the test or the team member reviewing the PR
    requests the necessary changes to the test metadata.

9)  Prepare a PR against the schema repository, targeting the master branch.

10) Initiate a `Pull request review`_ of your PR. The review should be quick
    because reviewers will already have seen the changes while reviewing the PR
    against the ``develop`` branch. Reviewers should not request any substantial
    changes at this stage unless last minute considerations absolutely require
    it. Once the PR has been approved by the DCP, move on to the next step.

11) Merge all PRs. You may squash and/or rebase a PR before merging it but that
    should not affect the diff between the base and head commits of the PR
    branch, except for housekeeping like resolving merge conflicts or adjusting
    the host name of schema URLs in PRs against the `schema-test-data`_
    repository. No semantic changes may be introduced to a PR after it has been
    approved by the DCP. All such housekeeping should be done on the PR branch
    prior to merging it.


Test metadata
-------------

Every change to the schemas in the `metadata-schema`_ repository must be
accompanied by a matching change to the test metadata in the `schema-test-data`_
repository. Both changes must be done as PRs following the `Pull request
review`_  process. 

The test metadata should exercise all graph shapes in actual use.

The schema references (``describedBy``) in all metadata documents in the main
branch of `schema-test-data`_ repository must match the schemas on the
`master` branch of the `metadata-schema`_ repository.


Pull request review
-------------------

As a PR author

1)  Announce the PR on the `#dcp2` channel on Slack, @-mentioning all
    `Designated reviewers`_.

2)  Request a review from all `Designated reviewers`_.

3)  Wait one week.

4)  If this is the first review cycle, remind about the PR on the ``#dcp2``
    channel on Slack, @-mentioning requested reviewers that haven't yet
    reviewed the PR.

5)  If two weeks have passed since step 1 and there are no binding reviews
    requesting changes, the PR has been approved by the DCP.

6)  Otherwise, if this is a PR against the `metadata-schema`_ or
    `schema-test-data`_ repositories and a reviewer requests that you first
    update the DCP/2 system design specifcation first, close this PR and
    proceed with step 2 in section `Changing the HCA metadata schema`_.

7)  Otherwise, respond to every review comment either by making a source code
    change that you think appropriately addresses the comment or by replying
    with a comment explaining your opposition to the change or asking for more
    information. When addressing a set of related review comments with a source
    code change, try to commit that change separately from those addressing
    other related sets of review comments. Avoid constantly squashing the PR
    branch unless doing so helps reviewers to better understand the branch
    history.

8)  Start another cycle by requesting a review from the reviewers currently on
    the PR, even those that already approved the PR or just commented on it.
    Proceed to step 3.

Steps 1, 2 and 8 must be done on a weekday.

As a PR reviewer

1)  Respond to review requests within one week of the request or 

2)  Name a delegate within one business day of the request. Do so by cancelling
    the request for review by you and requesting a review from the delegate
    instead. Make a normal (non-review) comment on the PR, announcing the
    delegation.

3)  Review the PR in good faith. Ask specific questions or make specific
    suggestions.

4)  If you accept the PR author's response to a comment made by the reviewer,
    mark the comment thread as resolved on Github.

.. _dismissed: https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/dismissing-a-pull-request-review

Once a designated reviewer delegated the review, none of the designated
reviewer's comments or reviews are binding. It's acceptable to direct the
delegate but that should be kept to a minimum and ideally be done outside of
Github e.g., Slack or E-Mail.

A delegate reviewer can only delegate back to the designated reviewer that named
them, and only after the PR author rerequested a review from the delegate.

Only comments and reviews by mediators, designated reviewers or their delegate
are binding. Other reviews should be `dismissed`_ by the author. Other comments
can be ignored.
  
Reviews by designated reviewers or their delegate can be `dismissed`_ by the
`Mediators`_.

At no time during the life-time of the PR can there be more reviewers listed on
the PR than there are entries in the lists of `Designated reviewers`_ and
`Mediators`_.


Discourse
~~~~~~~~~

Review comments and replies by authors should be kept brief. Typically, review
comments made during a review cycle are addressed by the author and marked
resolved by the reviewer during the next cycle. Any disagreement that cannot be
resolved in two cycles should be discussed in a conference call to which the the
PR author invites the `Mediators`_ and all reviewers currently on the PR.


Designated reviewers
~~~~~~~~~~~~~~~~~~~~

- Claire (EBI)
- Marion (EBI)
- Ruchi (Broad)
- Kylee (Broad)
- Andrew (Broad)
- Hannes (UCSC)
- Dave (UCSC)


Mediators
~~~~~~~~~

- Kathleen (Broad)
