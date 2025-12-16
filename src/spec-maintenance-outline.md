# Integrating the Reference into Feature Development: Process Design Options

## I. Context & Progress
- Spec team has made real progress: Reference + FLS approach established
- 2025H2 goals on track: content creation, FLS maintenance via safety critical consortium

## II. Where We Align
- Content experts (teams) own technical accuracy
- Editorial function needed for coherence and accessibility
- Teams don't want to do editorial work, editors need technical expertise
- Reference updates should happen earlier in development process, not just at stabilization
- Lang-team/types-team champions responsible for driving reference updates (though contributors/owners can also drive the work)
- [Include supportive quotes from conversations]

## III. Where We're Stuck
- Theory vs practice gap in content ownership
- Current process creates bottlenecks and friction [evidence from conversations]
- Editorial capacity constraints - current team size insufficient for integration goals
- Team composition questions linked to process choices

## IV. Focus: Process Design
- This document focuses on process options rather than team composition
- Key insight: Different processes require different types and amounts of capacity
- Process choice will determine what editorial capacity we need and how it's distributed
- Get the process right, and the capacity questions become clearer

### IV.5. Key Design Question: Editorial Workflow
- Content teams write first drafts (we align on this)
- But who does editing and cleanup? (varies by model)
- Where does editorial bandwidth come from? (capacity implications differ by model)
- Role of formal models (a-mir-formality, minirust): team-owned precision layer, can serve as "first draft" source before staging

## V. Four Staging Area Models
- **Model A: PRs (Current)** - Open PRs as staging area
- **Model B: Topic Branches** - Long-lived branches, periodic per-branch sync (like clippy)
- **Model C: Nightly vs Stable Reference** - Separate repos, periodic repo sync (like clippy)
- **Model D: Feature Flags** - Content in main Reference behind flags, continuous sync per PR

## VI. Example Scenarios
- How would each model handle typical development situations?
- [Work through concrete examples for each model]

## VII. Next Steps/Recommendations
- [TBD - pilot approach, lang team choice process]
