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

1. The reference serves as the most up-to-date, normative document describing the behavior of Rust.
2. The FLS "trails" the reference, bringing in the subset of additions that are relevant to the safety critical community.

[RFC #1636]: https://rust-lang.github.io/rfcs/1636-document_all_features.html

### 2025H2 Goals

For 2025H2 the team took two goals, both focused on making concrete progress. These goals are largely on track.

**[Maintaining the FLS](https://rust-lang.github.io/rust-project-goals/2025h2/FLS-up-to-date-capabilities.html)**: There is now a t-spec/fls team charged with extending the FLS to keep it up to date with each new Rust release. Many (but not all) members are drawn from the Safety Critical Rust Consortium. We have found that the FLS + Reference complement one another—adapting reference text for the FLS helps identify ambiguities and has even led to language changes in some cases.

**[Reference expansion](https://rust-lang.github.io/rust-project-goals/2025h2/reference-expansion.html)**: Ongoing additions to the reference are making progress. Authoring this content has been slow going because it requires "reverse engineering" the rules of the language to some extent. [Name resolution][#2055] and [divergence/type checking][#2067] have open PRs, though those PRs might not land by the end of the year. The team working on the project goal have also been working on an [experimental version of the reference that supports "feature-gated" content](https://rust-lang.github.io/project-goal-reference-expansion/) which [contains additional content](https://rust-lang.github.io/project-goal-reference-expansion/expansion-outline.html). This includes for example [feature-gated coverage of the trait solver](https://rust-lang.github.io/project-goal-reference-expansion/types/alias-types.html).

[#2055]: https://github.com/rust-lang/reference/pull/2055
[#2067]: https://github.com/rust-lang/reference/pull/2067

## Where We Align

### Reference updates should happen earlier

For the reference to help our process, we need to make keeping it up-to-date a more integral part of our process. We shouldn't wait until stabilization to write reference content. The act of writing reference content can help to find bugs and edge cases and it would be better to do it as part of the RFC process.

### Split responsibility

The responsibility for the content in the reference should be divided between:

* **content teams**, like lang, types, and opsem, that own the actual **rules being documented**. Ultimately these types need to be aligned with the content in the reference;
* an **editorial team**, currently t-lang/docs, that is responsible for the coherence, accessibility, and professional tone of the reference.

Members of content teams do not want to do editorial work:

> "I am an expert at [figuring out what to document on a technical level]. I am not an expert at figuring out how to structure this documentation. Doing that well is difficult and requires a lot of time."

## Where We're Stuck

The general shape of the process that we want seems clear:

* Contributors author a "first draft" of the reference content, focused on getting their understanding correct.
    * Lang team champions in particular are expected to help out with this work (possibly writing the reference material themselves).
* This first draft is vetted by the content team.
    * This will involve some iteration but the result may not be accessible to people outside the team, may use jargon, etc.
    * The types and opsem teams will also prototype in a-mir-formality and mini-rust respectively.
* The editorial team makes changes and improvements.
    * Lang team champions in particular are expected to help out with this work (possibly writing the reference material themselves).

But there are many differences in the details. This section explores a variety of options.

### Overview of process alternatives

We present four alternative processes for how this process might work. All of them are grappling with a few core issues:

* How shall we maintain reference text while it is being authored?
* How shall we combine the input from the content team, editorial team, and PR authors?
* How can we keep the reference up-to-date while ensuring that there is a coherent document ready to be published every 6 weeks?

The four alternatives are as follows:

1. *Long-lived PRs* (current process), where reference content is kept in a pending PR until it is ready to be merged (in the case of new features, this means that the feature has been stabilized). This holds the highest bar for quality of the reference itself, but comes at the cost of long review times and the need for PR authors to regularly rebase.
2. *Feature-gated text* (proposed by the reference expansion team), where reference content can be landed in tree but with various "feature gates" that indicate whether the text (a) has been approved by the content team; (b) has been approved by the editorial team; and (c) covers a stable Rust feature. Text is considered unstable and subject to arbitrary changes until all three conditions are met.
3. *Nightly reference*, a middle-ground between options 1 and 2, in which there is a single topic branch ("nightly") that contains feature-gated reference text. Reference changes land first in nightly and then are "ported over" to the reference when they've reached a required level of stability.

Naturally, whichever variant we choose, there are many fine-grained details to be worked out. The focus for this meeting should be the high-level direction.

### Option 1: Long-lived PRs (current process)

Under the [current process](https://github.com/rust-lang/reference/blob/master/docs/review-policy.md), changes to reference documentation stay in PRs until they meet three criteria:

> * **Understandability**: Language within the Reference should be understandable to most members of the Project. Contributions should assumes that readers are familiar with the rest of the content of the Reference, but, wherever possible, sections should facilitate that understanding by linking to related content.
> * **Defensibility**: When the lang-docs team merges a change to the Reference, they are agreeing to take responsibility for it going forward. Team members need to feel confident defending and explaining the correctness of content within the Reference. Whenever possible, changes to the Reference should back up any claims with concise examples to verify correctness.
> * **Voice**: Authors are not expected to have competence as a specification writer when drafting new contributions to the Reference. So long as claims are understandable and defensible, it is fine for PRs to be written in a casual tone or with the voice of the author instead of the voice of the Reference. Team members will bring editorial experience as part of their reviews and will revise the phrasing, organization, style, etc. to fit the Reference before merging if necessary.

The intended flow is that authors create an initial draft and the editorial team leaves commits and/or applies rewrites. The editorial team meets during weekly "office hours" to discuss these proposed suggestions and contributors are welcome to join. Once the PR reaches a state where it meets the above threshold, it can be merged. PRs do not need to be *complete* -- they may reference "to be written" parts of the reference -- but the text that is included needs to meet the bar for publication.

#### Connection to feature stabilization

Per [RFC #1636][], stabilization requires a reference chapter to be available.

#### Pro: high quality and editorial consistency

The current reference is a readable document with a profesional tone. Recent changes have adopted several the FLS's practice of labeling paragraphs and linking tests, making for easy reference, and PRs have also begun to include inline tests as evidence of defensibility (i.e., including a test that demonstates the language rule being described).

The team works to ensure that each release of the reference reads "like a book", with consistent terminology and structure across chapters.

#### Pro: easily visible diff

Keeping the changes for a language feature or reference update in a PR has the advantage that it is easy to view the diff for a new feature. This is useful during stabilization for example.

#### Pro: editorial review by non-experts helps to find bugs

TC put it like this:

> As a meta point that often comes up in discussions about this, it's worth emphasizing that editorial review is not generally separable from determining that it's feature complete. It's the process of going over it with a fine-tooth comb that routinely uncovers the semantic problems and gaps. That we also might switch "a"s to "an"s in this step is not what dominates the cost.

It is not the case that only the editorial team *can* do this (others do as well), but (today) it is often the editorial team that *does* do it.

#### Con: long PR review time, sense of "PR limbo"

In practice, the data suggests that many PRs are merged quite quickly but the remainder can stay open for a long time (the following chart contains PRs since the start of 2024):

```
PR Status Distribution (Non-trivial PRs: ≥5 line changes)
Merged: n=289, Open: n=79, Draft: n=7
==========================================================================================
Time Range   │ Merged                 │ Open (ready)          │ Draft          
──────────────────────────────────────────────────────────────────────────────────────────
0-1 weeks    │ ██████████ 139 (48.1%) │             0 ( 0.0%) │             1 (14.3%)
1-4 weeks    │ █████      70 (24.2%)  │             3 ( 3.8%) │             0 ( 0.0%)
1-3 months   │ ███        55 (19.0%)  │ █          14 (17.7%) │             1 (14.3%)
3-6 months   │ █          24 ( 8.3%)  │ ██         32 (40.5%) │             2 (28.6%)
6-9 months   │             1 ( 0.3%)  │             6 ( 7.6%) │             0 ( 0.0%)
9-12 months  │             0 ( 0.0%)  │             5 ( 6.3%) │             0 ( 0.0%)
1+ years     │             0 ( 0.0%)  │ █          19 (24.1%) │             3 (42.9%)

Merged stats: median=8.1 days, min=0.0, max=184.9
Open stats:   median=168.0 days, min=7.1, max=2314.0
Draft stats:  median=122.7 days, min=1.3, max=1560.1

Filtered out 109 trivial merged PRs
Filtered out 12 trivial open PRs
Filtered out 1 trivial draft PRs
```

These long delays mean that contributors are reluctant to block their changes on having landed changes to the reference. I want to emphasize that these quotes are shared in the spirit of a "no fault" analysis and are not meant to assign blame to the existing reference maintainers.

> "I've been pretty disappointed with the pace and arbitrary nature of how reference changes have proceeded in the past. Unless the reference team wants to give us [...] the ability to greenlight changes to the reference ourselves so we can accelerate things that are blocked, then it really feels like we're stranding ourselves here."

> "I'd be very discouraged if I had to, on top of the monumental effort of fixing the code itself and validating it didn't cause any regressions, also author or edit (or wait for someone to author/edit) a somewhat tedious section [...] In a really selfish way, it feels like punishing the author for trying to do something positive with the language."

#### Con: difficult for multiple authors to collaborate; hard to point people at the content


### Option 2: feature-gated edits

Josh Triplett and the team working on the reference expansion goal have been experimenting with an alternative process, [*feature-gated edits*](https://rust-lang.github.io/project-goal-reference-expansion/). In this process, new text can be landed in the reference and tagged with various kinds of feature-gates that indicate what remains to be done before the code can be considered stable:

* Needs content-team review
* Needs editorial review
* Needs stabilization (documents behavior of an unstable feature)
* ...and of course there are more possibilities, e.g., needs test, etc.

The precise process for managing this content would have to be worked out, but the general idea is something like this

* Any content team can land feature-gated content in the reference
* Removing the "needs X review" tag requires approval by the team (e.g., FCP)
* Once the text has been reviewed, the team should be looped into further changes
    * e.g., once the types team has signed off on something, they should be cc'd on editorial changes
    * and once the editorial team has signed off on something, they should be cc'd on any fixes

Some questions to consider:

#### Pro: Enables iteration by multiple authors

This setup naturally scales to iteration and contribution from multiple authors. For example, the implementor and the champion of a lang feature could both review each others PRs, just as they do on the compiler, until the reference text has reached a state where they consider it ready for review by the broader team.

Because text has landed upstream, it is also easy to point other people at it for review (just give them the URL).

#### Pro: Refactoring and terminology changes can be applied uniformly

Having "in-progress" text edits landed in the repository would have the same 

* Does it make sense to land "refactors" to stable content to "make room" for feature-gated content, even if that makes the stable content more complex?
    * Example: if creating new rules for temporaries would work better with a different formulation that is more complex than what's needed for the current rules, should we do so?

#### Con: Reference includes "unstable" text

The current reference includes only "stable" text, making it suitable for viewing from outside the Rust project. A reference that includes draft text might work against the "reputation for stability" that would otherwise come from a polished, well maintained specification.

It is possible that feature-gated text could be hidden by default, but that would require careful work to ensure that the remaining text makes sense without it -- there is no "type checker" for the reference to avoid, for example, errant references. The current prototype does not include this capability.

Many people have observed that feature-gated text also fails to account for the fact that adding new content into the reference often requires changes to accompanying, stable sections, so that they can be "widened" to accommodate the new semantics. It is possible that these changes could land however as independent refactorings.

#### Con: text may never receive polish

Without the clear list of "open PRs", it is harder to see what text needs to be polished. The editorial team expressed concerns that feature-gated or unstable content would become a dumping ground.

### Option 3: "nightly" reference

As a middle ground on the above, we could have two distinct copies of the reference. The "nightly" reference would contain feature-gated text. All edits would go first into nightly. At the point where a feature is ready to be stabilized, its text could be "upstreamed" to the stable reference.

This would be similar to how clippy and the compiler interact, except that (ideally) work would only land on the nightly, so that two-way synchronization is not required.

#### Pro: the stable reference presents a polished picture, the nightly reference allows for easy iteration

This middle ground corrects some of the concerns that could come from a reference with unstable content.

#### Con: more coordination cost

It would be more work to lift text from the "nightly" to the "stable" reference and there is already a lack of review bandwidth.

## Appendix

Scatter charts for merged vs open PRs by size

```
MERGED PRs: Size vs Time to Merge
Points: 398 total, 397 shown (≤2000 lines, ≤1000 days)
================================================================================
Age (days)
   185 ┤
       ┤░                                                                     
       ┤░                                                                     
       ┤░                                                                     
       ┤                                                                      
       ┤░                      ░░                                             
       ┤                                                                      
       ┤░      ░                                                              
       ┤▒▒                   ░                                                
       ┤░                                                                     
       ┤░░                                                                    
       ┤   ░░ ░░                  ░                                           
       ┤▒    ░    ░                                                           
    92 ┤▒                                                                     
       ┤░ ░                                                                   
       ┤▒░                                                                    
       ┤ ▒░ ▒                                                                 
       ┤░░ ░  ░       ░              ░                                        
       ┤░   ░                                                                 
       ┤█░▒     ░░                                                            
       ┤▓▒                                                                    
       ┤█▓ ░  ▒                                                               
       ┤█▓░░▒▒       ░                                                        
       ┤█▓▒▒░░                                                                
       ┤█▒▓░▒░░       ░ ░      ░     ░                                        
     0 ┤████▓▒▒▓░░      ░     ░▒                             ░               ░
       └──────────────────────────────────────────────────────────────────────
        1                                                                 1741
                              Size (lines changed)

Density: ░=1 point, ▒=2-3 points, ▓=4-7 points, █=8+ points
Stats: avg size=62 lines, avg age=21.1 days

OPEN PRs: Size vs Age Since Opening
Points: 99 total, 86 shown (≤2000 lines, ≤1000 days)
================================================================================
Age (days)
   958 ┤
       ┤░                                                                     
       ┤                                                                      
       ┤                                                                      
       ┤                                                                      
       ┤                                                                      
       ┤                                                                      
       ┤▒                                                                     
       ┤░░          ░                                                         
       ┤░                                                                     
       ┤                                                                      
       ┤                                                                      
       ┤                                                                      
   480 ┤░              ░                                                      
       ┤▒     ░                                                               
       ┤░      ░     ░                                                        
       ┤░                                                                     
       ┤░                                                                     
       ┤░       ░░                                                            
       ┤░░   ▒                                                                
       ┤▒                                                                     
       ┤▒▓▓▒░░░▒░  ░ ░ ░░░                                                    
       ┤▓░     ░░                                                             
       ┤▓░░ ░      ░                                                          
       ┤▓░▒ ░  ░░                           ░                                 
     1 ┤▓░             ░                                     ░               ░
       └──────────────────────────────────────────────────────────────────────
        2                                                                 1555
                              Size (lines changed)

Density: ░=1 point, ▒=2-3 points, ▓=4-7 points, █=8+ points
Stats: avg size=128 lines, avg age=218.3 days
```