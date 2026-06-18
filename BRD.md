1. Executive Summary
Commercial Accounting requires a web-based application to automate the operationalization of SSP, contract-combination analysis, revenue allocation, revenue recognition true-ups, and related accounting outputs for complex customer contract scenarios. A specific example is the OpenAI arrangement, which includes multiple order forms, TCV reduction mechanics, cash credits, stock warrants or equity-related consideration, late-delivery credits, terminable tranches, and material-right analysis. The same pattern is expected to apply to other strategic customers such as Meta.
By Q4, approximately 20-30% of revenue is expected to flow through these complex arrangements. The current workbook-based process is time consuming, must be refreshed multiple times per contract and per quarter in some cases, and is exposed to errors from manual row copying, formula maintenance, locked/unlocked period changes, and repeated scenario updates. The desired future state is a controlled web application that lets users select contracts and order forms, define the combination scenario, input accounting judgments, update delivery and actuals data, run the allocation engine, and produce audit-ready outputs.
2. Business Problem
Current process:
Users maintain a large Excel workbook with separate tabs for updates, main allocation logic, prior-period comparison, not-combined analysis, order-form tie-outs, SSP rates, material-right assessment, material-right lookback, review steps, and entry summaries.
The workbook requires row-set duplication for new monthly or modification scenarios, manual notation of what changed, monthly actual and forecast updates, and manual review of prior-period columns that should move from unlocked to locked.
Contract modifications, high-confidence delivery date changes, termination rights, late-delivery credits, and new order forms can require the contract-combination analysis to be re-spun multiple times.
The process must reconcile revenue allocation outputs, monthly revenue recognition, journal-entry impacts, tie-outs to order forms, and review evidence.
Pain points:
High manual effort to populate and refresh the template every month and for each modification.
 Error risk from manual formulas, hardcoding, row insertion, month-column updates, and inconsistent lock status handling.
Limited scenario control when comparing combined versus not-combined views or when deciding which contracts/order forms belong in a combination set.
Limited auditability of changes, approvals, source data, assumptions, and generated journal-entry outputs.
Difficult reuse across strategic customer arrangements that share similar mechanics but differ in SKUs, contracts, credits, delivery dates, and accounting judgments.
3. Objectives
Reduce the time required to perform monthly SSP reallocation and contract-combination updates.
Improve calculation accuracy, consistency, and repeatability for high-value strategic customer contracts.
Provide a controlled workflow for contract selection, scenario creation, technical accounting inputs, monthly actuals, modifications, review, and approval.
Create audit-ready outputs including allocation detail, revenue-recognition schedules, true-up summaries, journal-entry support, order-form tie-outs, and reviewer sign-offs.
Support reuse across OpenAI, Meta, and future key customer arrangements without rebuilding the logic in spreadsheets each time.
4. Scope
Area
In Scope
Notes
Contract workspace
Customer, arrangement, order-form, amendment, and scenario setup.
Users should be able to choose which contracts/order forms to combine and compare outcomes.
Source data intake
SFDC contract/order-form data, SKU-level line items, quantities, dates, prices, locations, credits, amendments, and status.
Initial release may support file upload if APIs are not ready.
Accounting inputs
SSP period/rates/ranges, POB flags, material-right assumptions, terminability, discount treatment, modification effective dates, and lock decisions.
Technical accounting remains accountable for judgments and policy conclusions.
Calculation engine
Service hours, rates, discounts, net selling price, SSP guide, relative allocation, revenue allocation, reallocation, rev-rec rate, and monthly revenue recognition.
Engine must be deterministic, explainable, and testable.
Monthly close workflow
Actuals, forecasts, HCDD updates, late-delivery credits, locked/unlocked month handling, true-up calculations, and JE support.
Prior-period treatment is a key control point.
Audit and reporting
Version history, change descriptions, validation checks, review workflow, tie-outs, exports, and audit package.
Audit evidence should be generated from the application, not manually assembled.

 
Out of scope for initial release:
Automated legal or technical accounting conclusions without human approval.
Revenue subledger, Revenue Orchastrator, or contract lifecycle management systems.
Contract drafting or amendment negotiation workflow.
Fully automated interpretation of contract language from unstructured documents unless approved as a later AI-assist feature with controls.
Direct journal posting unless separately approved by Finance, IT, and SOX
5. Current-State Process
Process Area
Current Workbook Behavior
Automation Implication
Scenario setup
Users copy a prior set of rows, leave blank rows, and write a description of monthly or modification changes.
Application should create a versioned scenario from a prior scenario and require change reasons.
Modification handling
Users review revenue columns that were previously unlocked and determine whether they should become locked after modification.
Application should enforce lock-transition rules and show prior versus current values.
Monthly updates
Users update HCDDs, late-delivery credits, period labels, delivered quantities/hours, and actual/forecast month headers.
Application should provide a monthly close workflow with controlled actual/forecast status.
Revenue true-up
Workbook calculates true-up only for prior months that remain unlocked and excludes locked months.
Application should compute true-ups from explicit locked/unlocked flags.
Accuracy check
Users compare prior-period delivered quantities and revenue against the prior row set to confirm no unintended changes.
Application should run automated variance checks and require review resolution.
Review evidence
A review tab tracks validation steps, initials, dates, comments, and posting confirmation.
Application should support review tasks, sign-off, comments, and evidence export.

 
6. Future-State Solution
The future-state application should behave as a scenario-based accounting engine rather than as a static spreadsheet clone. Users should work in a customer arrangement workspace, select contracts and order forms, define accounting assumptions, update data, run calculations, resolve validations, and export standardized outputs.
Module
Purpose
Expected Capabilities
Portfolio dashboard
Manage strategic customer arrangements.
View customers, active scenarios, close status, pending reviews, open variances, and latest calculation run.
Contract combination workspace
Define which contracts/order forms are analyzed together.
Select SFDC contracts, amendments, order forms, and modification events; run combined and not-combined scenarios.
Line-item editor
Maintain SKU, location, quantity, dates, price, and POB details.
Support imported lines, manual adjustment with approval, negative credit lines, back-billing lines, and change comments.
Delivery schedule grid
Track actual and forecast quantities or hours by month.
Dynamic month range, lock/unlock flags, actual/forecast flags, HCDD updates, and prior-period protections.
Accounting inputs
Capture technical accounting assumptions.
SSP tables, material-right inputs, terminability, credit/warrant treatment, discount allocation method, and policy references.
Calculation engine
Run deterministic allocation and revenue-recognition logic.
Compute hours, prices, discounts, SSP guide, allocations, revenue schedules, true-ups, and deltas.
Review and controls
Evidence review before close or modification approval.
Validation checklist, variance review, sign-off, audit log, approvals, and export package.
Reporting and export
Produce support for accounting entries and audit.
Main allocation detail, entry summary, JE lines, tie-outs, review log, comparison views, and Excel/PDF exports.

 
7. Functional Requirements
ID
Requirement
Priority
Notes
FR-01
Provide role-based access for preparers, reviewers, approvers, administrators, and read-only audit users.
High
Protect sensitive revenue data and approval actions.
FR-02
Allow users to search and select a customer, arrangement, contract, amendment, and order form from SFDC or uploaded source data.
High
Supports the core selection question: which contracts should be combined.
FR-03
Create and save named scenarios, including combined and not-combined versions, with effective dates and status.
High
Scenarios should be reusable across monthly closes and modifications.
FR-04
Import or manually enter line-level contract data: order number, SKU, location, quantity, contractual start/end dates, price per service hour, sales price, and POB flag.
High
Mirrors main workbook columns A-W.
FR-05
Maintain monthly delivery quantity or service-hour grids with actual/forecast status and locked/unlocked status by month.
High
Workbook currently covers 111 months from 2025-10-31 through 2034-12-31; application range must be dynamic.
FR-06
Calculate monthly service hours based on line quantity, 24 hours per day, and date overlap for each monthly period.
High
Used for delivery schedules and revenue recognition.
FR-07
Calculate contractual days, contractual service hours, contractual total price, latest estimated service hours, and latest estimated total price.
High
Foundational allocation inputs.
FR-08
Support HCDD updates and modifications that shift delivery/start dates and recompute downstream allocations.
High
Key driver of repeated re-spins.
FR-09
Support terminable tranches and enforce associated RPO and contract-combination treatment based on accounting configuration.
High
OAI use case includes tranches pulled out of RPO/combination analysis due to termination rights.
FR-10
Capture TCV reductions, cash credits, stock warrants/equity consideration, late-delivery credits, and other non-standard consideration.
High
OAI example includes multiple reduction mechanics and negative/credit lines.
FR-11
Maintain SSP tables by quarter, product/SKU, customer segment, and range; identify SSP period used for each line.
High
Current source includes Q1'26 and Q4'25 SSP tables.
FR-12
Calculate relative selling price allocation, discount allocation, net sales price, applicable SSP guide per hour, total SSP guide, revenue allocation, reallocation amount, and rev-rec rate per service hour.
High
Mirrors current calculation fields EF-FD.
FR-13
Provide material-right assessment and lookback functionality with assumptions for option pricing, cost estimates, uplift, likelihood of exercise, SSP comparison, and probability-adjusted discount.
High
Current workbook has Material Right Assmt and Material Right Lookback tabs.
FR-14
Calculate revenue recognition by month using allocation results and actual/forecast service quantities.
High
Current workbook includes a separate 111-month revenue recognition grid.
FR-15
Calculate revenue true-ups only for prior months marked unlocked and exclude locked periods from true-up calculations.
High
Explicit current-state control requirement.
FR-16
Prevent unintended prior-period changes by comparing current scenario values against the prior approved scenario for locked months.
High
Automates final accuracy check.
FR-17
Generate entry summary and JE support with debit/credit lines, accounts, subsidiary, customer/name, memo, department, class, and posting reference fields.
High
Current Entry Summary includes JE support output.
FR-18
Generate order-form tie-outs with variance status and tickmark explanations.
Medium
Current OF 5 tie-out flags variances and tickmark references.
FR-19
Provide validation checks for missing required fields, date inconsistencies, negative values, unbalanced entries, tie-out variances, and formula/calculation errors.
High
Critical control requirement.
FR-20
Require user-provided change descriptions for monthly and modification-driven changes.
High
Current workbook explicitly requires descriptions in the change column.
FR-21
Maintain an audit log of imports, edits, overrides, calculation runs, review comments, approvals, and exports.
High
Needed for auditability and close control.
FR-22
Support export to Excel and PDF for allocation detail, review package, JE support, and audit tie-outs.
Medium
Users and auditors will still need portable evidence packages.
FR-23
Support override workflows with reason code, preparer, reviewer, timestamp, and optional attachment.
Medium
Needed when accounting judgments or source-system gaps require manual inputs.
FR-24
Allow configurable materiality/variance thresholds by customer, scenario, or accounting policy.
Medium
Supports review workflow without hardcoding thresholds.
FR-25
Provide a controlled re-run workflow after contract modification, with before/after comparison of allocation and revenue impacts.
High
Directly addresses the need to re-spin analyses after changes.

 
8. Business Rules
ID
Rule
Description
BR-01
Contract selection
A scenario must explicitly identify the customer, arrangement, contracts, amendments, and order forms included in the analysis. The application should not infer the accounting conclusion without user confirmation.
BR-02
Locked periods
Prior periods that are locked must retain prior approved revenue and delivery values unless reopened through an approved modification workflow.
BR-03
Unlocked periods
Prior unlocked periods can be recalculated and included in true-up calculations. The system must identify the true-up amount and posting reference.
BR-04
Actual versus forecast
Months through the current close period should be marked actual when actual delivery/service quantities are available. Future months should remain forecast until updated.
BR-05
Service hours
For each line and month, service hours equal quantity multiplied by 24 and by the number of service days overlapping the line start/end dates and monthly period.
BR-06
Termination rights
Lines or tranches marked terminable must receive accounting treatment configured by Technical Accounting, including potential exclusion from RPO and/or contract-combination calculations.
BR-07
SSP application
The system must use the SSP period configured for the scenario and should store midpoint, low-end, high-end, and applicable guide value used for each line.
BR-08
Discount allocation
Cash credits, warrants/equity consideration, TCV reductions, and other discounts must be allocated according to the configured accounting method, with non-POB treatment supported.
BR-09
Material right
Material-right conclusions must be calculated from configured assumptions and retained as part of the scenario evidence, but final judgment requires user approval.
BR-10
Tie-outs
Order-form and JE outputs must tie to source contract values or explain variances before scenario approval.

 
9. Data Requirements
Source
Example Fields
Requirement
SFDC / contract data
Customer, account, contract ID, order form, amendment, SKU, quantity, location, start date, end date, price, status, credits, commercial terms.
Prefer API integration; allow controlled upload for MVP if APIs are delayed.
Technical Accounting memo/input
POB determination, contract-combination decision, material-right conclusions, SSP period, terminability, credit/warrant treatment, modification effective date.
Must be captured as user-approved assumptions with evidence links or attachments.
SSP repository
Quarter, product/SKU, midpoint, low-end, high-end, customer segment, effective dates.
Must be versioned so prior approved scenarios keep the rates used at approval time.
Delivery / operations / FP&A
High-confidence delivery dates, actual delivered quantities/hours, forecast quantities/hours, cost estimates, interest rates where applicable.
Used for monthly updates and material-right calculations.
ERP / revenue close
Prior-month actual revenue, JE numbers, account, department, class, subsidiary, posting status.
Required for true-up and posting evidence; integration can be phased.
Manual attachments
Contracts, screenshots, memos, approval evidence, tie-out support.
Must support upload and association to scenario, line, validation, or review step.

 
10. Controls, Auditability, and Governance
Every scenario should have a unique ID, version, owner, status, source data snapshot, calculation timestamp, and approval history.
The application should separate imported data, accounting judgments, manual overrides, calculated outputs, and exported evidence.
Calculation logic should be deterministic and covered by test cases. AI-assisted coding may be used to build the application, but production calculations should not depend on opaque AI outputs.
Overrides should require reason, preparer, approver where applicable, and timestamp.
Scenario approval should be blocked by unresolved critical validations such as missing required fields, tie-out variances above threshold, or unbalanced JE output.
The application should preserve prior approved scenarios for audit and comparison rather than overwriting historical assumptions.
Access should be role-based and aligned with finance close controls, confidentiality requirements, and change-management policy.
11. Non-Functional Requirements
Category
Requirement
Usability
The web application should support accounting users who understand the workbook logic but should not require them to maintain formulas or manually manage hundreds of month columns.
Performance
A scenario calculation should complete quickly enough for iterative review; target performance should be defined during design based on expected line counts and customer portfolios.
Reliability
Calculations should be repeatable, testable, and produce the same output for the same scenario version and source data.
Security
Application must protect sensitive customer, pricing, revenue, contract, and accounting data with role-based access and appropriate audit logging.
Scalability
Design should support additional strategic customers, order forms, SKUs, and future periods without workbook redesign.
Maintainability
Accounting rules, SSP tables, variance thresholds, and export templates should be configurable by authorized administrators where practical.
Explainability
Outputs should include calculation lineage and source references sufficient for preparer/reviewer/auditor review.
Availability
Availability targets should align with monthly and quarterly close calendars, including support expectations during close windows.

 
12. Outputs and Reporting
Scenario summary showing customer, contracts/order forms included, status, key assumptions, and calculation run date.
Main allocation detail with line-level contract data, latest estimates, locked/unlocked quantities, SSP application, revenue allocation, and reallocation amount.
Combined versus not-combined comparison view.
Monthly revenue recognition schedule by line and scenario.
Revenue true-up report identifying prior unlocked periods, amount posted, and JE reference.
Entry summary and JE detail with accounting dimensions and memos.
Order-form tie-out reports with variance status and tickmark explanations.
Material-right assessment and lookback output.
Review checklist, approvals, comments, and unresolved validation report
Audit package export in Excel and/or PDF.
13. Acceptance Criteria
The application can reproduce the workbook's core allocation, revenue recognition, true-up, and entry-summary outputs within an approved tolerance.
A user can create a scenario, select contracts/order forms, add or remove lines, update delivery dates, run calculations, and compare results against a prior scenario.
The system enforces locked-period protections and calculates true-ups only for prior unlocked periods.
The system supports terminable tranches, material-right inputs, cash credits, stock warrants/equity consideration, late-delivery credits, and non-POB lines.
The system generates a complete review package with allocation detail, JE support, tie-outs, validations, review comments, approval status, and export files.
All manual overrides are auditable and cannot be silently applied.
Critical validations prevent scenario approval until resolved or explicitly approved by an authorized reviewer.
The application supports at least OpenAI and one additional strategic customer pilot scenario before production rollout.
14. Assumptions and Dependencies
·        SFDC contains or can be extended to contain the contract/order-form line data required for the automation.
·        Technical Accounting will continue to own accounting conclusions, material-right judgments, SSP period selection, and contract-combination conclusions.
·        IT will confirm whether the MVP integrates directly with SFDC/ERP or starts with controlled uploads and exports.
·        Commercial Accounting will provide test scenarios, expected outputs, and tolerance thresholds from current workbooks.
·        Data privacy, customer confidentiality, SOX/control requirements, and change-management requirements will be addressed during design.
·        AI-assisted development may be used by IT, but the deployed engine must use approved, deterministic calculations with tests and approvals.

Team
Representative
Status
Date
Commercial Accounting
Jeremy Pearlman
 Approved
6/3/26
Commercial Accounting
Phillip Bussjaeger
 Not started


Commercial Accounting
Mark Fischer
 Not started


Business PM/PMO
Alan Lee (Optional)
 Not started


IT PM Owner
Taj Singh (Optional)
 Not started


IT AI Manager
Vinay Yella
 Not started








15. Open Questions
ID
Question
Owner
OQ-01
Which SFDC objects and fields contain the required contract, order-form, amendment, SKU, pricing, credit, and status data?
IT / Revenue Ops
OQ-02
Should the MVP use direct SFDC integration, controlled file upload, or both?
IT / Business
OQ-03
Which ERP/revenue systems must receive or validate JE outputs, and is writeback in scope?
IT / Accounting
OQ-04
What are the approved materiality thresholds for tie-out variances, revenue deltas, and validation warnings?
Technical Accounting
OQ-05
Who can approve contract-combination conclusions, material-right assumptions, terminability flags, and manual overrides?
Technical Accounting / Controls
OQ-06
What exact rules apply when a termination right pulls a tranche out of RPO or out of a contract-combination scenario?
Technical Accounting
OQ-07
Which customers and arrangements beyond OpenAI and Meta must be supported in the first release?
Business Sponsor
OQ-08
What export formats, naming conventions, and evidence package structure do auditors and close reviewers require?
Accounting / Audit
OQ-09
What retention period and access restrictions apply to scenario versions and attached contract support?
Legal / IT Security
OQ-10
What test cases from historical closes should be used for user acceptance testing?
Business / IT

 
Appendix A: Workbook-Derived Observations
Workbook Area
Observed Content
BRD Relevance
Steps to Updating
Manual checklist for row copying, modification updates, monthly HCDD/actual updates, true-up, and accuracy checks.
Source for workflow, controls, and validation requirements.
New Main
Main scenario model covering contract inputs, subsequent updates, 111-month actual/forecast quantity grid, lock flags, SSP allocation fields, and 111-month revenue recognition grid.
Core target for calculation engine.
Old Main
Prior scenario / historical model used for comparison and locked-period review.
Source for prior-version comparison requirements.
Main if not combined
Alternate view for not-combined analysis and reallocation impact.
Source for scenario comparison requirements.
03.31.26 Entry Summary
Revenue reduction, ST/LT asset support, JE lines, accounts, memos, departments, classes, and posting references.
Source for output/export requirements.
OF 5 Tie-Out
Tie-out of order form amounts to main tab, variance status, and tickmark references.
Source for tie-out and variance requirements.
Q1'26 SSP
SSP rates for products such as H100, H200, GB200, B200, GB300, STOR-Cold-01, CPU-GP2-01, and CPU-GP2-02.
Source for configurable SSP repository requirements.
Material Right Assmt
Material-right calculation with discounts, gross TCV, cost estimates, uplift, SSP comparison, total service hours, and discount value.
Source for material-right module requirements.
Material Right Lookback
Historical/lookback material-right assessment and probability-adjusted discount support.
Source for versioned assumption and lookback requirements.
MF Review
Review checklist for delivery-date updates, SSP validation, material-right updates, line accuracy, formula accuracy, and posting confirmation.
Source for review workflow and sign-off requirements.

 
Appendix B: Suggested Build Phases
Phase
Focus
Candidate Deliverables
Phase 0
Discovery and data mapping
SFDC field map, workbook logic inventory, control requirements, test case selection, UX wireframes.
Phase 1
MVP scenario engine
Customer/contract selection, line-item editor, monthly grid, core calculations, scenario versioning, basic exports.
Phase 2
Close workflow and controls
Locked/unlocked handling, actual/forecast updates, true-up report, review checklist, validations, audit log.
Phase 3
Advanced accounting modules
Material-right assessment/lookback, terminable tranche logic, cash credit/warrant treatment, combined/not-combined comparison.
Phase 4
Integrations and scale
SFDC API integration, ERP export/writeback as approved, customer portfolio dashboard, reusable templates for Meta and other key customers.

