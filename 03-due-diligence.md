# Due diligence prompts

Use these once a data room is open, after the screening stage has said the deal is worth the cost of diligence. Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach the documents named under **Inputs**, fill the bracketed fields and run it.

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
| Data room and request list | DD-01 to DD-04 |
| Financial due diligence | DD-05 to DD-13 |
| Commercial due diligence | DD-14 to DD-21 |
| Management due diligence | DD-22 to DD-27 |
| Legal and contracts | DD-28 to DD-31 |
| Technology, cyber and AI exposure | DD-32 to DD-33 |
| Tax, regulatory and environmental | DD-34 to DD-35 |
| External evidence and consolidation | DD-36 to DD-38 |
| Sessions with management, experts and customers | DD-39 to DD-41 |

---

## Data room and request list

### DD-01 Index the data room

**Use case:** Know what is in the room before anyone reads it.

**Inputs:** The data-room index export, or the files themselves.

**Prompt:**
```
Here is the data room for [Company], a [deal type: platform buyout / add-on / carve-out / take-private / growth investment].

Build a source register with one row per file: reference, file name, what the document actually is (judge from its first and signature pages, not the file name), date, executed or draft, period covered, workstream (financial, commercial, legal, people, technology, tax, regulatory, environmental, other), and whether you could read it.

Then:
1. Flag duplicates and superseded versions. Executed beats draft; audited beats management; later beats earlier.
2. List files you could not read and why.
3. Map the room against a standard buy-side request list for this deal type and mark each item Received, Partial or Not in room. Before you mark anything Not in room, search the full index by counterparty name, short name and document type and say which searches you ran.

Return the register as a table, then the gap list.
```

**Output:** A source register, a duplicates list, unreadable files, and a coverage table against the standard request list.

**Moody's:** Not needed.

**Follow-ups:**
- "Which five missing items could change the price or stop the deal? Rank them and say why."
- "Write the request for each Partial item as an email paragraph to the seller's adviser."

**Adjust for:** Carve-out: add a column for whether each document belongs to the target perimeter or the parent. Take-private: add the public filings as a separate source set.

### DD-02 Build the diligence request list

**Use case:** Turn the gaps into a numbered request list the seller can work through.

**Inputs:** DD-01 output, or the data-room index; the investment thesis in a few lines.

**Prompt:**
```
Using the source register and our thesis for [Company] ([two or three sentences]), write the diligence request list.

Number each request. For each one give: workstream, the exact document or data wanted, the period, the format (for example monthly, customer level, Excel), why we need it in one line, tier (1 = could change price or stop the deal; 2 = confirmatory), and status (outstanding / partial / received).

Put tier 1 first. Write the requests so the seller cannot answer them with a summary: ask for underlying data and executed documents. Keep the list to what this deal needs, not a generic template.
```

**Output:** A numbered, tiered request list.

**Moody's:** Not needed.

**Follow-ups:**
- "Which requests would a seller usually resist, and what is the fallback ask for each?"
- "Group the tier 1 items by who on the seller's side can answer them."

**Adjust for:** Lower mid-market: ask for ledgers and bank statements, because packaged schedules may not exist. Growth equity: add capitalisation table, preference terms and existing investor rights.

### DD-03 Plan the reading order

**Use case:** Read the documents most likely to kill the deal first.

**Inputs:** The source register.

**Prompt:**
```
From the source register for [Company], propose a reading order for the first three days of diligence. Put first the documents most likely to reveal something that would stop the deal or move the price: audited accounts and auditor letters, the largest customer contracts, debt and security documents, litigation, licences, and anything the seller has described as sensitive.

For each document give: why it is early, what to look for, and which workstream owns it. End with the three questions we should be able to answer by the end of day three.
```

**Output:** A day-by-day reading order with owners.

**Moody's:** Not needed.

**Follow-ups:**
- "Split the list between two associates so they do not overlap."

**Adjust for:** Turnaround: put cash, debt maturities and creditor correspondence first.

### DD-04 Red-flag scan

**Use case:** Get a first stoplight view across the room before full diligence.

**Inputs:** The information memorandum, the data room or the key documents in it.

**Prompt:**
```
Scan the material on [Company] for red flags in these eight areas: active litigation; ownership of intellectual property; undisclosed material contracts; dependence on one or two people; restated accounts; regulatory breaches; customer concentration; debt covenant problems.

For each flag give: area, what you found, the quoted evidence with its locator, severity (CRITICAL / MAJOR / MINOR / INFO), confidence (high if the document says it directly, medium if it is implied, low if inferred), and the question it raises.

Then give an overall signal: RED if any CRITICAL flag, or any MAJOR flag with high confidence; AMBER if any other MAJOR flag; GREEN otherwise. "No flags" is an acceptable answer in an area. Do not invent concerns to look thorough.
```

**Output:** A flag table and an overall stoplight.

**Moody's:** `findEntity` then `getShellCompanyIndicators` on the target and its parent adds corporate-structure indicators (mass registrations, circular ownership, dormancy, director flags); treat director flags as INFO unless another category is also found (standing rule 13). `searchNews` by entity with litigation and regulatory keywords over the past 365 days (standing rule 12b); few results is weak evidence of absence, so say so. If the group has a rated issuer (standing rule 9b), `getCreditOpinion` with `FactorsLeadingToDowngrade` and `CreditChallenges` gives the leverage and cover context for the debt covenant area.

**Tested:** On a UK gym group the only material flag came from the rated borrower's credit opinion (leverage near the downgrade trigger, weak interest cover) combined with a debt-funded acquisition announced after it. Director flags (14 of 17) and a one-item news result added little.

**Follow-ups:**
- "For each RED or AMBER flag, what document would confirm or clear it?"

**Adjust for:** Regulated sectors: add licence status as a ninth area.

---

## Financial due diligence

### DD-05 Tie out the numbers

**Use case:** Make sure every set of figures in the room agrees before analysing any of them.

**Inputs:** Management accounts, audited accounts, trial balance or ledger, tax returns, the information memorandum.

**Prompt:**
```
For [Company], reconcile revenue, gross profit, EBITDA and net debt across every source in the room for each period: information memorandum, management accounts, audited accounts, trial balance, and tax returns.

Build one table per measure: rows are periods, columns are sources, with a difference column against the audited figure. Any difference above [1%] gets a line explaining it, or goes on the Contradicted list. Tell me which source we should treat as the base and why.
```

**Output:** Reconciliation tables and a stated base source.

**Moody's:** `getEntityFinancials_v2` (reported basis) adds an external record of filed figures for private companies that file accounts. Put it in its own column and compare; do not overwrite the room's figures with it. State the EBITDA definition each source uses before comparing. Moody's EBITDA is EBIT plus depreciation and amortisation on the accounts' own basis (post IFRS 16 for IFRS filers) and will not equal management's adjusted EBITDA. Treat Moody's line labels and interim-period balance sheet lines as classifications to check, not facts.

**Follow-ups:**
- "Show me every difference above the threshold with both source locators side by side."

**Adjust for:** Carve-out: reconcile carve-out accounts to the parent's segment disclosure instead of to audited target accounts.

### DD-06 Quality of earnings

**Use case:** Rebuild reported earnings into the figure the fund will stand behind.

**Inputs:** Management's adjusted EBITDA schedule, monthly profit and loss, ledger detail where available, the information memorandum.

**Prompt:**
```
Build the quality-of-earnings bridge for [Company] for [years and last twelve months].

Show three rows: reported EBITDA, management-adjusted EBITDA, and fund-accepted EBITDA. State whether each row is before or after rent (IFRS 16 or pre-IFRS 16), and keep one basis for the bridge, net debt and price. Treat share-based payment add-backs as an adjustment to grade, not as a given. List every management adjustment and classify it as one of: non-recurring item, owner or related-party cost, accounting-policy normalisation, or run-rate / pro forma. Grade each one Supported (documented and genuinely one-off), Reasonable (plausible, partly documented) or Unsupported, with a one-line reason and its source.

Rules: reject any "one-off" that appears in more than one year; accept a run-rate adjustment only if the event has already happened; never include future synergies; look for one-off income that management did not remove (asset sales, insurance receipts, provision releases, grants) and remove it.

End with the gap between management-adjusted and fund-accepted EBITDA, in amount and percentage.
```

**Output:** A three-row bridge, an adjustment schedule with grades, and the gap.

**Moody's:** Where the company or its group's issuer is rated (standing rule 9b), `getCreditOpinion` with `KeyIndicatorsTable` shows Moody's adjusted EBITDA for comparison. Treat it as a third view, not as the answer. If the section returns no key indicators, take the figures from the Summary text or mark them [GAP] (standing rule 11a).

**Follow-ups:**
- "Rebuild fund-accepted EBITDA assuming every Reasonable item is rejected. What does that do to the entry multiple?"
- "Which adjustments would a lender accept under a typical credit agreement definition, and which would they cap?"

**Adjust for:** Lower mid-market: expect owner costs (salaries, cars, family members) and test each against a market replacement cost. Carve-out: add a standalone cost adjustment for services the parent provided.

### DD-07 Revenue quality

**Use case:** Understand how much of the revenue will recur and how it is recognised.

**Inputs:** Revenue by customer, product and month; contracts or terms; revenue recognition policy.

**Prompt:**
```
Analyse revenue quality for [Company] over [period].

1. Split revenue into recurring under contract, recurring without contract, repeat but not contracted, and one-off. Show the share of each by year.
2. Check recognition against contract terms for the ten largest contracts: is revenue recognised when the contract says it is earned?
3. Look for cut-off problems: revenue spikes in the last month of quarters or years, credit notes issued after period end, unusually long payment terms near year end.
4. Show gross margin by product and by the top 20 customers if the data allows.

Flag anything that suggests revenue is being pulled forward or that recurring revenue is overstated.
```

**Output:** A revenue mix table, a recognition check, cut-off tests and margin by product and customer.

**Moody's:** `getEntitySegmentFinancials` gives disclosed segment splits for companies that report them, which can be compared with the room's split.

**Follow-ups:**
- "Show the monthly revenue chart with the quarter-end months highlighted."

**Adjust for:** Software and subscription: report annual recurring revenue and reconcile it to recognised revenue. Project businesses: report backlog and its conversion history.

### DD-08 Net working capital and the peg

**Use case:** Set the working-capital level that must be left in the business at closing.

**Inputs:** Monthly balance sheets for at least 24 months.

**Prompt:**
```
For [Company], build monthly net working capital for the last [24] months: trade receivables, inventory, prepayments and other current assets, less trade payables, accruals and other current liabilities. Exclude cash, debt and debt-like items, and say where you put each line.

Show the monthly series, the average over the last 12 months, the seasonal pattern, and any month that looks managed (for example payables stretched just before a period end).

Propose a peg as a range with the method behind each end. List any items whose classification is arguable and how they move the peg.
```

**Output:** A monthly series, seasonality view and a proposed peg range.

**Moody's:** `getEntityPeers` then `getEntityFinancials_v2` on each peer gives receivable, payable and deferred income levels to compare with, labelled as external benchmarks. Screen the peer list first (standing rule 12a). Compare balances as a share of revenue, not Moody's A/P days, which divide by cost of goods sold and mislead for service businesses.

**Follow-ups:**
- "Recalculate the peg excluding the three most unusual months."

**Adjust for:** Seasonal businesses: insist on a full 12-month average, never a point in time.

### DD-09 Net debt and debt-like items

**Use case:** Find everything that should come off the price.

**Inputs:** Balance sheet, notes to the accounts, debt agreements, leases, tax computations, pension reports.

**Prompt:**
```
List every net debt and debt-like item for [Company] at [date]: borrowings, accrued interest, leases, deferred and contingent consideration from past acquisitions, unpaid tax, pension deficits, customer deposits and deferred revenue that will need cash to deliver, transaction bonuses, unpaid capital expenditure, aged payables, provisions, and any other item a buyer would argue reduces equity value.

For each: amount, source, whether it is clearly debt, arguably debt, or working capital, and how it should be handled in the purchase agreement (deducted from price, specific indemnity, escrow, or covered by the peg).
```

**Output:** A schedule of items with a proposed treatment.

**Moody's:** For rated targets or a rated issuer in the group (standing rule 9b), `getCreditOpinion` shows Moody's adjusted debt, which often includes leases and pensions, as a cross-check. Do not substitute figures labelled "Adjusted" in `getEntityFinancials_v2` (standing rule 11a).

**Follow-ups:**
- "Which items will the seller dispute, and what is our evidence for each?"

**Adjust for:** Turnaround: add overdue creditors and arrears to tax authorities.

### DD-10 Cash conversion and capital expenditure

**Use case:** Test whether profit turns into cash, and what spending the forecast leaves out.

**Inputs:** Cash flow statements, fixed asset register, capital expenditure detail.

**Prompt:**
```
For [Company] over [period]:
1. Calculate cash conversion as operating cash flow before tax and interest divided by EBITDA, by year, and explain any year below [80%].
2. Split capital expenditure into maintenance and growth, with the basis for the split. Compare maintenance spending with depreciation of owned assets (exclude right-of-use depreciation under IFRS 16).
3. Look for deferred spending: ageing assets, falling maintenance spend as a share of revenue, or repair backlogs mentioned in board minutes.

Say what free cash flow looks like if maintenance spending returns to [its historical level / depreciation].
```

**Output:** Cash conversion by year, a capital expenditure split and an adjusted free cash flow view.

**Moody's:** Peer cash flow ratios through `getEntityFinancials_v2` with sections `all_ratios`. Moody's CAPEX / depreciation includes right-of-use depreciation for IFRS 16 filers; recompute on owned assets before comparing.

**Follow-ups:**
- "Which assets will need replacing in the next three years, and what will that cost based on the register?"

**Adjust for:** Asset-light services: focus on capitalised development or software costs instead of physical assets.

### DD-11 Test the forecast

**Use case:** Decide how much of management's plan to believe.

**Inputs:** Management's forecast, current-year budget and year-to-date actuals, past budgets.

**Prompt:**
```
Test [Company]'s forecast for [years].

1. Compare each of the last [three] budgets with what actually happened: revenue and EBITDA, amount and percentage. Is management usually optimistic, accurate or conservative?
2. Compare year-to-date actuals with this year's budget and project the full year on current run-rate.
3. Build a bridge from the last actual year to the first forecast year: volume, price, new customers, new products, cost savings, acquisitions. For each driver, say what evidence in the room supports it.
4. Flag any acceleration in growth or margin that has no named initiative behind it.
```

**Output:** A budget accuracy history, a current-year outturn and a forecast bridge with evidence grades.

**Moody's:** `searchAllDocuments` with the sector and region in the filter and recent research only, for Moody's sector growth expectations as an external reference point. If the sector search returns no relevant research, search for credit opinions on rated competitors in the same market (`searchAllDocuments` with the competitor's entity, or keywords naming it); quote their reported growth as a competitor reference, not a sector forecast.

**Follow-ups:**
- "Build a downside case that applies management's historical forecasting error to this plan."

**Adjust for:** Growth equity: test the hiring plan against the revenue plan, since growth usually depends on sales hires ramping on time.

### DD-12 Financial red flags

**Use case:** One pass for the financial issues that most often hurt buyers.

**Inputs:** The financial material in the room.

**Prompt:**
```
Review the financial material on [Company] for: restatements; changes in auditor or accounting policy; qualified audit opinions or emphasis-of-matter paragraphs; related-party transactions; unusual journal entries near period ends; capitalised costs rising faster than revenue; growing receivables relative to revenue; rising credit notes; and adjustments that only ever increase earnings.

For each finding: evidence with locator, severity, and the question for management.
```

**Output:** A financial red-flag table.

**Moody's:** `getShellCompanyIndicators` flags abnormal revenue per employee and dormancy history at the entity level.

**Follow-ups:**
- "Draft the questions for the finance director, most sensitive last."

**Adjust for:** Founder-owned targets: look closely at personal expenses run through the company.

### DD-13 Financial diligence summary

**Use case:** Write the financial diligence report from the work above.

**Inputs:** Outputs of DD-05 to DD-12.

**Prompt:**
```
Write the financial due diligence summary for [Company] for the deal team and investment committee.

Sections: headline view in five lines; earnings quality with the three-row bridge; revenue quality; working capital and proposed peg; net debt and debt-like items; cash conversion and capital expenditure; forecast reliability; red flags; open items with the request that would close each.

Every number must trace to the earlier analysis. Mark clearly where an accounting firm's signed report is still needed for lenders or insurers.
```

**Output:** A structured financial diligence report.

**Moody's:** Include the external corroboration table from DD-36 as an appendix. If DD-36 has not been run, include a short table of the Moody's evidence already used in DD-05 to DD-12 (entity match, filed financials and period, peers, corporate-structure indicators, any rating), each with its tool and date.

**Follow-ups:**
- "Turn the open items into a two-page list for the seller."

**Adjust for:** For the investment committee, cut to two pages and lead with the effect on price.

---

## Commercial due diligence

### DD-14 Commercial hypotheses

**Use case:** Decide what commercial diligence has to prove before spending on it.

**Inputs:** The information memorandum, our thesis.

**Prompt:**
```
For [Company], turn our investment thesis into the five to eight commercial questions diligence must answer. For each question: what the seller claims (quoted, with locator), what would have to be true for our price to work, what evidence would confirm or disprove it, and where we will get that evidence (data room, customer calls, expert calls, external data).

Rank the questions by how much a wrong answer would change the price.
```

**Output:** A ranked set of commercial hypotheses with an evidence plan.

**Moody's:** None at this step.

**Follow-ups:**
- "Which of these could we answer in a week with the data already in the room?"

**Adjust for:** Thematic funds: add a question on how much revenue depends on the theme and what happens if adoption is five years late.

### DD-15 Size the served market

**Use case:** Replace the seller's market story with a defensible estimate.

**Inputs:** The information memorandum's market section, any market studies in the room, company sales data.

**Prompt:**
```
Test the market claims for [Company].

1. Quote every market-size and growth claim in the material with its source, and classify each as sourced, unsourced or circular (citing a study the seller commissioned).
2. Build our own estimate of the market the company actually serves (its products, customer types and geographies), two ways: top-down from total market figures narrowed step by step, and bottom-up from number of potential customers times spend per customer. Show every assumption and label each as sourced or estimated.
3. Reconcile the two. If they differ by more than [15%], explain why and say which you trust more.
4. Give the company's implied share of the served market.
```

**Output:** A claims table, two market estimates, a reconciliation and implied share.

**Moody's:** `searchAllDocuments` (filter by sector and region, recent research) and `getEntityCountryProfile` give sector and macro context. Moody's does not supply market size; say so if asked. Credit opinions on rated competitors (`searchAllDocuments` naming the competitor) often give members, revenue per customer and site counts, which can feed the bottom-up estimate; label them as competitor data.

**Follow-ups:**
- "What growth rate does management's plan imply for market share, and is that plausible?"

**Adjust for:** Buy-and-build: also count the number of companies in the niche and their size spread, as evidence of fragmentation.

### DD-16 Customer concentration and retention

**Use case:** Find out how dependent revenue is on a few customers and whether customers stay.

**Inputs:** Revenue by customer by month or year, for at least three years.

**Prompt:**
```
Using customer-level revenue for [Company]:

1. Concentration: share of revenue from the top 1, 5, 10 and 20 customers each year, and any customer whose revenue moved by more than [25%] year on year.
2. Retention: build a cohort table with customers grouped by the year they started, showing revenue in each later year as a percentage of the first year. Compare cohorts at the same age.
3. Gross revenue retention (revenue kept from existing customers, ignoring growth) and net revenue retention (including growth from existing customers), by year. Show both; one without the other hides churn.
4. Customers lost in the period: how many, how much revenue, and any reason given in the room.

Do not draw a retention conclusion from fewer than three complete cohorts; show the data and say it is too early.
```

**Output:** Concentration tables, a cohort table, retention rates and lost-customer analysis.

**Moody's:** `findEntity` on the named top customers, up to ten (one call), then `getEntityRatings` on each (state the rating type) and `getEntityProbabilityOfDefault_v2` with `early_warning`, shown side by side (standing rule 11). A concentrated customer with weak credit is a bigger risk than concentration alone.

**Tested:** Without a data room, public filings name only customers above the 10% disclosure threshold. Convert their shares to amounts with the filed revenue, cite the filing by document and page, and mark cohorts, retention and lost customers [GAP]. On a US auto supplier this gave three customers over three years; cohort and retention parts stayed empty, as they should.

**Follow-ups:**
- "What happens to EBITDA if the largest customer leaves? Show it with the gross margin on that customer."

**Adjust for:** Growth equity: add logo retention and expansion revenue by cohort.

### DD-17 Competition and pricing power

**Use case:** Understand where the company wins and loses, and whether it can raise prices.

**Inputs:** Competitor information in the room, pricing history, win and loss data if any, customer contracts.

**Prompt:**
```
For [Company]:
1. List competitors in tiers (direct, adjacent, substitutes) with the evidence for each.
2. From any win and loss data, show win rate by competitor and the reasons given.
3. Pricing: list every price increase in the last [three] years, the increase announced versus the increase actually achieved, and customer losses in the following quarters.
4. Decompose revenue growth into price, volume and mix if invoice data allows.
5. Assess substitution risk, including from AI: could a cheaper tool, a customer's in-house team or an AI product replace what the company sells?

End with a one-paragraph view on pricing power, with the evidence for it.
```

**Output:** A competitor map, win and loss view, pricing history and a pricing-power view.

**Moody's:** `getEntityPeers` then `getEntityFinancials_v2` on each gives competitors' margins and growth where they file accounts. Run `getEntityPeers` on the operating company, not the rated Bidco, and filter the list (standing rule 12a). For pricing evidence, use `searchCompanyFilings` (annual, pass `reportYears`) on the company's or its issuer's annual report and on listed peers, and the credit opinion for Moody's view of competition.

**Tested:** On a UK gym group, peers of the rated Bidco were holding companies in other sectors (software, business services); peers of the operating company gave four usable gym operators. The issuer's annual report gave average revenue per member and the drivers of revenue growth, enough for a price and volume split; announced price increases, achieved increases and churn after each were not public.

**Follow-ups:**
- "Which competitor is most likely to be bought by a well-funded owner, and what would that change?"

**Adjust for:** Specialist funds: compare against the competitors in our own portfolio first.

### DD-18 Price, volume and mix bridge

**Use case:** Separate how much growth came from price, from selling more, and from selling a different mix.

**Inputs:** Invoice lines with date, customer, product, units and net revenue for two periods.

**Prompt:**
```
Build a price-volume-mix bridge for [Company] between [period 0] and [period 1] in a table, with the calculation shown.

For products sold in both periods: price effect = sum of (price now minus price before) times units now; volume effect = (total units now minus total units before) times average price before; mix effect = sum of (price before times units now) minus (average price before times total units now). Add new products and lost products as separate lines. Remove currency effects first and show them separately.

Check that price + volume + mix + new + lost equals the total revenue change exactly. If units are not comparable across products, report volume and mix combined and say why.
```

**Output:** A bridge table that sums to the total change.

**Moody's:** Not needed.

**Follow-ups:**
- "Repeat the bridge by customer segment."

**Adjust for:** Services businesses: use hours or headcount as the volume unit.

### DD-19 Test the growth plan

**Use case:** Decide what growth to underwrite.

**Inputs:** Management's plan, outputs of DD-15 to DD-18.

**Prompt:**
```
Break [Company]'s plan for [years] into its drivers: market growth, share gain, price, new customers, new products, new geographies and acquisitions. For each driver show the amount it contributes, the evidence for it from our diligence, and an evidence grade (strong / partial / none).

Then build three cases: management, base (only drivers with strong or partial evidence), and downside ([market slows by X points; price flat; top customer lost]). Show revenue and EBITDA for each.
```

**Output:** A driver bridge with evidence grades and three cases.

**Moody's:** Sector expectations from `searchAllDocuments`, labelled as context. For a rated company, the credit opinion's key indicators table carries Moody's forecasts (revenue, margin, leverage); show them as an external case, never as management's plan, and compare earlier opinions' forecasts with what happened.

**Tested:** Without a management plan, the driver bridge used public figures and Moody's forecasts, labelled as such. Moody's forecast of 4.8x leverage for 2026 in a November 2025 opinion became 6.2x in October 2026 after a refinancing, which is the kind of slippage the downside case should test.

**Follow-ups:**
- "What would we need to see in the first year to believe the management case?"

**Adjust for:** Growth buyout: the base case carries most of the entry price, so test the sales capacity behind it (see VC-25 in `06-value-creation.md`).

### DD-20 Customer reference programme

**Use case:** Plan calls with customers that test the thesis, not confirm it.

**Inputs:** The customer list with revenue and tenure; the commercial hypotheses.

**Prompt:**
```
Plan a customer reference programme for [Company].

1. Choose [eight to ten] customers across size, tenure, product and geography. Include at least one customer that left or reduced spend, and source at least a third independently of management.
2. Write a 30-minute interview guide in five parts: how they chose the company, the value they get, how hard it would be to switch, satisfaction and likelihood to recommend, and competitors they considered.
3. For each commercial hypothesis, note which questions test it.

Questions must be open and neutral.
```

**Output:** A reference list with rationale and an interview guide mapped to hypotheses.

**Moody's:** Not needed.

**Follow-ups:**
- "Write a shorter 15-minute version for smaller customers."

**Adjust for:** Business-to-consumer: replace reference calls with a survey and review-site analysis.

### DD-21 Commercial diligence summary

**Use case:** Write the commercial diligence report.

**Inputs:** Outputs of DD-14 to DD-20 and any call syntheses.

**Prompt:**
```
Write the commercial due diligence summary for [Company].

Sections: headline view; answer to each commercial hypothesis with its evidence grade; market; customers, concentration and retention; competition and pricing power; growth plan and the three cases; risks to the plan; questions still open.

Keep the seller's claims, our evidence and our conclusions visibly separate.
```

**Output:** A structured commercial diligence report.

**Moody's:** Add the external evidence appendix from DD-36.

**Follow-ups:**
- "Summarise this in one page for the investment committee."

**Adjust for:** None.

---

## Management due diligence

### DD-22 Map the roles from the record

**Use case:** Confirm who runs the business and whether the record matches what we were told.

**Inputs:** The information memorandum's management section, CVs or biographies, organisation chart.

**Prompt:**
```
For [Company], build a table of the leadership team: name, role, start date, reporting line, previous roles as stated, and whether each person is a shareholder.

Compare what the information memorandum says with the CVs and the organisation chart, and with the Moody's directors record if available. List every discrepancy in titles, dates or roles.

This is a factual record. Do not assess anyone's ability.
```

**Output:** A leadership table and a discrepancy list.

**Moody's:** `findEntity` on the target and its top holding company, then `getEntityManagersDirectors_v2` on both (with `includeDetails` set to true when start dates are needed; do not reproduce dates of birth). The record is the registered board, not the management team. It can be incomplete (a test returned only the chief executive for a large company), may list only "Director" with no executive titles, and after a take-private may still show listed-company roles such as audit committee members. Report it with its confirmation date and treat gaps as gaps.

**Follow-ups:**
- "Which roles in the organisation chart have no named holder or an interim?"

**Adjust for:** Carve-out: mark which leaders are transferring from the parent and which roles the parent currently performs.

### DD-23 Verify the track record

**Use case:** Tie management's claimed achievements to evidence.

**Inputs:** Management presentation, information memorandum, financial data.

**Prompt:**
```
List every achievement the management of [Company] claims (growth delivered, margin improved, launches, acquisitions integrated, turnarounds). For each one: who claims it, the claim quoted, the evidence in the room that supports or contradicts it (with numbers and locators), and a status of Confirmed, Partly confirmed, Contradicted or Asserted.
```

**Output:** A claims table with status.

**Moody's:** `getEntityFinancials_v2` on companies the managers previously ran (resolve them in one `findEntity` call) can test claims about past roles; this needs CVs naming those companies. Claims about the target itself can be tested against filed accounts (`getEntityFinancials_v2`, `searchCompanyFilings` on the annual report) and, for a rated issuer, against the figures quoted in credit opinions.

**Tested:** With no CVs the past-role step could not run. Testing the target's own published KPIs against its accounts worked and found a restated prior-year member figure that does not foot to revenue.

**Follow-ups:**
- "Which claims should we raise in references?"

**Adjust for:** None.

### DD-24 Key-person and owner dependence

**Use case:** Find out what breaks if one person leaves.

**Inputs:** Customer contracts, supplier agreements, licences, organisation chart, management interviews.

**Prompt:**
```
For [Company], identify who personally holds the relationships and knowledge the business depends on: the largest customers, critical suppliers, regulators and licences, key technology, and the sales pipeline.

For each dependency: the person, the evidence, what would happen if they left, and how to reduce the risk (contract terms, handover, retention, insurance). Flag hard if the founder is the named contact on the largest contracts.
```

**Output:** A dependency map with mitigations.

**Moody's:** Not needed.

**Follow-ups:**
- "Which of these should be a condition before signing?"

**Adjust for:** Lower mid-market founder-owned: this is usually the biggest risk in the deal; give it its own section in the committee paper.

### DD-25 Organisation and bench against the plan

**Use case:** Check the team can deliver the plan we are underwriting.

**Inputs:** Organisation chart, the value-creation hypotheses, the forecast.

**Prompt:**
```
Compare [Company]'s current organisation with what our plan needs over the next [24] months. List the roles the plan requires, whether each is filled, the depth beneath each leader, and gaps that need hiring. Pay particular attention to the finance function: can it produce monthly accounts, a budget and a covenant compliance certificate on time?

Rank the gaps by how much they put the plan at risk.
```

**Output:** A role-gap table ranked by risk.

**Moody's:** Not needed.

**Follow-ups:**
- "Which gaps must be filled before closing, and which in the first 100 days?"

**Adjust for:** Carve-out: list every function the parent provides today and when each must stand alone.

### DD-26 Incentives and alignment

**Use case:** Understand what management will gain and lose from the deal.

**Inputs:** Employment contracts, equity plans, shareholder agreements, the proposed management equity terms.

**Prompt:**
```
For [Company], summarise for each senior manager: current shareholding, rollover proposed, existing options and their treatment on change of control, change-of-control payments, notice periods, and restrictive covenants (non-compete, non-solicit) with their length.

Flag anyone who receives a large payout at closing and has little rolled equity, and anyone with no restrictive covenants.
```

**Output:** A per-person table with flags.

**Moody's:** Not needed.

**Follow-ups:**
- "Model what each manager receives under the proposed equity plan at a 2x and 3x exit." (Use VC-32 in `06-value-creation.md`.)

**Adjust for:** Minority deals: focus on alignment with the controlling shareholder.

### DD-27 Integrity facts

**Use case:** Collect the factual public record on the company's ownership, officers and corporate-structure indicators, and say what needs another source.

**Inputs:** The target's registered name and country; any names of directors, senior managers and owners above [10%] from the seller.

**Prompt:**
```
Build the integrity fact base for [Company] (registered name [legal name], country [country]) from the Moody's record and news. Facts with sources only.

1. Ownership chain: direct shareholders, ultimate owners (threshold 25.01) and beneficial owners, with percentages and information dates. Where the chain stops at a vehicle, resolve the vehicle by its BvD ID and list its shareholders. Re-add the percentages at each level. Count individual holders below [1%] rather than naming them.
2. Coherence check on the chain. Flag, as a question for the registry, any of: a trustee, security agent or nominee as sole shareholder; an owner far smaller than the group it owns (by employees or age); an ultimate owner that differs from the sponsor named by the seller or in news; owners listed with no percentage. Do not resolve the question yourself.
3. Officers: current directors and officers of the company and its top holding company, with role, start date, nationality and source date. Compare them with the names the seller gave and list differences in names, roles or dates.
4. Corporate-structure indicators: the counts and categories returned (flagged directors, atypical and dissolved directorships, mass registration, circular ownership, jurisdictional disparity, dormancy, financial anomalies). Report them as counts. Treat director flags as INFO unless another category is also found.
5. Adverse news on the company and its top holding company over the past [365] days: litigation, regulatory action, insolvency, fraud. Quote each relevant item with its date.

Then a table: area, what the record shows, source, what it cannot show.

End with a section "Needs another source", listing at least: the names and companies behind flagged directors and each person's directorship and insolvency history (Companies House officer search or a licensed provider); sanctions and politically exposed person screening (licensed screening service); litigation searches against the company and individuals (court records); links between people and customers or suppliers.

Do not draw conclusions about any person's character or conduct, and do not search news on private individuals by name. State that this is not sanctions or politically exposed person screening.
```

**Output:** An ownership chain with percentages that foot, a coherence check, an officer list against the seller's list, indicator counts, adverse news, and a "Needs another source" section.

**Moody's:** `findEntity` (with alternatives) on the company; `getEntityShareholders`, `getEntityUltimateOwners` (threshold 25.01) and `getEntityBeneficiaryOwners` (threshold 1010) for step 1; `findEntity` by `bvdId` for vehicles found in the chain (a name search for a vehicle returned a different company in a test; shareholder results carry no BvD ID, so take it from the ultimate-owner or beneficial-owner results); `getEntityManagersDirectors_v2` with `includeDetails` set to true on the company and its top holding company for step 3; `getShellCompanyIndicators` on both for step 4 (counts only, never names: standing rule 13); `searchNews` by entity with keywords for step 5 (standing rule 12b). Do not reproduce dates of birth.

**Tested:** The first version asked for each person's directorships, failed companies and litigation, which the connector cannot supply, and failed. On one UK restaurant group the record showed a security trustee holding 100% directly and a 2021 holding company with 435 employees as ultimate owner of a 170-company group: the coherence check in step 2 exists to catch this. Director flags ran at 14 of 17 to 79 of 92 across four groups, so they are INFO on their own. The redesigned prompt, run on a UK gym group, gave a chain whose holding-company register summed to 48.41%, an undefined co-investor position and three beneficial owners with no percentages: each became a registry request.

**Follow-ups:**
- "Draw the ownership chain from the target to the ultimate owners as a list with percentages."
- "Write the registry and screening requests that would close each item in 'Needs another source'."

**Adjust for:** Cross-border: note jurisdictions with limited public records.

---

## Legal and contracts

### DD-28 Contract review grid

**Use case:** Pull the same terms out of many contracts at once.

**Inputs:** Customer, supplier and partner contracts.

**Prompt:**
```
Review the contracts for [Company] as a grid: one row per contract, one column per question. Columns: counterparty; contract type; start and end date; renewal mechanism and notice period; annual value if stated; change of control (none / notice only / consent required / termination right / automatic termination / applies only if buyer is a competitor); assignment restriction; exclusivity; most-favoured pricing; termination for convenience (who holds it, notice period); liability cap; governing law.

For each cell give the value and its clause reference. Write "Not found" when you searched and it is not there, and "N/A" when it does not apply. Quote ambiguous wording exactly.

Then list every contract where change of control needs consent or allows termination, with the revenue at stake.
```

**Output:** A contract grid and a change-of-control exposure list.

**Moody's:** Not needed.

**Follow-ups:**
- "Add up the revenue under contracts with a termination right on change of control, including small contracts with the same term."

**Adjust for:** Asset deal: add whether each contract can be assigned and whether consent is needed.

### DD-29 Covenants that could bind the buyer

**Use case:** Check whether the target's contracts restrict our other portfolio companies after closing.

**Inputs:** Contract grid from DD-28; list of the fund's portfolio companies and their sectors.

**Prompt:**
```
From [Company]'s contracts, find every exclusivity, non-compete, non-solicit and most-favoured pricing clause. For each, read the definition of "Affiliate" or "Group" and say whether, after closing, it could apply to the fund or to our other portfolio companies: [list with sectors].

Rate each as Applies, Could apply (depends on interpretation), or Does not apply, quote the definition, and say which portfolio company would be affected.
```

**Output:** A buyer-group exposure table.

**Moody's:** Not needed.

**Follow-ups:**
- "Draft the questions for counsel on the Could apply items."

**Adjust for:** Large-cap funds with many holdings: run it against the full portfolio list.

### DD-30 Debt, security and litigation

**Use case:** Confirm what is owed, what is pledged and what is being disputed.

**Inputs:** Credit agreements, security documents, lien searches, litigation schedule, board minutes.

**Prompt:**
```
For [Company]:
1. List every debt facility with lender, amount, maturity, security, guarantees and change-of-control terms, and reconcile it to the balance sheet.
2. List every registered charge or lien and match it to a facility. Flag any charge with no matching facility.
3. List every current, threatened or recent (five years) dispute with amount claimed, status and any provision in the accounts. Flag claims with no provision.
```

**Output:** A debt schedule, a charges reconciliation and a litigation table.

**Moody's:** `searchNews` on the company over the past 365 days for disputes; `getEntitySubsidiaries` to confirm which group companies are obligors. For a rated issuer, `searchAllDocuments` scoped to it returns rating actions, credit opinions and covenant snapshots that give facilities, maturities, security, guarantors and covenants; `searchCompanyFilings` (annual) gives the borrowings and provisions notes. Moody's does not supply registered charges or court records.

**Tested:** On a UK sponsor-owned group the debt schedule, security package, guarantor list and covenant came from rating actions, a covenant snapshot and the issuer's annual report. `getEntitySubsidiaries` on the issuer listed only one of 26 group companies, so it cannot confirm obligors. Two news searches over 365 days found no disputes; a nil legal provision came from the accounts.

**Follow-ups:**
- "Which items need payoff letters or releases at closing?"

**Adjust for:** Turnaround: add creditor correspondence and any standstill agreements.

### DD-31 Intellectual property chain of title

**Use case:** Confirm the company owns what it sells.

**Inputs:** IP register, employment and contractor agreements, licence agreements, open-source inventory.

**Prompt:**
```
For [Company], confirm ownership of its key intellectual property: registered rights and their status, assignment clauses in employment and contractor agreements (especially founders and early developers), licences in and out, and open-source components with obligations that could force disclosure of source code.

List each gap with the asset affected, the evidence and the fix.
```

**Output:** An ownership table and a gap list with fixes.

**Moody's:** Not needed.

**Follow-ups:**
- "Which gaps should be fixed before closing as a condition?"

**Adjust for:** Software targets: give this its own section in the committee paper.

---

## Technology, cyber and AI exposure

### DD-32 Technology and cyber review

**Use case:** Check the technology can carry the plan and is reasonably secure.

**Inputs:** Architecture documents, security certifications, penetration test reports, incident log, IT budget.

**Prompt:**
```
Review [Company]'s technology:
1. Main systems and components, flagging anything end-of-life or unsupported.
2. Could the platform support twice today's volume? What would have to change?
3. Security: certifications and their expiry dates, last penetration test and open findings, multi-factor authentication for privileged access, incident history including any breach involving customer data.
4. Data protection: processor agreements with key suppliers, cross-border data transfers and their legal basis.
5. Technical debt and the cost to fix it, only where the documents support a figure.
```

**Output:** A technology and security findings table.

**Moody's:** Not needed.

**Follow-ups:**
- "What should the 12-month technology budget include that the current one does not?"

**Adjust for:** Carve-out: list systems shared with the parent and the transition services needed.

### DD-33 Exposure to AI

**Use case:** Decide whether AI helps or threatens the business.

**Inputs:** Description of products, customers, cost base.

**Prompt:**
```
Assess [Company]'s exposure to AI. Classify it as one of: improves the business (AI lowers costs or improves the product without changing the model); forces change (big upside, but the company must change how it works); threatens the model (AI could make the core product or service unnecessary).

Answer five questions with evidence: Could AI remove the need for what customers buy? How would volumes and prices change? Does AI let new competitors in? How could AI improve the offer? Where in the cost base could AI save money?

End with what this means for the exit multiple a buyer would pay in [year].
```

**Output:** A classification, five answers and an exit implication.

**Moody's:** `searchAllDocuments` with the sector filter for Moody's research on technology disruption in the sector. If the results are generic, read the credit opinions of the company and of rated sector peers, which discuss digital substitutes and technology in the operating model.

**Tested:** For a gym operator the sector-filtered search returned mostly hyperscaler and country AI research; one rated US fitness operator's credit opinion named fitness apps and free online content as competition. Moody's supplies no exit multiples, so the exit implication is qualitative.

**Follow-ups:**
- "Which management team members could lead an AI programme?"

**Adjust for:** None.

---

## Tax, regulatory and environmental

### DD-34 Tax and regulatory issues for advisers

**Use case:** Prepare the questions for tax and regulatory counsel.

**Inputs:** Tax returns and computations, licences, regulator correspondence.

**Prompt:**
```
For [Company], list issues for tax and regulatory advisers to confirm:
1. Tax: open years and audits, losses and whether a change of ownership restricts them, transfer pricing documentation, indirect tax registrations, any exposure without a provision.
2. Licences and permits: which ones the business needs, whether each transfers on change of control or needs consent or re-application, and expiry dates.
3. Merger control: jurisdictions where a filing may be needed and the effect on timetable.

This is a list of questions for advisers, not advice.
```

**Output:** An issues list by area for advisers.

**Moody's:** `getEntityCountryProfile` for country context on each jurisdiction. It returns the sovereign's credit opinion, not tax rules, licensing or merger-control thresholds. Tax facts (losses, restricted interest, deferred tax) come from the annual report through `searchCompanyFilings`.

**Tested:** For a UK group with Danish and Swiss subsidiaries, the tool returned the UK and Denmark sovereign opinions and failed for the Swiss company. The annual report's tax note gave unrecognised losses, restricted interest carried forward and the deferred tax asset timeline.

**Follow-ups:**
- "Which items could delay signing or closing?"

**Adjust for:** Regulated sectors: add regulator approval of the new owner as a separate line.

### DD-35 Environmental and social review

**Use case:** Find environmental and social liabilities and disclosure duties.

**Inputs:** Site list, permits, environmental reports, health and safety records, sustainability reports.

**Prompt:**
```
For [Company], list: sites and any known contamination or remediation; environmental permits and enforcement history; health and safety incidents; emissions data if reported; and sustainability reporting the company will be required to produce. For each issue give the evidence, possible cost if stated, and severity.
```

**Output:** An environmental and social issues table.

**Moody's:** `getEntityEsg` on the target and on the group's rated issuer if there is one (standing rule 9b); an unrated operating company may sit under a rated Bidco whose credit opinion carries ESG scores. Rated companies return Moody's ESG credit scores (the Credit Impact Score and E, S and G scores, which are not ratings) with analyst commentary, plus reported emissions. Unrated companies may still return reported Scope 1, 2 and 3 emissions with the source document and page. Re-add the scopes (standing rule 6) and do not compare years in which the Scope 3 categories change. Moody's does not supply site, permit or incident data.

**Tested:** A rated UK water utility returned CIS-5 with E-4, S-4 and G-5 commentary and two years of emissions cited to its annual report. An unrated UK gym operator returned no ESG scores but three years of emissions; its 2024 market- and location-based sub-totals did not foot, and Scope 3 rose twelvefold because new categories were added.

**Follow-ups:**
- "What would a future buyer ask about this at exit?"

**Adjust for:** Thematic funds: add the theme's impact indicators.

---

## External evidence and consolidation

### DD-36 External corroboration with Moody's

**Use case:** Check management's figures and story against an independent record.

**Inputs:** The information memorandum or data-room figures; the Moody's connector.

**Prompt:**
```
Use the Moody's tools to corroborate [Company] (registered name [legal name], country [country]).

1. findEntity once for the company, its parent and its [top five customers and top three suppliers]. Print each match with legal name, country and identifier. Stop and show me if any match looks wrong.
2. getEntityProfile, getEntityUltimateOwners (threshold 25.01) and getEntitySubsidiaries for the company. Then map the group (standing rule 9b): run getEntityRatings on any Bidco, Holdco, Topco or Finco found, and if an assigned rating exists, take Moody's-adjusted leverage from its credit opinion (standing rule 11a).
3. getEntityFinancials_v2 for the last [three] years.
4. getEntityPeers (up to 8) on the operating company, then getEntityFinancials_v2 for each peer. Peers of a holding or financing entity are other holding companies, not competitors.
5. For customers and suppliers: getEntityRatings, or getEntityProbabilityOfDefault_v2 where unrated.

Then build a three-column table for revenue, EBITDA, debt and headcount by year: management says, Moody's shows, difference. Put every difference above [5%] in the contradiction log with a question for management. Benchmark the company's margins and leverage against the peers, saying why each peer is comparable.

End with: facts we did not have before, management claims contradicted, and what Moody's did not cover.
```

**Output:** Entity matches, three-column table, contradiction log, peer benchmark and a value summary.

**Moody's:** As written. Tested on a UK private gym operator: three years of filed financials, the ownership chain up to the sponsor fund, and five peers came back. Financials are those of the legal entity named, so check whether it is the group parent.

**Tested:** For one sponsor-owned UK leisure group the matched entity returned EBITDA higher than its revenue, so it was a property or financing company inside the group. Standing rule 9c catches this; check the subsidiaries and ultimate owner to find the trading and borrowing entities. In a full run on a UK gym group, the follow-up found a B2-rated borrower (Pinnacle Bidco plc) that the main steps missed; the operating company's PD implied rating was A3. Of eight peers returned, four were usable after dropping duplicates, a property entity and a stale peer. Without an information memorandum the "management says" column is [GAP]; public statements gave site counts but no financials.

**Follow-ups:**
- "Which entity in the group carries the acquisition debt? Pull its financials and default probability too."
- "Show the peers' default probabilities and where the company sits among them."

**Adjust for:** Take-private: add `searchCompanyFilings` on annual reports and earnings call transcripts (the first call may start an ingestion that takes about three minutes; run it again).

### DD-37 Route findings between workstreams

**Use case:** Make sure a finding in one workstream gets sized by the right team.

**Inputs:** Findings from all workstreams.

**Prompt:**
```
Here are the findings so far for [Company] from each workstream. For each finding that needs another workstream's input, raise a hand-off: from which workstream, to which, and the exact question.

Use these routes: legal change-of-control or termination rights to financial (revenue and prepaid amounts at risk); financial revenue recognition issues to legal (are the rights enforceable); financial pricing anomalies to commercial (volume, renewal or competitive explanation); commercial service-level exposure to financial (maximum annual credit liability); legal intellectual-property gaps to technology (which systems depend on it); technology data-transfer gaps to legal (valid transfer mechanism); any material counterparty to external evidence (ownership and credit).
```

**Output:** A hand-off list with questions.

**Moody's:** Counterparty hand-offs go to DD-36.

**Follow-ups:**
- "Which hand-offs are still open after a week?"

**Adjust for:** None.

### DD-38 Consolidated findings register

**Use case:** One register of every finding, with what to do about each in the purchase agreement.

**Inputs:** All workstream outputs.

**Prompt:**
```
Merge every finding on [Company] into one register: ID, workstream, finding, evidence and locator, severity, confidence, amount at stake as stated in the source, and proposed protection (price adjustment, specific indemnity, escrow, warranty, condition before closing, covenant, insurance, or accept).

Remove duplicates and keep the more severe rating when two workstreams rate the same issue differently, with a note. Then challenge your own register: for every CRITICAL and MAJOR finding, re-open the source and confirm the quote and context. Mark each one Confirmed, Revised or Unresolved.

End with a draft view: proceed, proceed with conditions (list them), or do not proceed, with the reasons.
```

**Output:** A consolidated, challenged register and a draft view for the deal partner.

**Moody's:** External findings from DD-36 are included as their own workstream. When re-opening a CRITICAL or MAJOR finding that rests on Moody's research, search for issuer comments or rating actions published after it: a later comment can change the finding.

**Tested:** On a UK gym group, a MAJOR flag on an acquisition signed after the latest credit opinion was revised when an issuer comment published nine days after the opinion said the deal lowered forecast leverage.

**Follow-ups:**
- "Turn the conditions into a list for the purchase agreement negotiation."

**Adjust for:** None.

---

## Sessions with management, experts and customers

### DD-39 Prepare a management session

**Use case:** Use limited time with management on what matters.

**Inputs:** Open gaps, red flags, the findings register.

**Prompt:**
```
Prepare a [90]-minute session with [Company]'s [chief executive / finance director / sales director].

Give: an agenda with timings; the ten most important questions in order, each with a follow-up for a vague answer; what to listen for that would suggest a problem is being managed around; and what to ask to see in person.

Base everything on our open items for this deal. Start open and become specific. End by asking what we should have asked.
```

**Output:** A session plan.

**Moody's:** Not needed.

**Follow-ups:**
- "Which answers would end the deal?"

**Adjust for:** Site visits: add what to observe.

### DD-40 Synthesise expert and customer calls

**Use case:** Turn call notes into evidence for or against the thesis.

**Inputs:** Transcripts or notes from expert, customer or former-employee calls.

**Prompt:**
```
Synthesise these [number] call transcripts on [Company] by theme, not call by call.

For each commercial hypothesis: what speakers said, with short verbatim quotes and speaker ID; whether the evidence supports, contradicts or is ambiguous on the hypothesis; and how consistent speakers were. Keep statements, your observations and your analysis visibly separate.

Then list: where speakers contradicted management, points raised by two or more speakers that we had not considered, and who else we should speak to.
```

**Output:** A thematic synthesis and a contradictions list.

**Moody's:** Not needed.

**Follow-ups:**
- "Which quotes should go in the committee paper?"

**Adjust for:** None.

### DD-41 Track verbal claims

**Use case:** Stop things said in meetings from becoming facts without evidence.

**Inputs:** Meeting notes and transcripts.

**Prompt:**
```
From these meeting notes on [Company], list every factual claim management made: the claim, who made it, the date, and the document that would prove it. Mark each as Documented (we have the evidence), Requested, or Asserted (no evidence yet).

Flag any claim that conflicts with the data room.
```

**Output:** A claims tracker.

**Moody's:** Not needed.

**Follow-ups:**
- "Add the Requested items to the request list."

**Adjust for:** None.
