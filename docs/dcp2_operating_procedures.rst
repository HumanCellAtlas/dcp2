.. sectnum::
   :depth: 3
   :suffix: .

.. role:: raw-html(raw)
   :format: html
.. |nn| replace:: :raw-html:`<blockquote>`
.. |ne| replace:: :raw-html:`</blockquote>`




==========================
DCP/2 Operating Procedures
==========================

|nn| Text in a block quote (like this) is not normative. It was included
for informational purposes only and does not bear on the implementation. |ne|

.. contents::




Changing the HCA metadata schema
================================

The HCA metadata schema defines the shape of metadata documents being
exchanged between components of the DCP/2. It therefore constitutes a
de-facto contract between those components. The DCP consortium reviews schema
changes collectively to ensure that all teams in the consortium approve of a
schema change **before** it 

- affects the master branch of the `metadata-schema`_ repository,

- is published on `schema.humancellatlas.org`_, or

- is used by metadata staged for import into the production instance of TDR

.. _schema.humancellatlas.org: https://schema.humancellatlas.org/a

.. _metadata-schema: https://github.com/HumanCellAtlas/metadata-schema

.. _schema-test-data: https://github.com/HumanCellAtlas/schema-test-data

.. _dcp2: https://github.com/HumanCellAtlas/dcp2

.. _DCP/2 system design: dcp2_system_design.rst


Impact of Schema changes
-------------------------

Changes to the HCA metadata schema, including seemingly minor ones, can have
significant impact on the individual components of the DCP system. Even the
addition of a property can, in some case, require significant work in
metadata consumers like the Data Browser, especially if those consumers are
required to interpret the added property. It would be difficult to explicitly
define the criteria for classifing schema changes by impact. Instead, the
impact is determined individually for every schema change. Changes that are
deemed of low impact are fast-tracked through a lighter-weight review process.


Sizing schema changes
---------------------

A *schema change* is a carefully selected set of related modifications to one
or more of the JSONSchema documents in the `metadata-schema`_ repository. If
a schema change *can* be made smaller by excluding a particular change to a
schema document without sacrificing the desired end-user value, it *should*
be made smaller, so as to not overwhelm the reviewers with a mix of
orthogonal concerns. 

On the other hand, the set of changes shoudn't be too small either: all schema
document changes that are necessary to achieve the desired end-user value
should be combined in one schema change. If certain end user value is
achieved in a series of many small schema changes, reviewers will likely not
be able to see the big picture until they review the changes at the end of
that series, at which point it may be too late to reverse course.

Some schema changes warrant an update to the `DCP/2 System Design`_
specification. Whether they do or not is established collectively and early
on during the review process.


Responsiveness to submitter requirements
----------------------------------------


Schema changes are often motivated by the need to capture novel information
about the experimental design, phenotypes or sample collection process as
submitted by a participating laboratory, in a form that the current schema is
not able to sufficiently capture. Participating labs often don't have the
resources or tolerance for a prolonged submission process and are likely to
walk away if it takes too long to incorporate their submission into the
DCP/2.

The DCP/2 therefore needs to balance the celerity with which it adopts schema
changes on one hand against the effort needed to make all components
compatible with those changes on the other. The earlier a proposed change
faces scrutiny by the entire DCP/2 consortium, the better will the consortium
be able to negotiate the right balance, simply because the change will not
yet have permeated through the system, making it easier to make adujustments
to the proposed schema changes.


Schema change authorship
------------------------

Any member of the consortium can *sponsor* a schema change. The sponsor can
delegate authorship of the corresponding PRs to other members of the DCP/2
consortium. While the PRs that comprise a particular schema change don't all
need to have the same author, the sponsor must ensure that all those PRs are
semantically consistent with each other.



Review process overview
-----------------------

The process of reviewing schema changes utilizes Github's pull request (PR)
feature. Every schema change review begins with a PR against the `staging`
branch of the `metadata-schema`_ repository. The `develop` and `integration`
branches of that repository are not used anymore. Reviewers first determine

* the overall impact of the change

* whether the schema change is `sized <Sizing schema changes>`_ correctly

* whether the change requires updating the specification in the `dcp2`_
  repository

* whether it requires updating the test metadata in the `schema-test-data`_
  repository for other components to code unit tests against

If a change requires specification changes, the `metadata-schema`_ PR review is
suspended and a PR is filed against the `dcp2`_ repository and reviewed there.
Once that PR is approved, the `metadata-schema`_ PR is revised to match the
approved specification after which it is reviewed again.

Schema changes that are sized correctly and have negligible impact to other
components, are considered adopted by the DCP/2 as soon as the PR against the
`metadata-schema`_ repository is approved. No further reviews of PRs against
other branches or repositories are required.

Schema changes that are sized correctly, and that are of some impact to other
components, may require more involved testing. There are two ways of testing
schema changes: 

A)  (Meta)data exhibiting the proposed schema changes are first staged and then
    imported into the `dev` instance of TDR after which downstream components
    use the resulting `dev` snapshot to test the code changes that are required
    to support the schema change. The Ingest component can only populate
    staging areas from its staging instance, so the `metadata-schema`_ PR must
    first be approved provisionally [#provisionally]_, merged and deployed to
    the ``staging`` deployment of the Ingest component.

B)  (Meta)data exhibiting the proposed schema changes are committed to a feature
    branch of the `schema-test-data`_ repository, and a PR is filed for that
    branch. The `schema-test-data`_ repository can only populated using the
    staging instance of the Ingest component, so the `metadata-schema`_ PR must
    first be approved provisionally [#provisionally]_, merged and deployed to
    the ``staging`` deployment of the Ingest component.

.. [#provisionally]
   the provision being that the schema change may need to be amended in a
   follow-up PR

The reviewers of a PR can request either testing strategy.

In some cases a lab submission not only involves a schema change but also
introduces a novel way of linking the metadata enties into subgraphs. In those
cases, reviewers are likely to request testing strategy B. A request for either
strategy should be reasonably motivated. Possible reasons include

- the need to not only review schema changes but also review how those changes
  affect actual metadata documents

- the need to test future, unrelated code changes against the test metadata, so
  as to make sure that those future changes don't introduce regressions.
  Especially graph changes fall into that category.

Schema changes that are sized incorrectly need to be revised.


Pull request reviews
--------------------

Pull requests against the `dcp2`_, the `metadata-schema`_ or the
`schema-test-data`_ repositories are reviewed and approved in exactly the same
way, provided they are involved in a semantic change to either the DCP/2
specification, a DCP/2 standard operating procedure or a metadata schema
change:

As a PR author

1)  Announce the PR on the ``#dcp2`` channel on Slack, @-mentioning all
    `Designated reviewers`_. If the changes in the PR match the criteria for
    `expedited review`_, label the PR as ``expedited`` in GitHub and note that
    fact in the Slack announcement thread.

2)  In GitHub, request a review from all `Designated reviewers`_.

3)  Wait one week. For `expedited review`_, wait two business days. A business
    day is a weekday that is not a holiday for any of the participating
    institutions.

4)  If this is the first cycle of a normal, non-expedited review, remind about
    the PR on the ``#dcp2`` channel on Slack, @-mentioning requested reviewers
    that haven't yet reviewed the PR.

5)  If either

    a)  all reviewers approve of the PR without conditions or

    b)  two weeks (or two business days, for `expedited review`_) have passed
        since step 1 and there are no binding reviews requesting changes,

    merge the PR. You are done unless any of the approvals are conditional. A
    conditional approval is one that's dependent on successful testing using
    one of the strategies mentioned in `Review process overview`_. The
    condition has to be specified in the summary comment on approval. If
    testing fails, a reviewer may request amendmends to your changes. These
    requests are made as comments to the original, now merged PR. Open
    another PR with the requested amendmends and start at step 1.

6)  If a reviewer requests that you first update the DCP/2 system design
    specifcation or standard operating procedures first, suspend work on this
    PR and open another PR against the `dcp2`_ repository. Start at step 1
    there. Once that PR has been approved, update this PR branch with any
    follow-up changes resulting from the `dcp2`_ PR review process and resume
    this PR at step 1.

7)  If a reviewer requests that the PR be changed from `expedited review`_ to
    normal review, remove the label and note that fact on the Slack announcement
    thread. The current and subsequent review cycles revert back to the normal
    timeline.

8)  Otherwise, respond to every review comment either by making a source code
    change that you think appropriately addresses the comment or by replying
    with a comment explaining your opposition to the change or asking for more
    information. When addressing a set of related review comments with a source
    code change, try to commit that change separately from those addressing
    other related sets of review comments. Avoid constantly squashing the PR
    branch unless doing so helps reviewers to better understand the branch
    history.

9)  Start another cycle by requesting a review from the reviewers currently on
    the PR, even those that already approved the PR or just commented on it.
    Proceed to step 3.

Steps 1, 2 and 9 must be done on a weekday.

As a PR reviewer

1)  Respond to review requests within one week of the request (two business days
    for `expedited review`_) or

2)  Name a delegate within one business day of the request. Do so by cancelling
    the request for review by you and requesting a review from the delegate
    instead. Make a normal (non-review) comment on the PR, announcing the
    delegation.

3)  Review the PR in good faith. Ask specific questions or make specific
    suggestions. If you can't find anything objectionable in the PR, approve the
    PR as soon as possible. Don't get lost in details.

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

Expedited review
~~~~~~~~~~~~~~~~

Certain trivial changes are eligible for a compressed review timeline, as
defined in `Pull request reviews`_ above. The following changes are eligible for
expedited review:

- Additions of enum values to ``project.hca_bionetworks.hca_tissue_atlas``

Discourse
~~~~~~~~~

Review comments and replies by authors should be kept brief. Typically, review
comments made during a review cycle are addressed by the author and marked
resolved by the reviewer during the next cycle. Any disagreement that cannot be
resolved in two cycles should be discussed in a conference call to which the the
PR author invites the `Mediators`_ and all reviewers currently on the PR.


Designated reviewers
~~~~~~~~~~~~~~~~~~~~

- Enrique aka ``@ESapenaVentura`` (EBI)
- Amnon aka ``@amnonkhen`` (EBI)
- Nate aka ``@ncalvanese1`` (Broad)
- Hannes aka ``@hannes-ucsc`` (UCSC)
- Dave aka ``@NoopDog`` (UCSC)


Mediators
~~~~~~~~~

- Kathleen (Broad)
