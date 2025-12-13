# Integrating the Reference into Feature Development: Process Design Options

## Summary: Why are we meeting today?

The Rust specification process has hit a bit of an impasse. We have aligned on the overall plan of building on the *reference* as the most up-to-date, normative document describing the behavior of Rust, with the *FLS* trailing and adapting content for the safety-critical community. Our 2025H2 goals to author content and establish an FLS maintenance team are proceeding.

The *problem* is when we look past our 2025H2 goals to the longer-term question of how contribution to the reference should work. There we are hitting some core disagreements about whether structural changes are needed to the contribution process.

This document focuses on **process design options** rather than team composition or personnel. The key insight: different processes require different types and amounts of capacity. If we can agree on how the contribution process *should* work, the capacity questions become clearer.

## Context & Progress

The spec team was chartered to enact [RFC #3355][], which declares the intent to create a Rust specification that will

[RFC #3355]: https://rust-lang.github.io/rfcs/3355-rust-spec.html

> ...serve the needs of Rust users, such as authors of unsafe Rust code, those working on safety critical Rust software, language designers, maintainers of Rust tooling, and so on.

The RFC envisioned creating a new document that was "expected to replace the current Rust Reference". However, the team now sees building on the reference and the FLS as a more practical path:

1. The reference serves as the most up-to-date, normative document describing the behavior of Rust. Per [RFC #1636][], stabilization already requires the reference be updated.
2. The FLS "trails" the reference, bringing in the subset of additions that are relevant to the safety critical community.

[RFC #1636]: https://rust-lang.github.io/rfcs/1636-document_all_features.html

### 2025H2 Goals

For 2025H2 the team took two goals, both focused on making concrete progress. These goals are largely on track.

**[Maintaining the FLS](https://rust-lang.github.io/rust-project-goals/2025h2/FLS-up-to-date-capabilities.html)**: There is now a t-spec/fls team charged with extending the FLS to keep it up to date with each new Rust release. Many (but not all) members are drawn from the Safety Critical Rust Consortium. We have found that the FLS + Reference complement one another—adapting reference text for the FLS helps identify ambiguities and has even led to language changes in some cases.

**[Reference expansion](https://rust-lang.github.io/rust-project-goals/2025h2/reference-expansion.html)**: Ongoing additions to the reference are making progress. Authoring this content has been slow going because it requires "reverse engineering" the rules of the language to some extent. [Name resolution][#2055] and [divergence/type checking][#2067] have open PRs, though those PRs might not land by the end of the year. The team working on the project goal have also been working on an [experimental version of the reference that supports "feature-gated" content](https://rust-lang.github.io/project-goal-reference-expansion/) which [contains additional content](https://rust-lang.github.io/project-goal-reference-expansion/expansion-outline.html). This includes for example [feature-gated coverage of the trait solver](https://rust-lang.github.io/project-goal-reference-expansion/types/alias-types.html).

[#2055]: https://github.com/rust-lang/reference/pull/2055
[#2067]: https://github.com/rust-lang/reference/pull/2067

## Where We Align

There is broad agreement on several important points:

**Content experts own technical accuracy.** The lang/types/compiler teams are the ones that make the decisions that determine what the reference should express. When it comes to normative content, those teams are the ultimate source of authority.

**An editorial function is needed.** Someone needs to ensure the reference is coherent, accessible, and maintains a consistent voice. Raw technical content needs to be shaped into something readable by a broader audience.

**Teams don't want to do editorial work.** Multiple contributors have expressed that while they can provide technical content, the work of structuring and polishing documentation is distinct from their expertise:

> "I am an expert at [figuring out what to document on a technical level]. I am not an expert at figuring out how to structure this documentation. Doing that well is difficult and requires a lot of time."

> "I think even with T-types in control of the reference, there should be someone who takes dumps of information and puts them into a shape which is appropriate for the reference. We should review the final reference changes, answer any questions that come up during that process, and generally proactively provide them with the technical information they need."

**Reference updates should happen earlier in the development process.** Everyone agrees that waiting until stabilization to think about documentation creates friction. The question is *how* to integrate documentation work earlier.

**Subject matter teams should drive reference updates.** Lang-team and types-team champions should be responsible for ensuring reference updates happen, though contributors or feature owners can also drive the work.

## Where We're Stuck

### The Current Process

Under the [current process](https://github.com/rust-lang/reference/blob/master/docs/review-policy.md), authors write a PR that is accurate but not necessarily in the "editorial voice" of the reference. The t-lang/docs team assesses whether content is *understandable* and *defensible*, revising phrasing, organization, and style as needed before merging.

Contributors have shared what works well for them: office hours were widely praised, the reference is a high-quality readable document, and being able to write documentation in a "natural" style and lean on editors to clean it up is valuable.

### Contributor Experiences

Contributors have also shared where they feel friction:

> "I've been pretty disappointed with the pace and arbitrary nature of how reference changes have proceeded in the past. Unless the reference team wants to give us [...] the ability to greenlight changes to the reference ourselves so we can accelerate things that are blocked, then it really feels like we're stranding ourselves here."

> "If it were up to me we wouldn't even block language features on reference PRs being accepted."

> "The way the reference talks about the type system is incomplete, but even worse, the existing structure does not match our mental model/the implementation. This means that we may need to first restructure significant parts of it to document an actually small-ish change."

> "Right now the reference is an incredibly inaccessible walled garden where [I've been] told me multiple times that nobody should expect to be able to contribute a good PR to the reference and that it will need to be rewritten."

> "I'd be very discouraged if I had to, on top of the monumental effort of fixing the code itself and validating it didn't cause any regressions, also author or edit (or wait for someone to author/edit) a somewhat tedious section [...] In a really selfish way, it feels like punishing the author for trying to do something positive with the language."

The causes are multidirectional. t-lang/docs members report that it is difficult to get authors to "follow through" on reference changes, and that authoring is often an afterthought. The team is also understaffed (currently 2 members). From the other side, PR authors report it can be difficult to tell the state of a PR and what work is required to make it ready to land.

### Editorial Capacity

The current team size is insufficient for the integration goals we're discussing. But *how much* capacity we need depends on *what process* we choose—some approaches distribute editorial work differently than others

## Focus: Process Design

This document focuses on process options rather than team composition. Why? Because different processes require different types and amounts of capacity. Process choice will determine what editorial capacity we need and how it's distributed.

The question isn't "should we have staging?" We already have a staging area—open PRs *are* a staging area where content waits before entering the reference. The question is: **how should that staging area work?**

### Key Design Question: Editorial Workflow

We align on content teams writing first drafts (or at least providing technical accuracy). The design question is: **who does the editing and cleanup work?**

- Does editorial cleanup happen *before* content enters the staging area, or *after*?
- Does it happen incrementally per-PR, or in batches?
- Who has merge authority over different kinds of content?

Different answers to these questions lead to different process models with different capacity implications.

### Role of Formal Models

For the types team especially, prose in the reference may not be the right medium for specifying type system behavior. Projects like a-mir-formality and minirust serve as team-owned precision layers:

> "I feel like for T-types the answer to [precise specification] should be a-mir-formality and a better rustc impl, not the reference or the spec."

> "My expectation here is that improving the reference for type system related things, in the long term, ought to have no value for the types team itself because we should be maintaining our own more rigorous/involved pieces of documentation."

These formal models can serve as a "first draft" source—the precision layer that feeds into the reference staging process, with editorial work translating formal specifications into accessible prose.

## Four Staging Area Models

Here are four approaches to how the staging area could work. Each represents a different tradeoff between sync overhead, editorial workflow, and capacity requirements.

### Model A: PRs (Current Approach)

Open PRs serve as the staging area. Content waits in PRs until it meets editorial standards, then merges.

**Sync pattern**: Continuous per-PR sync—each PR must be brought to completion before merging.

**Editorial workflow**: Editorial review happens on each PR before merge. Authors write content; editors revise before accepting.

**Who has merge authority**: t-lang/docs team members.

**Capacity implications**: Requires editorial bandwidth to review each PR individually. Creates bottleneck if editorial capacity is limited.

**Strengths**: High quality bar for everything that lands. Reference always maintains consistent voice.

**Weaknesses**: PRs can stay open indefinitely. Unclear progress signals. Blocks authors who can't dedicate time to editorial iteration.

### Model B: Topic Branches

Long-lived branches for major topic areas (e.g., `types/trait-solver`, `lang/patterns`). Content accumulates on topic branches with lower editorial standards, then gets cleaned up and merged periodically.

**Sync pattern**: Periodic per-branch sync—topic branches merge into main on a cadence (like clippy).

**Editorial workflow**: Technical content can land on topic branches with minimal editorial friction. Editorial cleanup happens in batches before branch merges.

**Who has merge authority**: Subject matter teams have authority over their topic branches. Editorial team reviews at branch merge time.

**Capacity implications**: Editorial work happens in batches rather than per-PR. Subject matter teams can make progress without waiting for editorial review.

**Strengths**: Unblocks authors. Separates "is this technically correct?" from "is this editorially polished?" Subject matter teams have ownership.

**Weaknesses**: Topic branches can drift. Batch editorial work may be harder than incremental. Risk of branches never reaching merge-ready state.

### Model C: Nightly vs Stable Reference

Two separate reference instances: a "nightly reference" where content lands with lower standards, and a "stable reference" that maintains high editorial quality. Content flows from nightly to stable.

**Sync pattern**: Periodic repo sync—like how clippy tracks rust-lang/rust. Nightly reference tracks main, stable reference pulls from nightly.

**Editorial workflow**: Nightly reference accepts technically-correct content with minimal polish. Editorial team reviews content when promoting to stable.

**Who has merge authority**: Subject matter teams can merge to nightly. Editorial team controls stable.

**Capacity implications**: Similar to Model B—batch editorial work at promotion time. Clear separation of technical vs editorial review.

**Strengths**: Clear mental model ("nightly" for work-in-progress, "stable" for polished). Subject matter teams can move fast on nightly.

**Weaknesses**: Two repos to maintain. Users may be confused about which reference to consult. Content may languish in nightly.

### Model D: Feature Flags

Content lives in the main reference but behind feature flags (similar to `#![feature(...)]` in the language). Flagged content is visible but marked as draft/unstable.

**Sync pattern**: Continuous sync per PR—content lands immediately but flagged. Editorial work removes the flag.

**Editorial workflow**: Authors land technically-correct content behind flags. Editorial team polishes and removes flags.

**Who has merge authority**: Subject matter teams can merge flagged content. Editorial team controls flag removal.

**Capacity implications**: Editorial work can happen asynchronously after content lands. No batch pressure.

**Strengths**: Content is immediately visible and useful. Clear signal of what's draft vs polished. Incremental editorial progress.

**Weaknesses**: Requires tooling for flags. Users see draft content (maybe confusing). Risk of content staying flagged forever.

## Example Scenarios

*[To be developed: How would each model handle typical situations?]*

- A new language feature is stabilizing and needs documentation
- A types team member discovers the current reference structure doesn't match their mental model
- An enthusiastic contributor wants to document an underdocumented area
- A safety-critical user needs to know exactly what changed between Rust versions

## Discussion Questions

Rather than prescribing a solution, we're bringing these options for broader discussion:

1. **Does framing the current process as "a staging area that could work differently" resonate?** Or does this framing obscure important considerations?

2. **Which tradeoffs matter most to you?** Speed of landing content vs. consistent quality? Subject matter team autonomy vs. editorial coherence?

3. **What would make you comfortable blocking on reference updates?** The types team has been clear that current friction makes blocking unacceptable. What would have to change?

4. **Are there hybrid approaches worth considering?** For example, Model A for small changes + Model B for major new chapters?

5. **What experiments could we run?** Rather than committing to a wholesale process change, what targeted experiments might help us learn?
