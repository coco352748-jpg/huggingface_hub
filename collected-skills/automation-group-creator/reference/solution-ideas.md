# Solution ideas

Use this catalog when the user asks what they can build or has a broad goal.
Offer two or three ideas relevant to their work, not the whole catalog.
Describe the pain reduced and a first useful deliverable before discussing
Automations. These are candidates to investigate, not promises that every
repository has the required access, evidence or runtime capabilities.

## Recognize a good opportunity

Look for recurring work with accessible inputs, a bounded useful action and
feedback that can show whether it helped. Strong candidates involve repeated
retrieval, varied evidence, forgotten context, expensive coordination or
well-defined changes that people can review.

Ask which work is frequent, costly or error-prone, and whether the user can
show a recent example. Prefer a problem with an available owner and a concrete
first deliverable. Use [designing for outcomes](designing-for-outcomes.md) before
recommending architecture or making writes.

## Problems worth suggesting

| User outcome | First useful deliverable | Outcome evidence to measure | Important boundary |
| --- | --- | --- | --- |
| Get incoming issues to the right owner sooner | Proposed labels, duplicate candidates and routing rationale for review | Arrival-to-confirmed-route duration, confirmed misroutes among examined decisions and correction effort | Use the approved taxonomy; similarity is not proof that two issues are duplicates |
| Understand recurring user friction | A periodic, source-linked grouping of feedback with representative cases | Findings used in recorded planning decisions, omissions in audited source samples and results of linked corrective work | Preserve minority and contradictory reports; use only authorized feedback sources |
| Reduce time spent diagnosing CI failures | A failure summary grouping likely repeated causes and evidence for the next investigation | Time to a confirmed diagnosis, incorrect diagnoses and recurrence after the linked fix | Distinguish a flaky test from a real regression; do not disable tests merely to improve pass rate |
| Remove routine maintenance from engineers' queues | A small set of scoped maintenance tasks or draft PRs with checks | Completed intended maintenance, review/correction effort and linked regressions or reversions after adoption | Limit task classes and change size; existing checks and review still apply |
| Keep documentation aligned with behavior | Source-linked drift findings and focused draft documentation updates | Time to a confirmed drift correction, rejected findings and recurring misunderstandings linked to updated guides | Distinguish intentional product changes from missing documentation; do not publish private context |
| Give reviewers higher-signal context | A concise PR brief or review suggestions tied to the diff and project conventions | Explicit acceptance/rejection, corrective changes, review effort and later linked defects missed by the review | Avoid repetitive comments; passing checks and model agreement are not proof of correctness |
| Find and address meaningful test gaps | Candidate behavior gaps with reproducible cases, then bounded test PRs | Demonstrated regression detection, later linked defects missed by tests, comparable CI duration and flake frequency | Coverage or acceptance alone is not effectiveness; preserve test intent and avoid invented requirements |
| Prepare releases with fewer last-minute surprises | A source-linked readiness brief listing changes, missing evidence and unresolved blockers | Time to resolve known blockers, missed required checks and linked post-release incidents | Do not infer readiness from green CI alone or publish a release without approval |
| Keep dependencies current within policy | Prioritized update candidates and small, checked draft PRs | Time overdue under agreed update windows, review effort and linked compatibility failures after adoption | Follow version and risk policy; do not promise external advisories or auto-merge capabilities |
| Move approved migration work through a backlog | Inventoried cases, scoped tasks and reviewed changes with progress records | Consumers verified compatible, time to completion, manual effort and linked migration regressions | Verify real consumers and support windows; cross-repository work needs explicit access and scope |
| Make project status and dependencies easier to act on | A concise digest of evidence-backed blockers, missing owners and next decisions | Time to an acknowledged blocker decision or resolution, missed blockers and time spent gathering status | Missing activity is not evidence of poor performance; prioritization and commitments remain with people |
| Build reusable knowledge from resolved work | Source-linked runbook or troubleshooting drafts drawn from confirmed cases | Diagnosis-to-confirmed-resolution duration in documented reuse cases, unsuccessful reuse and stale guidance corrections | Separate observed fixes from hypotheses; respect source permissions and freshness |

These measures are candidates, not assumed data or guaranteed effects. Define
the eligible population, units or denominators, source and observation period
for the chosen idea. Reviewing an output can diagnose a weakness; learning also
needs evidence of what happened after it and what it missed. Use acceptance
as an early signal, not a substitute for effects in the user's work.

## Match the first version to available feedback

Always suggest how feedback could improve the system's approach, but make the
loop optional. The user can choose useful work without an assessor or improver.
Prefer focused responsibilities over one Automation doing all of those jobs.

The same idea can start at different levels:

| Available evidence and authority | Useful first version |
| --- | --- |
| Inputs are accessible, but downstream results are not recorded | Propose recording subsequent decisions/results, their work references and unresolved cases; do not claim effectiveness yet |
| Humans already correct or accept outputs | Record decisions and reasons, then follow their effects and missed cases before making an improvement claim |
| Reliable checks and bounded actions are available | Produce reviewed changes with immediate checks and later outcome tracking |
| Work needs several owners, stages or approvals | Track work in GitHub issues or a project board, with explicit owners, statuses and handoffs. Add stages only where necessary. |
| Mature outcome evidence and update authority are agreed | Apply bounded configuration improvements with agreed checks and outcome comparison |

Self-improvement is specific to the task. A CI diagnosis assistant might improve
its evidence retrieval after missing logs led to wrong diagnoses, then compare
diagnosis time and error rate. A documentation assistant might refine which
changes require updates after rejected findings, then watch correction delay
and misunderstandings it still missed. A triage system might adjust routing
instructions after confirmed misroutes, then compare correct-route delay and
correction effort. Name who collects those results, who interprets them, when
pending outcomes are revisited and who approves the exact instruction change.
None should improve a metric by skipping difficult cases, suppressing failures
or widening its authority.

Use the [small feedback loop](../examples/small-feedback-loop.md) when outcome
tracking is simple, [separate assessment and improvement](../examples/separate-assessment-and-improvement.md)
when evidence needs its own context, or a
[specialized pipeline](../examples/specialized-pipeline.md) when work truly needs
multiple stages. Problem categories do not prescribe a member count.

## Make a recommendation concrete

For each suggestion, explain the current pain, first output, evidence needed,
human decision, optional measured learning path and main limitation. Rank
candidates by likely value, repeatability, available evidence, feasible action
and operating burden.
Keep the recommendation brief and human-readable: explain the choices and why
they help, not implementation fields, IDs or a grants matrix.
If those facts are unknown, label the suggestion tentative and ask the question
that best distinguishes the candidates.

For example, if the user spends time on incoming issues and release preparation:

> Two useful starting points are reviewed issue routing or a release-readiness
> brief. Routing could propose labels and record maintainer corrections so we
> can measure correct-route delay and review effort. A readiness brief could
> gather the checks and unresolved blockers your release owner already uses,
> without making the release decision. Which of those recurring jobs consumes
> more of your time today?

Then narrow the chosen problem, inspect representative inputs with permission
and recommend a concrete design with labeled operating defaults. A trial is an
option for a specific uncertainty, not a required phase. Do not create a full
pipeline merely because the catalog mentions several potential outputs.

## When to suggest something simpler or defer

A deterministic script, CI check, one-off agent task or clearer human policy
may solve the problem without recurring model-driven work. Say so.

Do not recommend unattended changes when the task is unbounded, the user cannot
state acceptable actions or a costly mistake cannot be checked before it matters.
Propose a reviewed deliverable when that creates value. If essential data is
inaccessible, explain the dependency rather than pretending a richer prompt
will fix it.

When outcomes are too delayed or rare to support a learning claim, offer useful
work with recorded evidence and scheduled outcome follow-up, possibly by a
person. Disclose incomplete observation rather than replacing it with a quality
score. For goals such as "make the team more productive," find a concrete
repeated workflow and an observable result instead of treating people or
activity counts as a score.
