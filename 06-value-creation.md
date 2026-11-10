# Value creation prompts

Use these once the fund owns the company, from the week after closing until exit preparation starts. Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach the material named under **Inputs**, fill the bracketed fields and run it. Thresholds in brackets are the fund's own settings; replace them before running.

**How each entry is laid out**

- **Use case:** the job, in one line.
- **Inputs:** what to attach or connect.
- **Prompt:** the text to paste.
- **Output:** what should come back.
- **Moody's:** which connector tools to use and what they add. Every prompt also works without the connector.
- **Follow-ups:** prompts for drilling into the answer.
- **Adjust for:** how to change it for other deal types.

**Use cases in this file**

| Area | IDs |
|---|---|
| Plan | VC-01 to VC-06 |
| Lever diagnostics | VC-07 to VC-18 |
| Go-to-market expansion | VC-19 to VC-26 |
| Talent and organisation | VC-27 to VC-33 |
| Buy-and-build | VC-34 to VC-36 |

---

## Plan

### VC-01 Map the underwriting case to named initiatives

**Use case:** Turn the case the investment committee approved into a list of initiatives, each with an owner, and find the assumptions nobody owns.

**Inputs:** The underwriting model, the investment committee paper, the fund-accepted EBITDA definition (earnings before interest, tax, depreciation and amortisation), the diligence findings register.

**Prompt:**
```
For [Company], build the underwriting bridge and map it to initiatives.

1. State which EBITDA definition the model uses and reconcile it to the fund-accepted definition first; reported, company-adjusted and Moody's-adjusted EBITDA can differ widely for the same year. From the underwriting model, take entry and exit revenue, EBITDA margin, EBITDA, valuation multiple and net debt. Split the underwritten value created into: revenue effect = (exit revenue minus entry revenue) times entry margin times entry multiple; margin effect = exit revenue times (exit margin minus entry margin) times entry multiple; multiple effect = (exit multiple minus entry multiple) times exit EBITDA; net debt effect = entry net debt minus exit net debt, plus dividends, minus equity injected. Show that the four effects add up to the underwritten value created.
2. List every assumption in the model that moves revenue, margin or cash (price, volume, new customers, new markets, procurement, headcount, working capital, add-ons).
3. Map each assumption to one named initiative. An assumption with no initiative is "Unmapped". An exit multiple above the entry multiple with no stated ground is also Unmapped.
4. Check that no assumption is claimed by two initiatives.

Return the bridge table, the assumption-to-initiative map, and the Unmapped list ranked by value.
```

**Output:** An underwriting bridge that sums, an assumption map and a ranked list of unmapped assumptions.

**Moody's:** `searchAllDocuments` with the sector and region filters and recent research only gives Moody's sector outlook to set against the underwritten growth rate. Discard irrelevant hits and label it as context; it does not forecast this company.

**Tested:** Pure Gym group, November 2026, with Moody's forecast in the issuer's credit opinion standing in for the case. The bridge mechanics footed. The group had four defensible 2025 EBITDA figures (£208.7m to £336.9m: company-adjusted, financials tool, Moody's-adjusted, company-reported), so the definition must be fixed first. The sector search returned no UK fitness outlook; 10 of 15 hits were other sectors.

**Follow-ups:**
- "Which unmapped assumptions carry more than [10%] of value created, and who in management should own each?"
- "Show the bridge with the exit multiple held at the entry multiple."

**Adjust for:** Buy-and-build: split EBITDA into organic and acquired before mapping, and make the add-on programme its own initiative family. Take-private: add two families, public-company costs removed and investments public holders would not fund. Turnaround: map cash initiatives first; growth assumptions wait.

### VC-02 Size each initiative

**Use case:** Put a defensible, risk-adjusted figure on every initiative before anyone commits to it.

**Inputs:** VC-01 output, management's own estimates, any lever diagnostics already run (VC-07 to VC-18).

**Prompt:**
```
Size each initiative in the [Company] register in four steps, each in its own column:

1. Baseline: the current reconciled figure the initiative will move, with its source.
2. Full potential: the run-rate effect if everything works, with the arithmetic shown. Revenue initiatives convert to EBITDA at the contribution margin of that product or segment (revenue less the costs that vary with it), never at 100%.
3. Risk-adjusted: full potential times a confidence weight set by evidence grade: Diagnostic (measured on company data) [80%], Management (management estimate with some support) [60%], Assumption [30%].
4. Phased: risk-adjusted value by quarter from start date to run-rate date, net of one-off cost.

Also record per initiative: cash effect, one-off cost, difficulty (low / medium / high), owner, start date and evidence locator. Keep management's own estimate in a separate column.

Then total the risk-adjusted figures and compare with the bridge from VC-01. If the gap is above [5%], say which initiatives explain it.
```

**Output:** An initiative register with four sizing columns, management's estimate alongside, and a reconciliation to the bridge.

**Moody's:** `getEntityPeers` (screened under rule 12a, or the VC-17 set) then `getEntityFinancials_v2` on each peer gives peer margins as a sense-check on full potential. A peer gap is an upper bound, never a sizing. State the source format returned (Moody's Ratings Format or Standard Format); the same entity can show different EBITDA in each. Use consolidated group figures, not an operating subsidiary's, and say if fewer than three usable peers remain.

**Tested:** Pure Gym group, November 2026. 4 of 10 peers survived rule 12a and only two gave usable margins (37.3% and 39.0%): one peer returned revenue of 0, and Pure Gym Limited showed £283.9m EBITDA in Standard Format against £242.4m in Moody's Ratings Format for the same year. Without a register, the sizing columns were gaps.

**Follow-ups:**
- "Which initiatives depend on an Assumption grade, and what single piece of data would move each to Diagnostic?"
- "Show the phasing as a quarterly chart of cumulative risk-adjusted EBITDA."

**Adjust for:** Growth equity and minority influence without control: the register is a proposal to the controlling holder and the board; leave owner blank until the person has agreed. Large-cap: add the fund's shared procurement programme as a possible owner.

### VC-03 Write the 100-day plan

**Use case:** Choose the few things that must happen in the first 100 days and say what is deliberately left out.

**Inputs:** The sized initiative register, the organisation chart, the closing documents.

**Prompt:**
```
Write the 100-day plan for [Company].

1. Management-time budget: for each executive, assume [20%] of their time is available for change work, and hold back [20%] of that as contingency. Show available days per executive per fortnight.
2. Pick [8 to 15] actions. Each has: what is done, owner, start and end date, days of executive time it needs, the initiative it serves, and what "done" looks like.
3. Check no executive is booked above their budget in any fortnight. If they are, move or drop actions.
4. Write the cut list: every action considered and left out, with the reason and the date it will be looked at again.

Order actions so that measurement and reporting come before anything that needs measuring.
```

**Output:** A dated action list within a management-time budget, and a cut list.

**Moody's:** Not needed.

**Follow-ups:**
- "Which three actions would we regret most if they slipped by a month?"
- "Turn this into a one-page board summary."

**Adjust for:** Carve-out: lead with day-one readiness and exits from transition service agreements (the seller's temporary services). Lower-mid-market: the first actions are a finance function, a first budget and a first monthly pack. Turnaround: start with a 13-week cash flow forecast; nothing else starts until it exists.

### VC-04 Day-one list

**Use case:** Make sure nothing breaks on the day ownership changes.

**Inputs:** Closing documents, financing agreements, the organisation chart, key contracts, any transition service agreement.

**Prompt:**
```
Write the day-one list for [Company] under new ownership on [closing date].

Cover: bank mandates and signatories; payroll and supplier payment runs; delegated authorities and approval limits; insurance in force; notices required under financing agreements and key contracts (change of control, consent letters); first covenant reporting dates; communications to employees, customers and suppliers (who says what, and when); systems access for anyone leaving; and any condition subsequent from the purchase agreement.

For each item: owner, deadline (day one, week one, month one), source document with locator, and what happens if it is missed.
```

**Output:** A dated, owned day-one checklist.

**Moody's:** Not needed.

**Follow-ups:**
- "Which items need a signature from someone who is leaving?"

**Adjust for:** Carve-out: add every service the parent still provides, with the agreement reference and exit date. Take-private: add de-listing steps and the final market notices, flagged for counsel.

### VC-05 Test the plan against management time and covenant headroom

**Use case:** Find out whether the plan is deliverable by these people under this debt.

**Inputs:** The sized register, the 100-day plan, the credit agreement covenant definitions and test dates, the financing model.

**Prompt:**
```
Test [Company]'s value-creation plan against four constraints.

1. Bandwidth: initiatives per executive (flag above [3]); initiatives running at once (flag above [12]); any initiative owned by a vacant or interim role fails.
2. Covenant headroom: for each test date, recompute the covenants as defined in the credit agreement (quote the definition and locator) at 100%, 75% and 50% of risk-adjusted benefits, with one-off costs held at 100%. Flag any period where headroom falls below [15%].
3. Double counting: list any benefit claimed by two initiatives.
4. Reconciliation: risk-adjusted total versus the underwriting bridge, within [5%].

For each failure, propose a fix: re-phase, re-assign, drop, or seek a covenant amendment. Do not change the plan yourself.
```

**Output:** A constraint table with pass or fail by test, the stress cases by covenant test date, and proposed fixes.

**Moody's:** `findEntity` on the group, including the Bidco or Holdco name (alternatives on the operating company did not show the rated issuer in testing), then `getCreditOpinion` and `searchAllDocuments` scoped to the company's own rated issuer (often the Bidco) for covenant type, springing triggers, facility sizes, maturities and liquidity (rule 11b). Moody's research rarely gives the covenant limit; the executed credit agreement governs the test. Never treat a rating trigger as a covenant. For rated peers, `getEntityRatings` and `getCreditOpinion` (section `KeyIndicatorsTable`) give leverage and coverage as context.

**Tested:** Pure Gym group, November 2026. The issuer's credit opinion gave a springing senior secured net leverage covenant on the £216m SSRCF, tested when drawings exceed 40%, with notes due 2032; the covenant limit was not in any Moody's document. A 2023 covenant-quality snapshot covered notes since repaid. Stressing Moody's forecast EBITDA against the 6.0x downgrade trigger worked only as a labelled rating sensitivity.

**Follow-ups:**
- "Which single initiative, if it slipped two quarters, would cause the first covenant breach?"

**Adjust for:** Turnaround: run the cash test first; no growth initiative passes until it does. Growth buyout: add hiring capacity as a fifth constraint (see VC-25 and VC-30).

### VC-06 Initiative tracker and benefit ledger

**Use case:** Keep an honest monthly record of what each initiative has delivered.

**Inputs:** The initiative register, the latest closed management accounts, owner updates.

**Prompt:**
```
Update the [Company] initiative tracker for [month].

For each initiative: milestone status (on track / at risk / late, with days late); benefit status from a closed list: Identified, Committed (owner and budget agreed) or Realised (visible in closed management accounts for one full measurement period); planned versus actual benefit to date, with the ledger line that shows it; one-off cost to date versus budget.

Rules: a benefit counts as Realised only when you can point to the ledger line; owner updates without ledger evidence stay Committed. Flag any initiative more than [60] days behind for the board.

End with the total Realised, Committed and Identified figures against the plan.
```

**Output:** A tracker with a benefit ledger and board escalations.

**Moody's:** Not needed.

**Follow-ups:**
- "Show cumulative Realised benefit against the original phasing."
- "Which owners report progress that does not show in the ledger?"

**Adjust for:** Buy-and-build: keep synergies in the synergy tracker (VC-36) and reference them here, so no benefit is counted twice.

---

## Lever diagnostics

### VC-07 Data-quality gate

**Use case:** Check the company's raw data before any analysis runs on it.

**Inputs:** Row-level extracts (invoice lines, purchase lines, customer revenue by month, balance sheets), and the reported management accounts.

**Prompt:**
```
Before any analysis, run a data-quality check on these [Company] extracts.

For each dataset: row count, period covered, fields present, share of value with a missing customer, product or date (flag above [1%]), duplicate rows, and currency.

Reconcile each dataset to reported figures: revenue extracts to reported revenue, spend extracts to the in-scope cost lines. Show the difference by year and by month. A dataset fails if the difference is above [1%] in a year or [2%] in any month.

List which analyses each dataset can and cannot support. Do not run any analysis on a dataset that fails; say what would fix it.
```

**Output:** A data register with pass or fail per dataset and the analyses each supports.

**Moody's:** Not needed.

**Follow-ups:**
- "Write the data request to the finance team for every failed dataset."

**Adjust for:** Carve-out: record which data sits in the seller's systems and will be lost when transition services end. Lower-mid-market: expect ledger exports; say early which methods thin history rules out. Minority influence without control: list what the information rights allow.

### VC-08 Price-volume-mix bridge

**Use case:** Separate how much revenue growth came from price, volume and mix on the company's own invoices.

**Inputs:** Invoice lines for two periods (date, customer, product, units, unit of measure, net revenue, currency) that passed VC-07.

**Prompt:**
```
Build a price-volume-mix bridge for [Company] from [period 0] to [period 1], in code, with the results in a table.

A line is product times customer. For lines sold in both periods, with units q, net price p = revenue divided by units, total units Q and average price P0 = revenue in period 0 divided by Q0:
- Price effect = sum of (p1 minus p0) times q1
- Volume effect = (Q1 minus Q0) times P0
- Mix effect = sum of (p0 times q1) minus P0 times Q1
- New lines = period 1 revenue of lines with no period 0 sales
- Lost lines = minus period 0 revenue of lines with no period 1 sales

Remove currency effects first and show them as a separate line. State the convention: price is measured on current volumes, so the interaction sits in the price effect. Check that price plus volume plus mix equals the change on continuing lines exactly, and that adding new and lost lines gives the total change. If units are not comparable across products, report volume and mix as one figure: sum of (q1 minus q0) times p0.
```

**Output:** A bridge that sums to the total change, with the convention stated.

**Moody's:** Not needed.

**Follow-ups:**
- "Repeat by customer segment and by channel."
- "Show the price effect with the interaction term on its own line instead."

**Adjust for:** Services businesses: use hours or headcount as the volume unit. Buy-and-build: run per acquired entity as well as in total.

### VC-09 Discount waterfall to pocket price

**Use case:** Find where price leaks between the list price and the cash the company keeps.

**Inputs:** Invoice lines with list price and on-invoice discounts by type; off-invoice items by customer (rebates, settlement discounts, credits, returns, free goods, freight absorbed, marketing allowances).

**Prompt:**
```
Build the price waterfall for [Company] for [period].

Steps: list price, less on-invoice discounts by type, gives invoice price; less each off-invoice item, gives pocket price (what the company keeps). Show each step in currency and as a percentage of list. Realisation = pocket price divided by list price.

Checks: list revenue less every step equals pocket revenue; invoice revenue ties to the sales ledger; each off-invoice total ties to its ledger account.

Then show realisation by customer size band, product and sales representative, and list off-invoice items that have no recorded approval.
```

**Output:** A waterfall that reconciles, realisation by segment and a list of unapproved leakage.

**Moody's:** Not needed.

**Follow-ups:**
- "Which ten customers have the lowest realisation, and what drives it for each?"

**Adjust for:** Subscription businesses: treat free months and onboarding credits as off-invoice items.

### VC-10 Price dispersion for like customers

**Use case:** Find customers paying well below others who buy the same thing in the same way.

**Inputs:** Pocket price by customer from VC-09, customer size and channel.

**Prompt:**
```
For [Company], group customers by product, size band [define bands] and channel. Within each group, show median and quartiles of pocket price and plot pocket price against annual volume.

Flag customers priced below the lower quartile of their group, separating small accounts from large ones (large accounts may have earned their price through volume).

Size the gap of moving each flagged customer to the group median. Label it an upper bound, then apply a realisation haircut of [50%] and label that figure separately. Show group sizes; do not report a group with fewer than [10] customers.
```

**Output:** Dispersion by group, flagged customers and a labelled upper bound with the haircut applied.

**Moody's:** Not needed.

**Follow-ups:**
- "For the flagged customers, when does each contract allow a price change?"

**Adjust for:** Growth equity: focus on new-customer pricing, where dispersion is easier to correct.

### VC-11 Test pricing headroom

**Use case:** Decide whether the data can estimate price sensitivity, and if not, what the last price rise shows.

**Inputs:** Monthly volume and price by product group, promotion and stock-out flags, the history of price increases.

**Prompt:**
```
For [Company], test whether price elasticity (the percentage change in volume for a one per cent change in price) can be estimated.

Run it only if every product group has at least [24] monthly periods and at least [4] distinct price changes not tied to one event. Method: regression of log volume on log price with controls for season, promotion and stock availability. Report the estimate only if its confidence interval excludes zero and the sign is negative.

If the data does not qualify, write "elasticity not estimable on this data" and instead report the last price increase: realised increase divided by announced increase, and customers lost in the four quarters after.

In both cases, show break-even volume loss for a price rise of [x%]: price rise divided by (contribution margin plus price rise), all as percentages.
```

**Output:** Either an elasticity estimate with its interval or a statement that it cannot be estimated, the last-increase record, and break-even volume loss.

**Moody's:** Not needed.

**Follow-ups:**
- "Which product groups could take a rise with the least volume at risk, based only on what the data shows?"

**Adjust for:** Lower-mid-market: expect too little history; go straight to the last-increase record.

### VC-12 Customer retention by cohort

**Use case:** See whether customers stay and grow, on the company's own data.

**Inputs:** Revenue by customer by month for at least [36] months, with churned customers kept at zero.

**Prompt:**
```
Build cohort retention for [Company]. A cohort is the customers whose first revenue falls in the same [quarter / year]. Rows are cohorts, columns are age.

Show: logo retention at each age (customers active divided by customers at age 0); gross revenue retention = sum of the lower of end and start revenue, divided by start revenue, for customers active at the start (cannot exceed 100%); net revenue retention = end revenue from customers active at the start divided by their start revenue. Always show gross and net together.

Write the definition of "active" before computing if revenue is not contractual. Compare cohorts at equal age, never at the same calendar date. Check the table summed by month equals reported revenue. Do not show a cohort of fewer than [30] customers on its own.

Only build a churn model if there are at least [200] loss events with driver fields recorded before the loss; otherwise report loss rates by segment, tenure and size with counts.
```

**Output:** A cohort table, gross and net retention, and loss rates or a churn model where the data allows.

**Moody's:** Not needed.

**Follow-ups:**
- "Which segment's cohorts are getting worse with each new year?"

**Adjust for:** Growth equity and growth buyout: add payback on customer acquisition cost by cohort.

### VC-13 Procurement spend cube and tail spend

**Use case:** See where the money goes, so negotiations start in the right place.

**Inputs:** Payables invoice or purchase-order lines (supplier, date, amount, currency, business unit, ledger account, item code, quantity, unit price, payment terms).

**Prompt:**
```
Build a spend cube for [Company] for [period].

1. Normalise supplier names and group suppliers under their ultimate parent.
2. Assign every line to one category in a tree of [two or three] levels.
3. Cube: spend by category, supplier, business unit and period.
4. Tail spend: the suppliers making up the last [20%] of spend, with supplier and transaction counts.
5. Price variance on the same item (same item code and unit of measure): sum of (price paid minus reference price) times quantity, with the reference price set at [the lower-quartile price paid in the period]. Label it an upper bound.
6. Terms: contract payment terms against days actually taken to pay.

Check that the cube total reconciles to cost of goods sold plus operating expenses, less payroll, tax, depreciation and intercompany. Say where to open negotiations; do not call any figure a saving.
```

**Output:** A reconciled spend cube, a tail-spend view, an item price variance labelled as an upper bound, and a terms comparison.

**Moody's:** `findEntity` on the top suppliers in one call, then `getEntityUltimateOwners` (threshold 50.01) groups suppliers that belong to the same parent. Search by registered legal name and country, not brand (Matrix is a Johnson Health Tech brand), and check each match's country under rule 9a. An owner record that names the supplier itself means no parent was found, not that the supplier is independent.

**Tested:** Pure Gym group, November 2026, Moody's step only on illustrative equipment-maker names (neither annual report names suppliers). "Precor Incorporated" resolved to a Brazilian entity with no owner data; "Matrix Fitness UK Ltd" was not found; Johnson Health Tech showed itself as its own owner. The spend cube needs payables data.

**Follow-ups:**
- "Which categories could move to fewer suppliers without creating a single source?"

**Adjust for:** Buy-and-build: build the cube across all entities to show shared suppliers. Large-cap: run by business unit and feed the fund's shared procurement programme. Growth equity: usually skipped; say so.

### VC-14 Supplier concentration and credit check

**Use case:** Find suppliers the business depends on and check whether they are financially sound.

**Inputs:** The spend cube from VC-13, supplier contracts for the largest suppliers.

**Prompt:**
```
For [Company], list suppliers that are critical: top one and top three share of spend per category, single-source items, and suppliers with no alternative qualified within [90] days.

For each critical supplier: spend, share of category, contract end date and termination terms (with locator), ultimate owner, and credit standing. State whether credit standing is an assigned rating or a model output.

Rank suppliers by the combination of dependence and weak credit, and propose one mitigation per supplier (second source, stock buffer, contract term, monitoring).
```

**Output:** A critical-supplier table with ownership, credit and mitigations.

**Moody's:** `findEntity` on all critical suppliers in one call, by registered legal name and country rather than brand (a brand such as Matrix belongs to Johnson Health Tech; rule 9). Then `getEntitySubsidiaries` on the parent to find the entity that invoices the company in its country, `getEntityUltimateOwners`, `getEntityRatings`, and `getEntityProbabilityOfDefault_v2` (category `early_warning`) where unrated. Label ratings as opinions and probability of default as a model output (rule 11); PD comes back as a decimal fraction (0.0224 = 2.24%). The company's annual report via `searchCompanyFilings` may describe supply-chain and energy dependence even when it names no suppliers. Moody's does not say whether a supplier will accept new terms.

**Tested:** Pure Gym group, November 2026. "Life Fitness" resolved to Life Time, Inc. (a gym operator) and "Precor" to its former owner, neither flagged as not found (rule 9a). The annual report gave the energy-contract cover; spend and contract terms remained gaps. Round 2, with registered names and countries: Life Fitness, LLC resolved correctly and showed a Severe early-warning category (from a payment model); "Precor Incorporated" resolved to a Brazilian entity. Some supplier owner records show the company as its own owner, so sponsor ownership of a supplier may need news or the supplier's own filings.

**Follow-ups:**
- "Which critical suppliers are owned by a competitor or by another sponsor?"
- "Set up a quarterly watch on these suppliers' news and default probability."

**Adjust for:** Turnaround: add suppliers who have shortened credit terms with the company in the last [six] months.

### VC-15 Working-capital days and cash release

**Use case:** Measure the cash tied up in working capital and how much could be released, keeping one-off and recurring gains apart.

**Inputs:** Monthly balance sheets and profit and loss for at least [24] months, receivables and payables ageing, invoice-level payment dates.

**Prompt:**
```
For [Company], compute monthly in a table:
- Receivable days = trade receivables divided by revenue, times days in period
- Payable days = trade payables divided by cost of goods sold, times days in period
- Inventory days = inventory divided by cost of goods sold, times days in period
- Cash conversion cycle = receivable days plus inventory days minus payable days

State whether balances are period-end or average, and adjust if receivables include sales tax and revenue does not. Compare period-end with mid-month balances to catch delayed payment runs.

If cost of goods sold is not a meaningful base (service and subscription businesses, where it covers only merchandise or a small share of costs), compute payable days on total cash operating costs, or on revenue if those are not available, and drop inventory days if inventory is immaterial. Say which base you used for each measure, and use the same base for targets and cash release.

For target days of [receivables / inventory / payables], compute cash release:
- Receivables: (current days minus target days) times revenue divided by days in period
- Inventory: (current days minus target days) times cost of goods sold divided by days in period
- Payables: (target days minus current days) times cost of goods sold divided by days in period

Show the release as a one-off cash gain. Show the recurring gain separately: lower interest cost at [rate] and lower bad debts. Net each lever of its cost (settlement discounts, supplier price rises, stock-out risk).
```

**Output:** A days table, cash release by lever, and one-off and recurring gains on separate lines.

**Moody's:** `getEntityPeers` then `getEntityFinancials_v2` on each gives peer receivable, payable and inventory levels, labelled as external benchmarks with period and basis. These are year-end balances only: label peer days as year-end and do not compare them with the company's monthly averages. Use the screened peer set from VC-17, not the raw peer list (rule 12a).

**Tested:** Pure Gym group, November 2026. Payables of £19.0m against cost of goods sold of £18.0m gave 385 payable days, meaningless for a gym; payables on revenue (9.3 days against a peer median of 19.0) was the usable measure. Two of four peers had only 2024 accounts.

**Follow-ups:**
- "Which 20 customers account for most overdue receivables, and what are their terms?"

**Adjust for:** Turnaround: run weekly and start here. Carve-out: use standalone balances, not the parent's. Seasonal businesses: set targets by month, not one annual figure.

### VC-16 Cost-to-serve and customer profitability

**Use case:** Find which customers make money once the cost of serving them is counted.

**Inputs:** Revenue and gross margin by customer, plus activity data: orders, deliveries, returns, support tickets, account management time, payment days.

**Prompt:**
```
Build customer profitability for [Company] for [period].

1. Start from gross margin by customer.
2. Allocate cost-to-serve using activity drivers: cost per order, per delivery, per return, per support ticket, per hour of account management, and the financing cost of the customer's payment days at [rate]. Show each driver rate and how it was derived from the cost base.
3. Check that allocated costs sum to the cost pools they came from.
4. Rank customers by profit after cost-to-serve, and show the cumulative profit curve.

List customers that lose money after cost-to-serve, with the driver that causes it. Say which drivers rest on estimates rather than recorded activity.
```

**Output:** A customer profitability table, a cumulative profit curve and loss-making customers with causes.

**Moody's:** Not needed.

**Follow-ups:**
- "For loss-making customers, which change (price, minimum order, service level) fixes each one?"

**Adjust for:** Turnaround: use this to choose segments to exit. Growth equity: run by segment rather than customer.

### VC-17 Peer benchmark gap

**Use case:** See where the company sits against peers, and size the gap as a ceiling to investigate.

**Inputs:** The company's reconciled financials; the Moody's connector if available.

**Prompt:**
```
Benchmark [Company] against peers on: gross margin, EBITDA margin, revenue growth, receivable days, payable days, inventory days, revenue per employee and leverage.

1. Build a peer set of at least [5] companies and give one line on why each is comparable (product, customers, geography, size). Drop any that is not. If fewer than [5] survive, say how many, add names the deal team gives, and if still short report the shortfall and state that medians and quartiles rest on a small set; never pad the set with weak comparables.
2. For each measure: the company's figure, peer median and quartiles, and the period, currency and basis of each peer figure. Choose measures the business model supports: where cost of goods sold is not a meaningful base (service and subscription businesses), replace gross margin and inventory days with measures the accounts support, compute payable days on cash operating costs or revenue, and list the measures you dropped. State the date of every headcount used for revenue per employee, and do not compare it across currencies without a stated exchange rate.
3. Value of closing half the gap to the median, labelled "upper bound to investigate". Never call it a target or forecast.
4. List differences in definition that could explain part of each gap.
```

**Output:** A peer table with stated comparability, and half-gap values labelled as upper bounds.

**Moody's:** `findEntity` on the company, then `getEntityPeers` (up to 10), then `getEntityFinancials_v2` on each peer (sections `all_ratios` where useful). Screen the list under rule 12a before computing anything. Run `getEntityPeers` on the operating company (on a holding company it returns unrelated holding companies), but take the company's own figures from the entity that files consolidated accounts (rule 9b), and its leverage from the rated issuer's credit opinion (rule 11a). Headcount comes from `getEntityProfile` and is undated. Moody's does not supply peer prices, discounts or cohorts.

**Tested:** Pure Gym group, November 2026. Of 10 peers returned, 6 were dropped: three subsidiaries of groups already in the set, a property entity, a US benefits company and a theme-park group. Four survived, one below the minimum. One peer's headcount came back as 0. Round 2: `getEntityPeers` on the consolidated holding company returned ten unrelated UK holding companies and the group's own Bidco, so the peer call must run on the operating company. Adding the deal team's names (Basic-Fit N.V., Deuce Midco Limited) restored the set to six, though Deuce Midco returned revenue only.

**Follow-ups:**
- "Which gaps also appear in our own diagnostics, and which only in the benchmark?"

**Adjust for:** Take-private: before closing, this is the only diagnostic that can run. Minority influence without control: it carries more weight because company data is limited.

### VC-18 AI opportunities

**Use case:** Find a small number of places where AI tools can raise earnings during the hold, and say no to the rest.

**Inputs:** The cost base by function, process descriptions, systems list, the initiative register.

**Prompt:**
```
Identify AI opportunities for [Company]. For each candidate process, answer three gate questions:
1. Data: is the data usable now, without a long data project first? (Fails if the answer starts "first we would need to".)
2. Owner: is there a named owner in the management team who will run it? (Fails if only a sponsor or committee is named.)
3. Pilot: can a pilot start within [30] days with one team, one process and bought tools? (Fails if it needs a custom build or several teams.)

All three yes gives Go. Any no gives Wait, with the one thing that would unblock it.

Size Go items: hours saved at [30%] of task time (never 100%). Count hours as earnings only where a cost line falls or freed capacity produces measured revenue; otherwise record capacity freed and earnings impact of zero. Rank by annual earnings impact net of tool cost (licences, set-up, training), then by months to first measurable result.

End with a not-doing list: every opportunity rejected, the reason and the date to look again.
```

**Output:** A gated opportunity list (Go / Wait), sized Go items and a not-doing list.

**Moody's:** `searchCompanyFilings` on the company or a listed peer (annual report, emerging-risk and technology sections) for AI already in use and systems changes under way. `searchAllDocuments` with the sector filter gives Moody's AI research as context only; in testing most hits were about technology companies, so discard the rest.

**Tested:** Pure Gym group with The Gym Group PLC as listed stand-in, November 2026. The listed peer's annual report said AI is already used in customer service, marketing, data and software delivery, and that member-management and payment systems are being replaced; Moody's described PureGym's app-based model with no gym-level sales staff. The sector search returned 1 relevant document of 9. With no named owners, every candidate stayed at Wait.

**Follow-ups:**
- "Write the pilot brief for the top Go item, with its measure of success."

**Adjust for:** Short remaining hold: prefer results that show in trailing twelve-month earnings before the sale. Thematic funds: check that opportunities do not conflict with the theme's commitments.

---

## Go-to-market expansion

### VC-19 Commercial baseline and funnel

**Use case:** Know where revenue comes from and how the sales engine performs before planning growth.

**Inputs:** Revenue by customer by month with segment, geography, product and channel; opportunity records from the customer-relationship system (CRM) with stage dates and outcomes; sales and marketing cost by month.

**Prompt:**
```
Build the commercial baseline for [Company] for [period].

1. Revenue by segment, geography, product and channel. Each cut must sum to reported revenue within [1%].
2. Funnel: stage conversion for a fixed starting cohort (opportunities entering the next stage divided by those entering this stage); win rate = won divided by (won plus lost), by count and by value, excluding open opportunities; median sales cycle in days for won deals.
3. Customer acquisition cost = sales and marketing cost in the period divided by new customers won. Payback in months = acquisition cost divided by (monthly revenue per new customer times gross margin). Flag payback above [24] months.
4. Check that opportunities won in the CRM reconcile to new revenue billed.

Do not show a win rate for any cell with fewer than [30] closed opportunities.
```

**Output:** A reconciled revenue baseline, funnel measures and acquisition cost with payback.

**Moody's:** Not needed.

**Follow-ups:**
- "Where in the funnel do we lose most value, and is that consistent across segments?"

**Adjust for:** Lower-mid-market: expect no CRM; first actions are a written sales process and stage definitions, and funnel measures wait for a quarter of clean data. Carve-out: separate revenue won through channels shared with the parent.

### VC-20 White-space grid

**Use case:** Show where the company sells today and where it does not, with the evidence of demand in each gap.

**Inputs:** VC-19 baseline, lost-deal reasons, inbound enquiries, customer interviews, any market model built by the fund or an adviser.

**Prompt:**
```
Build a white-space grid for [Company]: segments by geographies by products.

Mark each cell once: Served, Under-served (some sales, well below [threshold]), or Unserved. For each Under-served or Unserved cell, give the evidence of demand and grade it from a closed list: Proven (the company already sells this to this kind of customer), Tested (orders, signed pilots or paid trials), Indicated (inbound enquiries, deals lost for lack of the offer, interviews), Assumption (market reports or analogy only). Every claim needs a locator.

Then shortlist [3 to 5] cells to size, and say why the others are left out.
```

**Output:** A graded grid and a shortlist with reasons.

**Moody's:** `getEntityCountryProfile` for each candidate country and `searchAllDocuments` with sector and region filters give country and sector context. `getEntityPeers` then `getEntitySegmentFinancials` shows which peers report revenue in a market (check the currency label, rule 12a; request `outputFormat` json, which gives local-currency and USD values side by side, because the default table can show USD figures under a local-currency header). `searchNews` on named local operators shows recent deals and expansion. None of this is a market size; it never raises a grade above Assumption by itself.

**Tested:** Pure Gym group, hypothetical entry into Spain, November 2026. Sector research filtered to Spain returned nothing on fitness; mark it [MOODY'S: NOT COVERED]. `getEntityCountryProfile` failed for one Spanish entity and worked for another in the same country, so retry with a different entity. A listed peer's segment data and news on local operators gave the useful evidence.

**Follow-ups:**
- "Which Unserved cells could be reached through an existing customer's other sites?"

**Adjust for:** Thematic funds: order markets by how far the theme's adoption has progressed there, with policy and subsidy dependence stated. Large-cap: run by business unit and country.

### VC-21 Size the expansion options

**Use case:** Put revenue, margin, investment and payback on each shortlisted option.

**Inputs:** The shortlist from VC-20, the baseline, cost assumptions for entering each option.

**Prompt:**
```
Size each expansion option for [Company] by quarter for [years]:
- revenue, with the volume and price assumptions shown;
- contribution after the cost of selling it (never revenue at 100%);
- investment: people, marketing, product changes, entry costs;
- time to first revenue;
- payback: the month in which cumulative contribution first exceeds cumulative investment;
- the key assumption and the evidence grade from VC-20.

Risk-adjust revenue by grade: Proven [80%], Tested [60%], Indicated [40%], Assumption [20%]. Check options do not claim the same customers or revenue, or a benefit already in the value-creation plan. Rank by risk-adjusted contribution and payback.
```

**Output:** A phased sizing per option, risk-adjusted, ranked, with an overlap check.

**Moody's:** `getEntityPeers` (screened under rule 12a, or the VC-17 set) then `getEntityFinancials_v2` gives peer revenue and margins. In testing no peer record carried a selling or marketing line; take those from peers' annual reports through `searchCompanyFilings`, and say when a peer reports marketing inside other costs. For options already entered by acquisition, issuer comments and the annual report give price, contribution and integration cost.

**Tested:** Pure Gym group, US (Blink) and Switzerland (NonStop Gym) as options already taken, November 2026. Price, sites, revenue contribution and integration cost came from the annual report and a Moody's issuer comment; no selling or marketing line came back for any of five gym records. Forward sizing needs the company's cost assumptions.

**Follow-ups:**
- "What would have to be true for the lowest-ranked option to beat the top one?"

**Adjust for:** Buy-and-build: add entry by acquisition as an option in its own right and hand candidates to VC-34. Turnaround: expansion waits; use the baseline to find segments to leave.

### VC-22 Partner and distributor screen in a new market

**Use case:** Check the legal identity, owners and credit of possible partners, distributors or anchor customers before talks start.

**Inputs:** A list of candidate partners in [country] with whatever identifiers are known; the entry route chosen.

**Prompt:**
```
Screen these candidate partners for [Company]'s entry into [country]: [list].

For each: matched legal entity, country and identifier; ultimate owner and group; directors on record; credit standing (state whether a rating or a model output); recent significant news; overlap with our competitors (owned by or distributing for one); and open questions for a reference call.

This screen gives factual context only. It does not judge whether a partner is good, and it is not sanctions or integrity clearance. List what a licensed screening service and local counsel must still check.
```

**Output:** A partner table with entity, ownership, credit, news and open questions, and a scope statement.

**Moody's:** `findEntity` on all candidates in one call (print matches and stop if any looks wrong). A country filter is ignored when several names are searched together, so write the country into each name, and set getAlternatives to true: the alternatives often hold the right operating company when the main match is wrong. Then `getEntityUltimateOwners`, `getEntityManagersDirectors_v2`, `getEntityRatings` or `getEntityProbabilityOfDefault_v2`, and `searchNews` over [365] days with importance set to significance. `getEntityCountryProfile` adds country context. Smaller private distributors may return [MOODY'S: NOT COVERED]. Where the ownership chain stops at a holding vehicle, take the sponsor from news, labelled as news; where ownership on record and recent news disagree (for example a sale agreed but not yet in the record), list it as Contradicted.

**Tested:** five Spanish gym operators, November 2026. One name was dropped from both the matched and not-found lists (rule 9a); two were not covered. Ownership stopped at Luxembourg vehicles. News showed the two strongest candidates had agreed to merge, which Moody's ownership records did not yet reflect. Round 2, with getAlternatives: the missing candidate appeared among another name's alternatives, but that entity's PD confidence said it was "not active (dissolved or inactive)", so it was not the trading company; "Supera" with a Spain filter matched an Australian finance company.

**Follow-ups:**
- "Which candidates are owned by a sponsor, and when did that sponsor buy them?"

**Adjust for:** Large-cap: flag distribution-law and merger-control questions for counsel. Buy-and-build: a candidate distributor may also be an add-on; pass it to VC-34.

### VC-23 Pricing and packaging for a new segment

**Use case:** Set an offer and price for a segment the company does not yet serve well.

**Inputs:** Current price lists and packages, pocket price by segment (VC-09), customer interviews in the new segment, competitor price evidence if held.

**Prompt:**
```
Propose pricing and packaging for [Company] in [segment].

1. What this segment needs that current packages do not provide, and what it does not need, with evidence.
2. [Two or three] package options: contents, price metric (per user, per unit, per site, subscription), and list price.
3. For each option: contribution per customer, discount guardrails, and the risk of existing customers moving to the cheaper package (state how many existing customers would qualify).
4. How to test price before committing (paid pilot, limited offer, quotes at two price points).

Use only price evidence held by the company or cited sources. Where none exists, mark the price an Assumption.
```

**Output:** Package options with contribution, cannibalisation risk and a price test design.

**Moody's:** Not needed. Moody's does not supply prices.

**Follow-ups:**
- "Draft the discount approval matrix for the new packages."

**Adjust for:** Growth equity: the proposal goes to management, who decide.

### VC-24 Cross-sell into the existing base

**Use case:** Find customers who buy one product and could buy another.

**Inputs:** Revenue by customer by product, product adoption dates, account owners.

**Prompt:**
```
For [Company], build a customer-by-product matrix and show penetration of each product in each segment.

For each product pair, list customers who buy A but not B, where similar customers (same segment and size band) buy both. Size the opportunity as those customers times the median spend on B among similar customers who buy it, labelled an upper bound. Show group sizes.

Then list the evidence that cross-selling works here: past cross-sell orders, time from first to second product, and retention of multi-product customers against single-product ones. Grade the evidence (Proven / Tested / Indicated / Assumption).
```

**Output:** A penetration matrix, cross-sell upper bounds and graded evidence.

**Moody's:** Not needed.

**Follow-ups:**
- "Which account owners hold the largest cross-sell lists, and is that achievable alongside their targets?"

**Adjust for:** Buy-and-build: cross-sell between acquired businesses counts as Proven only where orders exist; revenue synergies start at zero.

### VC-25 Sales capacity planning

**Use case:** Check whether the sales team, seller by seller, can deliver the revenue in the plan.

**Inputs:** Sales roster (start dates, targets, results), ramp history, plan revenue by quarter, open pipeline by stage.

**Prompt:**
```
Test [Company]'s sales capacity against plan revenue for [years].

1. Ramp: from the team's history, the share of full target a seller achieves by tenure (month 1 to 3, 4 to 6, 7 to 12, after 12).
2. Attainment: the team's own historical attainment against target, never an aspiration.
3. Capacity = sum over sellers of (target times ramp factor for their tenure times expected attainment), by quarter, for current sellers and for planned hires on their planned start dates.
4. Productivity per seller = new revenue won divided by average number of fully ramped sellers.
5. Pipeline cover = stage-weighted open pipeline divided by remaining target. Flag below [3] times.
6. Attrition: apply the team's historical seller attrition to the roster.

Show capacity against plan by quarter and the gap. Flag if more than [25%] of plan revenue depends on sellers not yet hired. Give the hires needed, by start date, to close the gap.
```

**Output:** A seller-by-seller capacity model, the gap to plan by quarter, and the hiring needed.

**Moody's:** `getEntityPeers` (screened under rule 12a, or the VC-17 set) then `getEntityFinancials_v2`. In testing no peer record carried a sales and marketing line; take it from peers' annual reports through `searchCompanyFilings` where they disclose it. Read the account-substitution note: in testing, a request for sales and marketing expense was answered with Rent Expense. UK filers may report only selling and administrative costs; US 10-K filers report sales and marketing separately. Moody's has no pipeline, win-rate or seller data.

**Tested:** Pure Gym group, November 2026: not applicable (no seller roster; no sales and marketing line for five gym records). Re-run on The Sage Group plc: "The Sage Group plc" resolved to a subsidiary until looked up by BvD ID; the financials tool gave combined SG&A only and substituted Rent Expense for sales and marketing; peers' 10-Ks gave sales and marketing at 23.5% and 37.0% of revenue; Sage itself discloses commissions (£170m, 6.8% of revenue) but no sales and marketing split. The seller model needs the private roster.

**Follow-ups:**
- "What does capacity look like if new hires start [one quarter] late?"
- "Redraw territories so each seller's addressable accounts are comparable."

**Adjust for:** Growth equity and growth buyout: this is the core test of the plan; run it before the investment committee too (see DD-19). Carve-out: rebuild coverage from zero and date it against the exit from transition services. Consumer subscription businesses with no seller roster (gyms, digital sign-up): skip the seller model and test member-acquisition cost and conversion by channel instead.

### VC-26 Pilot design with a stop rule

**Use case:** Prove one expansion option in one market before committing money to several.

**Inputs:** The top-ranked option from VC-21, the capacity plan, the budget available.

**Prompt:**
```
Design a pilot for [Company] in [market or segment].

Charter: scope (one market or segment), owner, budget, team, start date. Leading indicators to track monthly (for example qualified opportunities, win rate, first orders, price achieved) with a target for each. Gates at [90] days and [180] days: what must be true to continue, scale or stop. A stop rule written in advance: the result at which the pilot ends, regardless of sunk cost.

Only [one] pilot runs at a time. State what would be needed to start a second.
```

**Output:** A pilot charter with owner, budget, leading indicators, dated gates and a stop rule.

**Moody's:** Not needed.

**Follow-ups:**
- "Write the 90-day gate review template."

**Adjust for:** Minority influence without control: the charter is a board proposal. Lower-mid-market: keep the budget within what the company can fund from cash.

---

## Talent and organisation

Every prompt in this area compiles evidence and prepares decisions. None makes a decision about any individual. Personal data stays with the reviewers named in the request and is handled under the fund's privacy policy and employment law. Protected characteristics are excluded from every analysis.

### VC-27 Critical roles the plan needs

**Use case:** List the roles the value-creation plan depends on, and when each must be filled.

**Inputs:** The initiative register with owners and dates, the organisation chart, the diligence management findings.

**Prompt:**
```
For [Company], map the roles the value-creation plan needs. Reviewers with access: [names].

1. For each initiative, the roles needed to deliver it and by when.
2. Mark a role Critical when an initiative cannot be delivered without it, or when losing its holder would put a covenant, a top customer or a licence at risk. Criticality belongs to the role and says nothing about the person in it.
3. For each critical role: filled, interim or vacant; initiatives it owns (flag above [3]); date needed.
4. Flag the plan CRITICAL if [chief executive / chief financial officer] is vacant or interim.

This compiles facts for the reviewers above. It makes no decision about any individual.
```

**Output:** A role map with critical roles, status, load and dates.

**Moody's:** `findEntity` on the company and its rated issuer, then `getEntityManagersDirectors_v2` with `includeDetails` set to true gives the factual record of registered officers and their dates. Use name, registered role and dates only; do not reproduce date of birth, age or gender. Roles come back as Director or Secretary, not executive titles, so take titles and interim status from the company. The record can be thin; it says nothing about ability.

**Tested:** Pure Gym group, November 2026. Two registered directors on the operating company and the rated issuer, against 5 directors and 3 senior managers in the annual report; no executive titles, so the step 4 CEO/CFO flag could not be set from Moody's. The detailed record included date of birth, age and gender, which were left out.

**Follow-ups:**
- "Which critical roles need interim cover because a permanent hire will take longer than [90] days?"

**Adjust for:** Carve-out: list every role that leaves with the parent, dated against transition-service exits. Buy-and-build: add an integration lead and a group finance function sized for several entities. Turnaround: cash and restructuring roles first.

### VC-28 Organisation spans, layers and function cost

**Use case:** Measure the shape and cost of the organisation from payroll data.

**Inputs:** Payroll and human-resources extract (role, function, manager, start date, fully loaded cost), management accounts.

**Prompt:**
```
Analyse [Company]'s organisation. Reviewers with access: [names].

1. Reconcile headcount and employment cost to payroll and the management accounts. Check every employee has one manager and one function, and the reporting tree has no loops.
2. Span of control (direct reports per manager): distribution by function; flag spans below [4] or above [12].
3. Layers from chief executive to front line, longest chain; flag above [7].
4. Fully loaded cost of each function as a share of revenue, and revenue per full-time-equivalent employee.
5. Depth of finance and data functions: can they produce monthly accounts, forecasts and covenant reporting on time?

Every flag is a question for management. Do not show any figure for a group of fewer than [10] people. Make no recommendation about any named person.
```

**Output:** A reconciled organisation analysis with flags framed as questions.

**Moody's:** `getEntityPeers` (screened under rule 12a, or the VC-17 set) then `getEntityFinancials_v2` gives peer revenue and employee counts for revenue per employee; state whether headcount is average or year-end. In testing no record carried staff costs; take them from annual reports through `searchCompanyFilings`. Moody's has no peer organisation charts or pay.

**Tested:** The Gym Group PLC and the Pure Gym group, November 2026. The listed peer's employee note reconciled (£57.6m underlying, £0.6m in cost of sales) and its 1,924 average employees matched Moody's. Revenue per employee ranged from £60.8k to £127.3k across five records on mixed headcount bases; one peer returned 0 employees in key financials. No staff-cost line came back from Moody's. The source split 8 senior people by gender, which the under-10 and protected-characteristics rules excluded.

**Follow-ups:**
- "Which flagged spans sit in functions the plan depends on?"

**Adjust for:** Buy-and-build: run per acquired entity to show duplicated roles as facts for the integration plan. Large-cap: run by division and flag works councils for counsel.

### VC-29 Attrition analysis

**Use case:** Find where people are leaving, without exposing any individual.

**Inputs:** Joiner and leaver records with function, tenure band, manager and regretted flag; aggregated engagement survey results.

**Prompt:**
```
Analyse attrition at [Company] for [period]. Reviewers with access: [names].

1. Check joiners less leavers explain the headcount movement each period.
2. Attrition rate = leavers divided by average headcount, annualised. Regretted attrition rate = leavers the company marked regretted divided by average headcount.
3. Cut by function, tenure band and manager group. Suppress any group under [10] people, including where its figure could be worked out by subtracting other groups from a total.
4. Aggregated engagement themes, with response counts.

Report patterns by group only. Do not identify, profile or predict the departure of any individual, and exclude protected characteristics.
```

**Output:** Attrition and regretted attrition by group with suppression applied, and survey themes.

**Moody's:** Not needed.

**Follow-ups:**
- "Which critical-role functions have attrition above the company average?"

**Adjust for:** Lower-mid-market: most groups will be suppressed; say so and report the company total only. Turnaround: engagement work waits; refer any headcount action to employment counsel.

### VC-30 Hiring plan and search briefs

**Use case:** Sequence the hires the plan needs and brief each search.

**Inputs:** VC-27 role map, VC-25 capacity gap if relevant, time-to-fill history.

**Prompt:**
```
Write the hiring plan for [Company]. Reviewers with access: [names].

1. Sequence hires by the date each initiative needs them, using median time to fill from the company's history (days from role approval to start).
2. Where a critical hire will take longer than [90] days, propose interim cover.
3. For each hire, a search brief: purpose of the role, initiatives it owns, reporting line, [3 to 5] first-year success measures tied to the plan, required experience, and the interview panel.
4. Total cost of hiring by quarter, and the effect on the plan if each hire is [one quarter] late.

Briefs describe the role. Selection decisions belong to the panel and the reviewers.
```

**Output:** A sequenced hiring plan, cost by quarter, and one search brief per hire.

**Moody's:** Not needed.

**Follow-ups:**
- "Which hires depend on another hire being in place first?"

**Adjust for:** Growth equity and growth buyout: hiring capacity is the constraint, so lead with it; founders decide. Lower-mid-market: the first hires are usually a finance lead and a commercial lead.

### VC-31 Leadership evidence pack for human review

**Use case:** Give the people who decide on senior roles a factual record of delivery, with the review grid left for them to complete.

**Inputs:** Initiative delivery by owner from monthly reviews, the diligence management findings, board minutes.

**Prompt:**
```
Compile a leadership evidence pack for the critical roles at [Company]. Reviewers with access: [names].

For each critical role and its holder: commitments made (from the plan, budgets and board minutes), with date and source; delivery against each, with the ledger or report that shows it; verbal observations marked as verbal and unverified, with speaker and date.

Then provide an empty review grid (for example performance against potential) for the reviewers to complete. Do not place anyone in it, score anyone, or comment on character or ability. Evidence about a person is a dated fact with a source, or it is left out. This pack compiles evidence only; any decision is for [the operating partner, the deal partner and the chair]. Leadership assessment, including psychometric testing, needs a qualified external assessor.
```

**Output:** A factual delivery record per critical role and an empty grid for human completion.

**Moody's:** `getEntityManagersDirectors_v2` gives registered offices held and their dates, as fact only. It supplies no assessment of people, and none may be inferred from it. Use name, registered role and dates only; do not reproduce date of birth, age or gender. Company-level results from filings or research are not evidence about a person unless a board paper names the owner.

**Tested:** Pure Gym group, November 2026. The officer record gave two registered directors with appointment dates and no titles. Moody's earlier forecasts set against later outcomes (for example leverage of about 5x expected for 2025 against 5.9x reported) made a dated company-level commitments ledger, kept unattributed. The grid stayed empty.

**Follow-ups:**
- "Draft the brief for an external assessor for [role]."

**Adjust for:** Minority influence without control: personal data is available only as the shareholder agreement allows; the pack goes to the board.

### VC-32 Management incentive plan modelling at 2x and 3x exits

**Use case:** Show what each manager would receive under the incentive plan at different exit outcomes, and what it costs the fund.

**Inputs:** Executed incentive plan documents, articles and shareholder agreement, capitalisation table, entry equity structure, exit cases.

**Prompt:**
```
Model [Company]'s management incentive plan. Reviewers with access: [names].

1. Mechanics, each with its document locator: instruments (ordinary shares, preference shares, loan notes, options), coupon or preferred return, ranking, management pool size, vesting, good and bad leaver terms, and any ratchet with its trigger.
2. Exit cases: total equity proceeds equal to [2.0x] and [3.0x] the total equity invested at entry, plus [downside / base / upside] from the latest reforecast. For each case, also solve for the proceeds that give the fund exactly that multiple after management's share, and show both.
3. Waterfall per case: proceeds to each instrument in ranking order. Check the waterfall distributes exactly the equity proceeds.
4. Management payout per person and in total = value reaching the management pool times the vested share. Dilution to the fund = management payout divided by total equity proceeds.
5. Show the ratchet effect, if any, at the trigger point and just below it.

These figures illustrate the plan's mechanics. Tax, legal and remuneration design are referred to the fund's advisers. Decisions on awards belong to [the deal partner and the chair].
```

**Output:** A mechanics table with locators, a waterfall per case that sums, payouts per manager and dilution to the fund.

**Moody's:** `getEntityShareholders` gives registered shareholders as a cross-check on the capitalisation table. It does not show unregistered options or plan terms, and the register can sum well below 100%; list the shortfall as Missing. Report individual holders in aggregate. The issuer's credit opinion may give the sponsor and management equity split and preferred-equity terms.

**Tested:** Pure Gym group, illustrative plan on stated assumptions, November 2026. The holding company's register summed to 48.41% and listed individuals' holdings; the credit opinion gave management 11% to 14% of equity on conversion of the sponsor's co-investor's preferred equity. The waterfall summed in every case, and testing just below the ratchet trigger exposed a cliff where the ratchet pushed the fund back below 3.0x.

**Follow-ups:**
- "Show the payout for each manager if they leave as a good leaver at [date]."
- "How much of the 3x payout comes from the ratchet?"

**Adjust for:** Take-private: model the new private plan only, after public awards are settled. Growth equity: cover the option pool and its dilution across funding rounds; founders decide.

### VC-33 Succession and retention for critical roles

**Use case:** Record who could step into each critical role, where there is nobody, and what retention measures exist.

**Inputs:** VC-27 role map, management's succession list, contracts and incentive terms for critical-role holders.

**Prompt:**
```
For [Company], build the succession and retention table for critical roles. Reviewers with access: [names].

Per critical role: named successors as nominated by management, with readiness as management states it (now / within 12 months / longer) and the source; where nobody is named, write "gap". Critical-role cover = critical roles with a named ready successor divided by critical roles; flag below [80%].

Retention facts per role: notice period, restrictive covenants, unvested incentive value (from VC-32), and any retention arrangement, each with its locator. Propose retention actions for reviewers to consider.

This records facts and options. It makes no decision about any individual.
```

**Output:** A succession table with gaps, critical-role cover and retention facts with proposed actions.

**Moody's:** Not needed.

**Follow-ups:**
- "Which gaps should be filled by an external search, and when should it start?"

**Adjust for:** Buy-and-build: include add-on founders who stay, with their earn-out and retention terms. Lower-mid-market: expect the founder to hold several critical roles; list each separately.

---

## Buy-and-build

### VC-34 Add-on fit and actionability

**Use case:** Decide which add-on candidates deserve effort first.

**Inputs:** The add-on pipeline with entity identifiers, the platform's add-on criteria and disqualifiers, the fund's portfolio list, the maximum multiple for an add-on.

**Prompt:**
```
Score these add-on candidates for [Platform]: [list].

Score three axes separately, 1 to 5, with evidence; never average them:
- Fit: product, customer and geographic overlap with the thesis; earnings quality; size against criteria.
- Actionability: ownership situation and likely seller motive (from public facts, labelled as inference); whether a conversation exists; known price expectations.
- Integration difficulty: systems, sites, people, customer overlap, brand, approvals (5 is hardest).

Apply disqualifiers first: accounts that cannot be verified; revenue concentrated in a customer the platform already serves; key relationships held by a seller who is leaving; a conflict with another fund holding; a mandate exclusion.

For each scored candidate: multiple arbitrage = (assumed exit multiple minus multiple paid) times add-on EBITDA, with the exit multiple set at [the platform's entry multiple] and a case one turn lower. Revenue synergies count as zero. Before using a candidate's EBITDA, check its margin against the platform and peers and explain any outlier.
```

**Output:** A three-axis scorecard, disqualified candidates with reasons, and deal arithmetic at two exit multiples.

**Moody's:** `findEntity` on all candidates in one call, then `getEntityUltimateOwners`, `getEntitySubsidiaries` and `getEntityManagersDirectors_v2` for owner, group and directors; `getEntityFinancials_v2` where the candidate files accounts. `searchNews` with importance set to significance finds recent acquisitions by peers (the live test found a portfolio company's own add-on deal this way). For franchise brands, check whether the matched entity is the franchisor or an operator; a franchisor's accounts do not describe the estate. Ownership facts do not tell you whether the owner will sell, and ownership information dates are not acquisition dates (rule 12a): take sponsor entry dates from the adviser or registry filings. A purchase multiple quoted in Moody's research is a fact about that deal, never a benchmark for the multiple paid or the exit multiple (rule 11b).

**Tested:** four UK gym businesses, November 2026. One was not found; a franchise brand matched its franchisor (no revenue); both scored candidates were stale under rule 10, and one showed an EBITDA margin of 58.5% against peers at 37% to 43%.

**Follow-ups:**
- "Which candidates are sponsor-owned, and how long has the sponsor held them?"

**Adjust for:** Lower-mid-market: expect thin accounts and verifiability disqualifiers. Growth buyout: weigh product and market extension above cost overlap. Turnaround: out of scope until the cash test passes; say so and stop.

### VC-35 Integration plan

**Use case:** Plan day one and the first 90 days of an add-on, with one owner per item.

**Inputs:** The purchase agreement, the add-on's systems, sites, people and contracts lists, the synergy case approved at investment committee.

**Prompt:**
```
Write the integration plan for [Add-on] into [Platform].

1. A written definition of "integrated" for this deal (for example one finance close, one payroll, one CRM, one brand), to be met within [12] months.
2. Day-one list, then items for days 30, 60 and 90 across: systems, people, customers, brand, finance and legal. Each item has one named owner and a date.
3. Customer and supplier overlap between the two businesses, with the contract terms that apply.
4. Risks to revenue during integration, and how each will be watched.
5. Capacity: list integrations already running; flag if more than [2] run at once.
```

**Output:** A definition of integrated, a dated owned plan and an overlap list.

**Moody's:** `findEntity` then `getEntityUltimateOwners` on shared top customers and suppliers groups them correctly, with `getEntityRatings` or `getEntityProbabilityOfDefault_v2` for credit context. `getEntitySubsidiaries` on each supplier's parent finds its entity in each country the platform operates in, so terms can be consolidated at group level. For consumer businesses there are no shared top customers; group shared suppliers only.

**Tested:** Moody's part only, November 2026. One equipment group had subsidiaries in all four of the platform's countries, which is the useful finding for procurement integration.

**Follow-ups:**
- "Which items depend on systems still covered by a transition service agreement?"

**Adjust for:** Carve-out platforms: no integration starts on systems still under transition services. Lower-mid-market: include a first proper finance close for the add-on. Large-cap: flag merger-control filings for counsel.

### VC-36 Synergy tracker and pro forma figures

**Use case:** Track planned against realised synergies, split organic from acquired growth, and label pro forma earnings.

**Inputs:** Platform and add-on monthly accounts before and after purchase, the approved synergy case, covenant definitions.

**Prompt:**
```
Update the synergy tracker for [Platform] for [month].

1. Synergy ledger: planned (must equal the investment committee case), identified, realised (visible in closed accounts for [3] full months, with ledger lines), cost to achieve. Synergy leakage = (identified minus realised) divided by identified; flag above [20%].
2. Growth split: like-for-like growth (businesses owned throughout both periods) and acquired growth (total growth minus like-for-like). Check they sum to total growth.
3. Pro forma EBITDA = reported trailing twelve-month EBITDA, plus add-on EBITDA for months before ownership, plus run-rate synergies not yet in the accounts, each on its own line and labelled "pro forma" every time. Flag MAJOR if unrealised synergies exceed [10%] of pro forma EBITDA.
4. Check no synergy is counted in two deals or in a value-creation initiative.

End with whether the stop rule applies: integration backlog above capacity, like-for-like growth negative for two quarters, or covenant headroom below [level].
```

**Output:** A synergy ledger, a growth split that sums, a labelled pro forma bridge and a stop-rule check.

**Moody's:** `getCreditOpinion` and `searchAllDocuments` scoped to the platform's rated issuer (find it by its Bidco or Holdco name, rule 9b) give add-on prices, EBITDA, purchase multiples, pro forma leverage and covenant facts (rule 11b); a purchase multiple is a deal fact, not a benchmark. Put every pro forma line on one EBITDA basis or say which basis each uses. For rated peers, `getEntityRatings` and `getCreditOpinion` (section `KeyIndicatorsTable`) give leverage context. The executed credit agreement decides what counts in covenant EBITDA.

**Tested:** Pure Gym group with the Blink and NonStop Gym add-ons, November 2026. A Moody's issuer comment gave NonStop's price, EBITDA, 9.4x and 7.8x multiples and pro forma leverage; the annual report's pro forma 2024 revenue made a growth split that footed (£49.8m like-for-like, £88.0m acquired). The pro forma bridge mixed Moody's-adjusted and company-adjusted EBITDA until labelled. Synergy cases and the covenant limit were gaps.

**Follow-ups:**
- "Show blended entry multiple (total enterprise value paid divided by combined EBITDA) with and without realised synergies."

**Adjust for:** Growth equity and minority influence without control: run only if management asks; output is a board paper input. Buy-and-build: run monthly after each close; add-ons that miss the integrated definition are flagged for exclusion from adjusted earnings at exit (see EX-02).
