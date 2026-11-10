# Prompt library for the private equity deal cycle

155 standalone prompts, organised by use case across seven stages of the deal cycle. Each one can be pasted into Claude with the documents it names. None needs the workspace skills loaded.

## How to use it

1. **Paste the standing rules first.** Copy the block in [`00-standing-rules.md`](00-standing-rules.md) into the conversation, or save it as the instructions of a Claude Project. The prompts stay short because they rely on it: citations, gap labels, keeping management claims apart from evidence, and the Moody's checks.
2. **Find the use case** in the index below and open its file.
3. **Attach the inputs** listed under the prompt, fill the bracketed fields, and run it.
4. **Use the follow-ups** to drill into the answer. The **Adjust for** line says how to change the prompt for carve-outs, take-privates, growth equity, minority stakes, turnarounds, specialist or thematic funds.

**Moody's connector.** Every prompt works without it. Where Moody's adds evidence, the prompt's **Moody's** field names the exact tools to use. Those tools and their known behaviours were tested on a live connection on 10 November 2026: see [`TEST-REPORT.md`](TEST-REPORT.md). The most important lesson for sponsor-owned companies: the operating company, the top holding company and the rated issuer (often the Bidco) are different entities with very different credit pictures. Find the rated issuer first and quote its assigned rating; standing rule 11 says how.

**These are drafts for a professional to review.** Nothing here is investment, legal, tax or accounting advice.

## Stages

| File | Stage | What it covers | Prompts |
|---|---|---|---|
| [`01-sourcing-and-origination.md`](01-sourcing-and-origination.md) | Sourcing and origination | Find and qualify targets before any information memorandum arrives. | 20 |
| [`02-screening.md`](02-screening.md) | Screening | Decide in hours whether a teaser or memorandum deserves diligence spend. | 15 |
| [`03-due-diligence.md`](03-due-diligence.md) | Due diligence | Financial, commercial and management diligence, plus legal, technology, tax and external evidence. | 41 |
| [`04-valuation-and-ic.md`](04-valuation-and-ic.md) | Valuation and investment committee | Returns, financing context and the committee paper. | 12 |
| [`05-portfolio-monitoring.md`](05-portfolio-monitoring.md) | Portfolio monitoring | Monthly packs, covenants, early warning, marks and investor reporting. | 21 |
| [`06-value-creation.md`](06-value-creation.md) | Value creation | The plan, lever diagnostics, go-to-market expansion, talent and add-ons. | 36 |
| [`07-exit.md`](07-exit.md) | Exit | Value attribution, readiness, buyers and the equity story. | 10 |

## Index of use cases

### Sourcing and origination

| ID | Use case | Uses Moody's |
|---|---|---|
| [SO-01](01-sourcing-and-origination.md#so-01-thesis-and-buy-box) | Thesis and buy box | Yes |
| [SO-02](01-sourcing-and-origination.md#so-02-theme-exposure-test) | Theme exposure test | Yes |
| [SO-03](01-sourcing-and-origination.md#so-03-value-chain-map) | Value-chain map | Yes |
| [SO-04](01-sourcing-and-origination.md#so-04-universe-build-and-funnel) | Universe build and funnel | Yes |
| [SO-05](01-sourcing-and-origination.md#so-05-de-duplication-and-entity-resolution) | De-duplication and entity resolution | Yes |
| [SO-06](01-sourcing-and-origination.md#so-06-ownership-and-succession-signals-for-a-list) | Ownership and succession signals for a list | Yes |
| [SO-07](01-sourcing-and-origination.md#so-07-sponsor-owned-companies-and-estimated-hold-period) | Sponsor-owned companies and estimated hold period | Yes |
| [SO-08](01-sourcing-and-origination.md#so-08-hiring-and-expansion-signals-from-public-sources) | Hiring and expansion signals from public sources | Yes |
| [SO-09](01-sourcing-and-origination.md#so-09-news-signals) | News signals | Yes |
| [SO-10](01-sourcing-and-origination.md#so-10-add-on-pipeline-around-a-platform) | Add-on pipeline around a platform | Yes |
| [SO-11](01-sourcing-and-origination.md#so-11-carve-out-candidates) | Carve-out candidates | Yes |
| [SO-12](01-sourcing-and-origination.md#so-12-take-private-screen-from-public-filings) | Take-private screen from public filings | Yes |
| [SO-13](01-sourcing-and-origination.md#so-13-turnaround-and-credit-stress-watchlist) | Turnaround and credit-stress watchlist | Yes |
| [SO-14](01-sourcing-and-origination.md#so-14-one-company-cited-brief) | One-company cited brief | Yes |
| [SO-15](01-sourcing-and-origination.md#so-15-score-a-list-against-the-mandate) | Score a list against the mandate | Yes |
| [SO-16](01-sourcing-and-origination.md#so-16-challenge-the-ranking) | Challenge the ranking | Yes |
| [SO-17](01-sourcing-and-origination.md#so-17-relationship-check) | Relationship check | Yes |
| [SO-18](01-sourcing-and-origination.md#so-18-owner-outreach-draft) | Owner outreach draft | No |
| [SO-19](01-sourcing-and-origination.md#so-19-intermediary-coverage-plan) | Intermediary coverage plan | Yes |
| [SO-20](01-sourcing-and-origination.md#so-20-weekly-sourcing-digest) | Weekly sourcing digest | Yes |

### Screening

| ID | Use case | Uses Moody's |
|---|---|---|
| [SC-01](02-screening.md#sc-01-teaser-read) | Teaser read | No |
| [SC-02](02-screening.md#sc-02-information-memorandum-teardown) | Information memorandum teardown | No |
| [SC-03](02-screening.md#sc-03-claims-tracker) | Claims tracker | Yes |
| [SC-04](02-screening.md#sc-04-first-pass-earnings-adjustments) | First-pass earnings adjustments | Yes |
| [SC-05](02-screening.md#sc-05-concentration-quick-check) | Concentration quick check | Yes |
| [SC-06](02-screening.md#sc-06-red-flag-scan) | Red-flag scan | Yes |
| [SC-07](02-screening.md#sc-07-mandate-fit-and-score) | Mandate fit and score | Yes |
| [SC-08](02-screening.md#sc-08-quick-external-check-with-moodys) | Quick external check with Moody's | Yes |
| [SC-09](02-screening.md#sc-09-ai-exposure-at-first-look) | AI exposure at first look | Yes |
| [SC-10](02-screening.md#sc-10-valuation-sanity-check) | Valuation sanity check | Yes |
| [SC-11](02-screening.md#sc-11-quick-returns-sketch) | Quick returns sketch | Yes |
| [SC-12](02-screening.md#sc-12-go--no-go-memo) | Go / no-go memo | Yes |
| [SC-13](02-screening.md#sc-13-first-call-questions) | First-call questions | No |
| [SC-14](02-screening.md#sc-14-need-more-request) | NEED MORE request | No |
| [SC-15](02-screening.md#sc-15-screen-a-batch-of-teasers) | Screen a batch of teasers | No |

### Due diligence

| ID | Use case | Uses Moody's |
|---|---|---|
| [DD-01](03-due-diligence.md#dd-01-index-the-data-room) | Index the data room | No |
| [DD-02](03-due-diligence.md#dd-02-build-the-diligence-request-list) | Build the diligence request list | No |
| [DD-03](03-due-diligence.md#dd-03-plan-the-reading-order) | Plan the reading order | No |
| [DD-04](03-due-diligence.md#dd-04-red-flag-scan) | Red-flag scan | Yes |
| [DD-05](03-due-diligence.md#dd-05-tie-out-the-numbers) | Tie out the numbers | Yes |
| [DD-06](03-due-diligence.md#dd-06-quality-of-earnings) | Quality of earnings | Yes |
| [DD-07](03-due-diligence.md#dd-07-revenue-quality) | Revenue quality | Yes |
| [DD-08](03-due-diligence.md#dd-08-net-working-capital-and-the-peg) | Net working capital and the peg | Yes |
| [DD-09](03-due-diligence.md#dd-09-net-debt-and-debt-like-items) | Net debt and debt-like items | Yes |
| [DD-10](03-due-diligence.md#dd-10-cash-conversion-and-capital-expenditure) | Cash conversion and capital expenditure | Yes |
| [DD-11](03-due-diligence.md#dd-11-test-the-forecast) | Test the forecast | Yes |
| [DD-12](03-due-diligence.md#dd-12-financial-red-flags) | Financial red flags | Yes |
| [DD-13](03-due-diligence.md#dd-13-financial-diligence-summary) | Financial diligence summary | Yes |
| [DD-14](03-due-diligence.md#dd-14-commercial-hypotheses) | Commercial hypotheses | No |
| [DD-15](03-due-diligence.md#dd-15-size-the-served-market) | Size the served market | Yes |
| [DD-16](03-due-diligence.md#dd-16-customer-concentration-and-retention) | Customer concentration and retention | Yes |
| [DD-17](03-due-diligence.md#dd-17-competition-and-pricing-power) | Competition and pricing power | Yes |
| [DD-18](03-due-diligence.md#dd-18-price-volume-and-mix-bridge) | Price, volume and mix bridge | No |
| [DD-19](03-due-diligence.md#dd-19-test-the-growth-plan) | Test the growth plan | Yes |
| [DD-20](03-due-diligence.md#dd-20-customer-reference-programme) | Customer reference programme | No |
| [DD-21](03-due-diligence.md#dd-21-commercial-diligence-summary) | Commercial diligence summary | Yes |
| [DD-22](03-due-diligence.md#dd-22-map-the-roles-from-the-record) | Map the roles from the record | Yes |
| [DD-23](03-due-diligence.md#dd-23-verify-the-track-record) | Verify the track record | Yes |
| [DD-24](03-due-diligence.md#dd-24-key-person-and-owner-dependence) | Key-person and owner dependence | No |
| [DD-25](03-due-diligence.md#dd-25-organisation-and-bench-against-the-plan) | Organisation and bench against the plan | No |
| [DD-26](03-due-diligence.md#dd-26-incentives-and-alignment) | Incentives and alignment | No |
| [DD-27](03-due-diligence.md#dd-27-integrity-facts) | Integrity facts | Yes |
| [DD-28](03-due-diligence.md#dd-28-contract-review-grid) | Contract review grid | No |
| [DD-29](03-due-diligence.md#dd-29-covenants-that-could-bind-the-buyer) | Covenants that could bind the buyer | No |
| [DD-30](03-due-diligence.md#dd-30-debt-security-and-litigation) | Debt, security and litigation | Yes |
| [DD-31](03-due-diligence.md#dd-31-intellectual-property-chain-of-title) | Intellectual property chain of title | No |
| [DD-32](03-due-diligence.md#dd-32-technology-and-cyber-review) | Technology and cyber review | No |
| [DD-33](03-due-diligence.md#dd-33-exposure-to-ai) | Exposure to AI | Yes |
| [DD-34](03-due-diligence.md#dd-34-tax-and-regulatory-issues-for-advisers) | Tax and regulatory issues for advisers | Yes |
| [DD-35](03-due-diligence.md#dd-35-environmental-and-social-review) | Environmental and social review | Yes |
| [DD-36](03-due-diligence.md#dd-36-external-corroboration-with-moodys) | External corroboration with Moody's | Yes |
| [DD-37](03-due-diligence.md#dd-37-route-findings-between-workstreams) | Route findings between workstreams | Yes |
| [DD-38](03-due-diligence.md#dd-38-consolidated-findings-register) | Consolidated findings register | Yes |
| [DD-39](03-due-diligence.md#dd-39-prepare-a-management-session) | Prepare a management session | No |
| [DD-40](03-due-diligence.md#dd-40-synthesise-expert-and-customer-calls) | Synthesise expert and customer calls | No |
| [DD-41](03-due-diligence.md#dd-41-track-verbal-claims) | Track verbal claims | No |

### Valuation and investment committee

| ID | Use case | Uses Moody's |
|---|---|---|
| [IC-01](04-valuation-and-ic.md#ic-01-sources-and-uses) | Sources and uses | No |
| [IC-02](04-valuation-and-ic.md#ic-02-returns-model-specification) | Returns model specification | Yes |
| [IC-03](04-valuation-and-ic.md#ic-03-returns-attribution-at-entry) | Returns attribution at entry | No |
| [IC-04](04-valuation-and-ic.md#ic-04-sensitivity-grids) | Sensitivity grids | No |
| [IC-05](04-valuation-and-ic.md#ic-05-covenant-and-liquidity-stress) | Covenant and liquidity stress | No |
| [IC-06](04-valuation-and-ic.md#ic-06-financing-context-with-moodys) | Financing context with Moody's | Yes |
| [IC-07](04-valuation-and-ic.md#ic-07-valuation-cross-check) | Valuation cross-check | Yes |
| [IC-08](04-valuation-and-ic.md#ic-08-what-must-be-true) | What must be true | No |
| [IC-09](04-valuation-and-ic.md#ic-09-investment-committee-memo-draft) | Investment committee memo draft | Yes |
| [IC-10](04-valuation-and-ic.md#ic-10-pre-mortem) | Pre-mortem | Yes |
| [IC-11](04-valuation-and-ic.md#ic-11-committee-question-rehearsal) | Committee question rehearsal | No |
| [IC-12](04-valuation-and-ic.md#ic-12-conditions-tracker-for-signing-and-closing) | Conditions tracker for signing and closing | No |

### Portfolio monitoring

| ID | Use case | Uses Moody's |
|---|---|---|
| [PM-01](05-portfolio-monitoring.md#pm-01-normalise-the-pack-to-the-fund-template) | Normalise the pack to the fund template | No |
| [PM-02](05-portfolio-monitoring.md#pm-02-bad-data-scan) | Bad-data scan | No |
| [PM-03](05-portfolio-monitoring.md#pm-03-kpi-extraction-with-locators) | KPI extraction with locators | No |
| [PM-04](05-portfolio-monitoring.md#pm-04-variance-against-budget-and-the-underwriting-case) | Variance against budget and the underwriting case | No |
| [PM-05](05-portfolio-monitoring.md#pm-05-the-three-worst-variances) | The three worst variances | Yes |
| [PM-06](05-portfolio-monitoring.md#pm-06-timing-or-permanent) | Timing or permanent | No |
| [PM-07](05-portfolio-monitoring.md#pm-07-covenant-test-from-the-executed-agreement) | Covenant test from the executed agreement | Yes |
| [PM-08](05-portfolio-monitoring.md#pm-08-reporting-and-cure-deadline-calendar) | Reporting and cure deadline calendar | No |
| [PM-09](05-portfolio-monitoring.md#pm-09-13-week-cash-flow-review) | 13-week cash flow review | No |
| [PM-10](05-portfolio-monitoring.md#pm-10-maturity-wall-and-refinancing-readiness) | Maturity wall and refinancing readiness | Yes |
| [PM-11](05-portfolio-monitoring.md#pm-11-portfolio-sweep) | Portfolio sweep | Yes |
| [PM-12](05-portfolio-monitoring.md#pm-12-counterparty-exposure-check) | Counterparty exposure check | Yes |
| [PM-13](05-portfolio-monitoring.md#pm-13-materiality-ranking) | Materiality ranking | No |
| [PM-14](05-portfolio-monitoring.md#pm-14-thesis-drift) | Thesis drift | Yes |
| [PM-15](05-portfolio-monitoring.md#pm-15-board-pack-review-and-questions) | Board pack review and questions | No |
| [PM-16](05-portfolio-monitoring.md#pm-16-quarterly-mark-support) | Quarterly mark support | Yes |
| [PM-17](05-portfolio-monitoring.md#pm-17-argue-the-write-down-and-the-write-up) | Argue the write-down and the write-up | Yes |
| [PM-18](05-portfolio-monitoring.md#pm-18-quarterly-report-sections) | Quarterly report sections | Yes |
| [PM-19](05-portfolio-monitoring.md#pm-19-credit-appendix) | Credit appendix | Yes |
| [PM-20](05-portfolio-monitoring.md#pm-20-due-diligence-questionnaire-answers) | Due-diligence questionnaire answers | No |
| [PM-21](05-portfolio-monitoring.md#pm-21-investor-question-response) | Investor question response | No |

### Value creation

| ID | Use case | Uses Moody's |
|---|---|---|
| [VC-01](06-value-creation.md#vc-01-map-the-underwriting-case-to-named-initiatives) | Map the underwriting case to named initiatives | Yes |
| [VC-02](06-value-creation.md#vc-02-size-each-initiative) | Size each initiative | Yes |
| [VC-03](06-value-creation.md#vc-03-write-the-100-day-plan) | Write the 100-day plan | No |
| [VC-04](06-value-creation.md#vc-04-day-one-list) | Day-one list | No |
| [VC-05](06-value-creation.md#vc-05-test-the-plan-against-management-time-and-covenant-headroom) | Test the plan against management time and covenant headroom | Yes |
| [VC-06](06-value-creation.md#vc-06-initiative-tracker-and-benefit-ledger) | Initiative tracker and benefit ledger | No |
| [VC-07](06-value-creation.md#vc-07-data-quality-gate) | Data-quality gate | No |
| [VC-08](06-value-creation.md#vc-08-price-volume-mix-bridge) | Price-volume-mix bridge | No |
| [VC-09](06-value-creation.md#vc-09-discount-waterfall-to-pocket-price) | Discount waterfall to pocket price | No |
| [VC-10](06-value-creation.md#vc-10-price-dispersion-for-like-customers) | Price dispersion for like customers | No |
| [VC-11](06-value-creation.md#vc-11-test-pricing-headroom) | Test pricing headroom | No |
| [VC-12](06-value-creation.md#vc-12-customer-retention-by-cohort) | Customer retention by cohort | No |
| [VC-13](06-value-creation.md#vc-13-procurement-spend-cube-and-tail-spend) | Procurement spend cube and tail spend | Yes |
| [VC-14](06-value-creation.md#vc-14-supplier-concentration-and-credit-check) | Supplier concentration and credit check | Yes |
| [VC-15](06-value-creation.md#vc-15-working-capital-days-and-cash-release) | Working-capital days and cash release | Yes |
| [VC-16](06-value-creation.md#vc-16-cost-to-serve-and-customer-profitability) | Cost-to-serve and customer profitability | No |
| [VC-17](06-value-creation.md#vc-17-peer-benchmark-gap) | Peer benchmark gap | Yes |
| [VC-18](06-value-creation.md#vc-18-ai-opportunities) | AI opportunities | Yes |
| [VC-19](06-value-creation.md#vc-19-commercial-baseline-and-funnel) | Commercial baseline and funnel | No |
| [VC-20](06-value-creation.md#vc-20-white-space-grid) | White-space grid | Yes |
| [VC-21](06-value-creation.md#vc-21-size-the-expansion-options) | Size the expansion options | Yes |
| [VC-22](06-value-creation.md#vc-22-partner-and-distributor-screen-in-a-new-market) | Partner and distributor screen in a new market | Yes |
| [VC-23](06-value-creation.md#vc-23-pricing-and-packaging-for-a-new-segment) | Pricing and packaging for a new segment | No |
| [VC-24](06-value-creation.md#vc-24-cross-sell-into-the-existing-base) | Cross-sell into the existing base | No |
| [VC-25](06-value-creation.md#vc-25-sales-capacity-planning) | Sales capacity planning | Yes |
| [VC-26](06-value-creation.md#vc-26-pilot-design-with-a-stop-rule) | Pilot design with a stop rule | No |
| [VC-27](06-value-creation.md#vc-27-critical-roles-the-plan-needs) | Critical roles the plan needs | Yes |
| [VC-28](06-value-creation.md#vc-28-organisation-spans-layers-and-function-cost) | Organisation spans, layers and function cost | Yes |
| [VC-29](06-value-creation.md#vc-29-attrition-analysis) | Attrition analysis | No |
| [VC-30](06-value-creation.md#vc-30-hiring-plan-and-search-briefs) | Hiring plan and search briefs | No |
| [VC-31](06-value-creation.md#vc-31-leadership-evidence-pack-for-human-review) | Leadership evidence pack for human review | Yes |
| [VC-32](06-value-creation.md#vc-32-management-incentive-plan-modelling-at-2x-and-3x-exits) | Management incentive plan modelling at 2x and 3x exits | Yes |
| [VC-33](06-value-creation.md#vc-33-succession-and-retention-for-critical-roles) | Succession and retention for critical roles | No |
| [VC-34](06-value-creation.md#vc-34-add-on-fit-and-actionability) | Add-on fit and actionability | Yes |
| [VC-35](06-value-creation.md#vc-35-integration-plan) | Integration plan | Yes |
| [VC-36](06-value-creation.md#vc-36-synergy-tracker-and-pro-forma-figures) | Synergy tracker and pro forma figures | Yes |

### Exit

| ID | Use case | Uses Moody's |
|---|---|---|
| [EX-01](07-exit.md#ex-01-value-attribution-bridge) | Value attribution bridge | No |
| [EX-02](07-exit.md#ex-02-organic-versus-acquired-growth) | Organic versus acquired growth | Yes |
| [EX-03](07-exit.md#ex-03-exit-readiness-scorecard) | Exit readiness scorecard | Yes |
| [EX-04](07-exit.md#ex-04-red-flag-scan-of-our-own-company) | Red-flag scan of our own company | Yes |
| [EX-05](07-exit.md#ex-05-vendor-due-diligence-scope) | Vendor due diligence scope | No |
| [EX-06](07-exit.md#ex-06-buyer-universe-mapping) | Buyer universe mapping | Yes |
| [EX-07](07-exit.md#ex-07-equity-story-with-verifiable-claims) | Equity story with verifiable claims | Yes |
| [EX-08](07-exit.md#ex-08-data-room-preparation) | Data-room preparation | No |
| [EX-09](07-exit.md#ex-09-exit-timing-and-route-considerations) | Exit timing and route considerations | Yes |
| [EX-10](07-exit.md#ex-10-questions-buyers-will-ask-with-evidence-backed-answers) | Questions buyers will ask, with evidence-backed answers | Yes |

## Related files

- [`../catalogue/SOURCE-CATALOGUE.md`](../catalogue/SOURCE-CATALOGUE.md): the open-source skills and prompt libraries these prompts learned from, with licences and install commands.
- [`../shared/moodys-evidence-layer.md`](../shared/moodys-evidence-layer.md): the verified Moody's tool map and where the data stops.
- The stage workspaces (`Diligence Agent/`, `Portfolio Monitoring Agent/`, `Value Creation Agent/` and the sourcing folders) run the same work as multi-agent skills with checks and file outputs. Use the prompts for one-off questions and the skills for repeatable workflows.
