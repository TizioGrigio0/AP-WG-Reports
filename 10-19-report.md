---
title: "Advanced Programming Working Group"
date: 2026-10-19
---

# Attendance

16 of 18 groups present.

Present: Rhaast, Zev, CookedCrab, in rust we t(rust), CrabRavers, Rusty Krub, c-lions, Rust Do It, Rust in Case, unsafe { }, Whole Lotta Panic, The Larpers, Rustein Island, Firestarters, The_Untitled_Group, Unwrap() and pray

Absent: Crabs harbour, Rust in peace

# Summary

The first working group meeting set up the process for correcting the protocol document and reviewed the first pull requests against chapter 2. The coordinator presented the GitHub repository and the rules for issues and pull requests, read chapters 1 and 2 aloud, and went through three pull requests, an example of stacked pull requests and a new issue on the Map struct. Following a suggestion by a group leader, the coordinator settled that the missing Cliff variant is added to Terrain now and that splitting the enum is deferred; the coordinator also offered to merge the archipelago_radius type fix if nobody objected. The professor set requirements for every report and advised the coordinator to delegate repository work to maintainers. Two teams were candidates for the geometry contract; no contractor was chosen and the scope of the work remains open.

# Guidance from the Professor

- Every working group report must state what was done, which groups were present and which were missing, and whether each absence was announced in advance; frequent absences reflect poorly on the groups concerned.
- The coordinator should delegate rather than tag and process every contribution personally, ideally appointing one or two maintainers (for example one for tagging requests and one for resolving them) within a flexible organisational structure.
- Reports should give a short recap of each discussion and the tally of any vote as counts of yes, no and abstain, without naming who voted for what.
- Trivial fixes waste meeting time; the working group should find a quicker way to handle them, and the meeting can end as soon as the agenda is done.
- This meeting and the next are about agreeing on a standard workflow that everybody follows, since problems can only be solved once the group agrees on how to solve them.
- The working group should report what the contractors did so that the effort of contractor work can be estimated.

# Discussion

## 1. Repository workflow and rules for issues and pull requests

The coordinator presented the repository that holds the original protocol file and an editable LaTeX copy, and an explanatory file on how to write issues and pull requests. Many submitted pull requests bundled several fixes, often because contributors worked on their fork without separate branches, so later pull requests carried the fixes of earlier ones. Titles must name the chapter and the problem so that a reader understands what, where and why without searching the 23 page document.

- **Coordinator**: Required one problem per pull request on its own branch, and explained how to split stacked pull requests by extracting single commits.
- **A group leader**: Explained that most stacked pull requests came from their team, which built each fix on top of its own corrected version.
- **A group leader**: Added that many of the problems arise because the repository does not compile.
- **Professor**: Said that the point is a standard every contributor follows, including people who join later.

**Outcome:** Pull requests must fix a single problem on a separate branch, and stacked pull requests are to be split.

## 2. Reading of chapters 1 and 2 and whether every enum must be non-exhaustive (section 1.1)

The coordinator read chapter 1 (participants and the four message sets) and chapter 2 (geometry, map, small types and Params) so that everyone shares the same picture. An existing issue argues that not every enum should be non-exhaustive, since Direction represents a fixed set of six values, contrary to the rule that every enumeration is non-exhaustive without exception.

- **Coordinator**: Flagged the non-exhaustive issue as worth discussing in class and postponed it in favour of pull requests that could be decided immediately.

**Outcome:** The coordinator suggested discussing the non-exhaustive question in class later; no date was set.

## 3. Missing Cliff variant in the Terrain enum (section 2.4)

The Terrain enum lists Clear, Rock and Island but the text defines land as Island or Cliff, so Cliff is missing. The pull request also bundled the file needed to compile the protocol, which breaks the one problem per pull request rule; the coordinator said it might be accepted this time but not again. A suggestion on the pull request proposed splitting Terrain into a water type and a land type, which led to a discussion of how each option encodes movement and rock damage.

- **A group leader**: Supported splitting Terrain into water and land types, since this encodes the different behaviour of passable and impassable cells in the types.
- **Another group leader**: Argued that four variants are not a problem given the is_land and is_water functions, and that two enums make functions taking any terrain harder to write.
- **Coordinator**: Objected that with only two outer variants it becomes harder to tell Clear from Rock.
- **Coordinator**: Asked how the split would handle the extra cost of entering a rock.
- **A third group leader**: Observed that a match ends up with four arms either way, whether flat or nested.
- **Another group leader**: Proposed adding Cliff now and handling the split in a separate pull request later.

**Outcome:** The coordinator settled that Cliff goes into the existing enum for now; splitting Terrain into water and land types was deferred. No vote was counted.

**Links:** https://github.com/TizioGrigio0/AP-archipelago-protocol/pull/1

## 4. Return type of Map::water_cells (section 2.4)

water_cells returns a HashSet, whose iteration order is arbitrary, while the comment above it requires a fixed order; since games must be reproducible from the seed, this is a defect. The pull request proposes a Vec, and a comment on it proposes a plain iterator like clear_cells, which works as long as the underlying structure has a deterministic order.

- **Coordinator**: Presented the pull request and the reasoning that a HashSet cannot give a reproducible order.
- **A group leader**: Preferred a plain iterator, so that callers can add filters and pay for one traversal instead of building a collection first.
- **A group leader**: Asked that water_cells and clear_cells return the same kind of value, both Vec or both iterators.

**Outcome:** Not decided; the return type depends on how the Map struct stores its cells, which has to be specified first.

**Links:** https://github.com/TizioGrigio0/AP-archipelago-protocol/pull/2

## 5. Underspecified Map struct (section 2.4)

The Map struct in the protocol is empty, so nobody knows how cells are stored, and decisions such as the water_cells return type cannot be made yet. No issue existed for it, so the coordinator opened one during the meeting as a worked example of a well formed issue (issue 21) and connected it to the water_cells question.

- **A group leader**: Pointed out that the Map struct is unspecified and suggested not imposing a data structure, only requiring a deterministic order.
- **Coordinator**: Called the Map struct the source of the problem, to be fixed before the water_cells question.

**Outcome:** The coordinator stated that the Map struct must be specified before the water_cells question can be settled, and left both for later.

**Links:** https://github.com/TizioGrigio0/AP-archipelago-protocol/issues/21

## 6. Type of archipelago_radius in Params (section 2.6)

Params declares archipelago_radius as a String although it is a number. The coordinator considered the fix correct but criticised the pull request for not stating which section it concerns.

- **Coordinator**: Proposed merging the fix directly unless someone objected.
- **Professor**: Remarked that trivial fixes like this waste meeting time and should be handled more quickly.

**Outcome:** The coordinator offered to merge the fix unless someone objected; no objection is recorded.

**Links:** https://github.com/TizioGrigio0/AP-archipelago-protocol/pull/4

## 7. Compiling the protocol and keeping protocol.pdf in the repository

The LaTeX source in the repository does not currently compile. A group leader questioned whether a committed protocol.pdf only causes confusion and suggested building it automatically.

- **A group leader**: Proposed removing protocol.pdf and adding a GitHub Action that builds it on every merge or attaches it to releases.
- **Coordinator**: Said they plan to keep the PDF in the repository for now.

**Outcome:** The coordinator said a pending pull request that makes the protocol compile would be merged; whether to keep protocol.pdf in the repository was left open.

## 8. Geometry contractor candidates and scope

Two teams, Rusty Krub and Rust in Case according to the scribe notes, were candidates for the shared hex geometry library. The coordinator did not choose between them and asked for their plans in private. A candidate asked whether wind and line of sight belong to the geometry contract.

- **A group leader**: Asked about sending their team's plan for the geometry part; the coordinator asked for it in private.
- **Another group leader**: Said their team had already pushed geometry corrections and started on visibility, and asked whether wind and line of sight are part of the contract.
- **Coordinator**: First suggested that weather could eventually be added to the geometry work.
- **Coordinator**: Recalled reading that the contractors do geometry and line of sight.
- **Coordinator**: Then argued that weather interacts with directions rather than the map, so it does not belong in geometry.
- **A third group leader**: Agreed that weather is not a big part of the geometry work.

**Outcome:** No contractor was chosen; the candidates will send their plans privately, and the exact scope, in particular weather, is still to be confirmed.

# Decisions

1. Each pull request fixes a single problem on its own branch; stacked pull requests are to be split.
2. Cliff is added to the existing Terrain enum for now, and splitting Terrain into water and land types is deferred.

# Action Items

| Owner | Task | Deadline |
|:--------------------|:--------------------------------------|:-------|
| Coordinator | Merge the pull request adding Cliff to Terrain. | TBD |
| Coordinator | Merge the archipelago_radius type fix, as offered, unless someone objects. | TBD |
| Authors of stacked pull requests | Split them into separate pull requests that each fix one problem. | TBD |
| Geometry contractor candidates | Send their plans for the geometry contract to the coordinator in private. | TBD |
| Coordinator | Hand issue and pull request review over to a maintainer, as announced in the meeting. | TBD |

# Open Issues for the Next Working Group

1. Whether Terrain should be split into separate water and land types (section 2.4).
2. The return type of water_cells, and whether it should match clear_cells, pending the Map struct (section 2.4).
3. The internal representation of the Map struct is unspecified (issue 21).
4. Whether every enum must be non-exhaustive, given that Direction is a fixed set (section 1.1).
5. Whether protocol.pdf should stay in the repository or be built automatically, for example by a GitHub Action; the LaTeX source compiles only once the pending fix is merged.
6. Which team becomes the geometry contractor, and whether weather and line of sight are part of the contract.
