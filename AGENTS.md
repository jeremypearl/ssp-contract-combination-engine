# SSP Contract Combination Engine

This repository contains an accounting application that replaces a complex Excel workbook used for:

- SSP allocation
- Contract combination analysis
- Revenue recognition
- Revenue true-ups
- Journal entry support
- Audit packages

Rules:

1. Calculations must be deterministic and testable.
2. Do not use AI to make accounting conclusions.
3. Technical Accounting owns accounting judgments.
4. Approved scenarios cannot be edited.
5. Locked periods cannot change unless reopened through an approved workflow.
6. Every override must include:
   - reason
   - user
   - timestamp
   - reviewer
7. Build features in small phases.
8. Create tests for every calculation.
9. Prioritize correctness over UI design.
10. Explain all technical decisions in plain English.
