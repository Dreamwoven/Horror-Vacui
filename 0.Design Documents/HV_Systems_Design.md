# Horror Vacui Systems Design
This document overviews how issues are identified and how implementation of features and changes is accomplished.

## Sources of Truth
- Horror Vacui Wiki.odt provides the original grounding document for design decisions.
- HV_Release_History.md provides a comprehensive release history for every single version of Horror Vacui; this document can be used to discern how HV arrived at its current state.

## Identifying Issues
TBD

## Implementation
### Versioning
Horror Vacui uses Major.Minor.Patch versioning - Major is reserved for drastic changes to gameplay, Minor is for substantial changes to code or new additions, and Patch is for updates that focus entirely on small tweaks or bug fixes.

### Implementation Guidelines
Horror Vacui uses a "push and shuffle" development cycle.
This means, in order:
- 1) Introduce a new feature or drastic change if there are no outstanding issues - this is the push.
- 2) Assure quality in implementation and ethos, adjust numbers for maximum efficacy or design adherence - this is the shuffle.