# Portfolio monitoring prompts

Use these during the hold period, from the first monthly pack after closing to the last investor report before exit. Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach the documents named under **Inputs**, fill the bracketed fields and run it.

A note on what these prompts are. Each one retrieves and analyses current data when you run it. That is different from a monitoring service. A monitoring service needs a schedule that is known to have run, stored history to compare against, thresholds the fund has set, and a named owner for every alert. If you want the early-warning prompts to work as monitoring, keep each run's output, feed the previous run back in, and assign owners. A run that is skipped leaves a gap that cannot be filled afterwards.

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
| Monthly pack | PM-01 to PM-06 |
| Covenants and liquidity | PM-07 to PM-10 |
| Early warning | PM-11 to PM-14 |
| Board and valuation | PM-15 to PM-17 |
| Investor reporting | PM-18 to PM-21 |

---

## Monthly pack

### PM-01 Normalise the pack to the fund template

**Use case:** Turn a company's monthly pack, in whatever layout it arrives, into the fund's standard format.

**Inputs:** The monthly management accounts or board pack; the fund's reporting template; last month's normalised pack.

**Prompt:**
```
Map [Company]'s pack for [month] to the fund template attached.

1. For each template line, give the pack line or lines it comes from, with page or sheet and cell. Where several pack lines combine, show the sum.
2. List pack lines you could not map and template lines the pack does not supply, as [GAP].
3. Run three ties: income statement lines re-add to the pack's EBITDA; the balance sheet balances; opening cash plus net cash flow equals closing cash, and closing cash equals balance-sheet cash.
4. Check that prior-month figures in this pack equal last month's normalised pack. List every restatement with both figures.
5. State the currency, units and whether figures are consolidated or entity-level. Do not assume a currency the pack does not state.
6. Note whether the pack arrived by the agreed date of [date].
```

**Output:** A mapped template with locators, a gap list, three tie results, a restatement list and a lateness note.

**Moody's:** Not needed.

**Follow-ups:**
- "Write the mapping as a reusable table so next month's pack maps the same way."
- "Draft the note to the finance director listing the gaps and restatements."

**Adjust for:** Lower mid-market: expect no formal pack; build the template from the ledger export and say so. Buy-and-build: map acquired businesses separately before consolidating, and label pro forma figures. Minority: record what the information rights entitle us to and what arrived.

### PM-02 Bad-data scan

**Use case:** Catch errors in the pack before anyone draws conclusions from it.

**Inputs:** The normalised pack for this month and the last [12] months.

**Prompt:**
```
Scan [Company]'s normalised data for [month] and the prior [12] months for signs of bad data:
1. Repeated values: any line identical to the prior month or across several months, which may be a copied-forward figure.
2. Sign errors: costs shown as positive, revenue negative, or a balance flipping sign with no explanation.
3. Currency and unit mismatches: a line that jumps by roughly a factor of 1,000 or by an exchange rate, or a subsidiary reported in a different currency.
4. Unexplained swings: any line moving by more than [threshold %] month on month with no commentary.
5. Totals that do not foot, and year-to-date figures that do not equal the sum of months.

For each issue: line, months affected, values, the likely explanation if the pack supports one, and the question for the finance team. Do not correct any figure; list it.
```

**Output:** An issue list by type with values and questions.

**Moody's:** Not needed.

**Follow-ups:**
- "Which issues would change a KPI status if corrected?"

**Adjust for:** Buy-and-build: check for acquired-entity figures entered in the month before completion. Carve-out: check for transition-service charges coded to the wrong cost lines.

### PM-03 KPI extraction with locators

**Use case:** Pull the agreed KPIs (key performance indicators) for the month, each traceable to the pack.

**Inputs:** The normalised pack; the KPI set agreed for this company with definitions.

**Prompt:**
```
Extract [Company]'s agreed KPIs for [month]: [list of eight to twelve KPIs, or "use the attached KPI definitions"].

For each KPI: definition (numerator, denominator, source, cut-off date); direction (higher is favourable or lower is favourable); value for the month, year to date and last twelve months; and the locator for every input figure. Recompute each KPI from its inputs; do not copy a KPI the pack has already calculated without recomputing it, and show any difference.

A KPI without an agreed definition is reported as [GAP: definition not agreed].
```

**Output:** A KPI table with definitions, direction tags, three periods and locators, plus recomputation differences.

**Moody's:** Not needed monthly. Quarterly, `getEntityPeers` (clean the list under rule 12a) then `getEntityFinancials_v2` gives peer margins, growth and working-capital ratios for context, on an annual reported basis.

**Follow-ups:**
- "Show each KPI over the last [12] months and mark any three-month run in one direction."

**Adjust for:** Specialist funds: use the house KPI set and definitions. Growth equity: add runway (cash divided by the latest month's net burn) and burn multiple (net cash burned divided by net new annual recurring revenue). Thematic funds: add the theme's leading indicators and impact KPIs with the same locators.

### PM-04 Variance against budget and the underwriting case

**Use case:** Show how the month compares with the budget and with the case the fund invested on.

**Inputs:** PM-03 output; the budget; any reforecast; the underwriting case (the financial case approved at investment).

**Prompt:**
```
Before computing anything, tell me which column in [Company]'s pack is the budget, which is a reforecast, and which (if any) is the underwriting case. If the pack does not say, stop and ask me which scenario counts as "plan".

Then, for each KPI and for revenue, EBITDA and cash, show four columns: actual, prior period, plan, and underwriting case, for the month, year to date and last twelve months.

For each variance: amount, percentage, the favourable direction for that metric (stated, for example "higher is favourable" for revenue, "lower is favourable" for churn or receivable days), and whether the variance is favourable or adverse. Set a status from the adverse variance: green under [x%], amber [x% to y%], red over [y%].

Revenue, EBITDA and cash always get a one-line narrative. Large favourable variances get one too.
```

**Output:** A confirmed scenario mapping, a four-column variance table with direction and status, and narratives.

**Moody's:** Not needed.

**Follow-ups:**
- "Compare last-twelve-month EBITDA with the underwriting case for the same date and give the plan status: on track, watch or action required."

**Adjust for:** Buy-and-build: split variance into like-for-like and acquired. Carve-out: add standalone cost run-rate against plan. Turnaround: put weekly cash against plan first.

### PM-05 The three worst variances

**Use case:** Give the deal lead the three variances that matter most, with causes and questions for the board.

**Inputs:** PM-04 output; management commentary; the ledger or sales detail if available.

**Prompt:**
```
From [Company]'s variance table for [month], pick the three most damaging adverse variances, judged by their effect on EBITDA and cash in amount (percentage alone can mislead on small lines).

For each:
1. The variance, amount and direction.
2. The drivers: split revenue into volume, price, mix and timing, and costs into rate and volume, where the data allows. The drivers must sum to the variance; show anything unexplained as its own line.
3. The cause, stated only where the pack, the ledger or management commentary supports it. Otherwise write "driver unclear".
4. Two questions for the board, specific enough that a vague answer is obvious.
```

**Output:** Three variance cards with driver bridges, supported causes and board questions.

**Moody's:** Where a named customer or supplier is the cause, `findEntity` then `getEntityRatings` gives its credit standing (state the rating class and outlook). Only where no entity in its group is rated, use `getEntityProbabilityOfDefault_v2` (rules 9b and 11). A credit measure speaks to failure risk, not to price or volume; do not cite it as the cause of a variance.

**Follow-ups:**
- "Is any of the three part of a trend over three or more months?"

**Adjust for:** Growth equity: favour variances in bookings, net revenue retention and burn over EBITDA.

### PM-06 Timing or permanent

**Use case:** Separate variances that will reverse from those that will not.

**Inputs:** PM-04 and PM-05 outputs; prior months' variance tables; management commentary.

**Prompt:**
```
For each [Company] variance above [threshold] in [month], classify it as:
- Timing: the revenue or cost has moved between months and will reverse within [period]; name the month it should reverse in.
- Permanent: it will not reverse (lost customer, price cut, structural cost).
- Classification: a coding or allocation change with no economic effect.

Give the evidence for each classification. Check prior months: any variance called timing before that has not reversed by now is reclassified as permanent unless new evidence supports it. Show the effect on full-year EBITDA of the permanent items only.
```

**Output:** A classified variance list, a check on earlier timing calls and a full-year effect.

**Moody's:** Not needed.

**Follow-ups:**
- "List every timing call from the last six months with whether it reversed."

**Adjust for:** Seasonal businesses: compare with the same month last year before calling anything timing.

---

## Covenants and liquidity

### PM-07 Covenant test from the executed agreement

**Use case:** Test each financial covenant from the signed credit agreement and show how much room is left.

**Inputs:** The executed credit agreement and every amendment, waiver and consent; financial statements for the test period; management's compliance certificate.

**Prompt:**
```
Run the covenant tests for [Company] as at [test date] for the test period [period].

1. From the executed agreement and every amendment, list each financial covenant with its level in force at this date (apply the step-down schedule), with clause and page. If an amendment is referred to and not in hand, mark the affected test Undetermined.
2. Build covenant EBITDA clause by clause from the agreement's definition, applying each cap. Show fund-accepted and management EBITDA as memo rows only.
3. Compute each ratio unrounded, shown to two decimals.
4. Headroom:
   - Maximum test (for example net debt to EBITDA): (covenant level minus actual ratio) divided by covenant level.
   - Minimum test (for example interest cover): (actual ratio minus covenant level) divided by covenant level.
   Positive is a pass. Also give the fall in covenant EBITDA that would cause a breach, in amount and percentage, holding other inputs at test-date values, and confirm that putting that EBITDA back into the ratio returns the covenant level.
5. Headroom at the next step-down on current figures.
6. Reconcile line by line to management's compliance certificate.

Status per test: Compliant; Compliant, tight (headroom below [watch level %]); Not tested this period (springing test not triggered, with the trigger calculation); Shortfall, cure right available; Shortfall, no cure right available; Waiver or amendment in place; Undetermined. Never adjust a figure to produce a pass. Whether a shortfall is an event of default is for counsel.
```

**Output:** A covenant term sheet with clauses, the EBITDA build, ratios, headroom two ways, a certificate reconciliation and a status per test.

**Moody's:** `findEntity` on the borrower named in the agreement and its parent; compare the matched legal name with the agreement exactly (rule 9a). `getEntityUltimateOwners` on the borrower shows the chain above it; `getEntitySubsidiaries` lists direct subsidiaries only. For a rated borrower, `searchAllDocuments` scoped to it with keywords on structural considerations gives Moody's description of the issuer, guarantor coverage, security, ranking and any springing covenant trigger (rule 11b). The agreement alone says which entities are obligors and what the covenant levels are.

**Tested:** On PureGym, the chain Pinnacle Topco Limited, Pinnacle Midco 1 Limited, Pinnacle Bidco plc (issuer of the notes and revolver) came from three tools. Moody's research gave guarantor coverage as 99% of adjusted EBITDA and the springing covenant trigger (revolver more than 40% drawn), but no guarantor names. Separately, "Zoro Bidco, Inc." resolved to Zendesk, Inc. without warning.

**Follow-ups:**
- "Draft an unsigned compliance certificate in the agreement's exhibit form, marked UNSIGNED DRAFT."
- "Show headroom over the last [four] test dates."

**Adjust for:** Buy-and-build: show headroom with and without pro forma acquired EBITDA and synergy add-backs, each within its cap and time window. Carve-out: check standalone costs and transition-service charges against the definition. Large-cap: list incurrence tests (tested only when the company takes a stated action) separately. Minority: if the agreement is not in hand, every test is Undetermined.

### PM-08 Reporting and cure deadline calendar

**Use case:** Know every date the company owes something to its lenders.

**Inputs:** The executed credit agreement with amendments; the company's financial year end; any waiver letters.

**Prompt:**
```
Build the lender deadline calendar for [Company] for the next [12] months.

For each obligation: what must be delivered or done (monthly or quarterly accounts, annual audited accounts, compliance certificate, budget, notice of default, equity cure deadline, step-down dates, test dates); the trigger it runs from; the period allowed; the business-day rule; the computed date; and the clause.

Compute each date from its own trigger. Flag any date within [20] business days, and any cure right with its limits (how many times, how often, maximum amount) and how many uses remain.
```

**Output:** A dated calendar with clauses, flags and cure-right status.

**Moody's:** Not needed.

**Follow-ups:**
- "Convert the calendar into reminders I can paste into the deal team's calendar."

**Adjust for:** Turnaround: add waiver, standstill and lender reporting dates, and weekly cash reporting obligations.

### PM-09 13-week cash flow review

**Use case:** Check the company's short-term cash forecast and how long the cash lasts.

**Inputs:** The company's 13-week cash flow; bank balances at the start date; facility statements; last week's version.

**Prompt:**
```
Review [Company]'s 13-week cash flow starting [date].

1. Opening cash ties to bank balances; each week's closing cash equals the next week's opening.
2. Compare last week's forecast for the week just ended with the actual, line by line, and explain differences above [amount].
3. Check the receipts assumptions against recent collection history and the payments against payables ageing and payroll dates.
4. Liquidity each week: cash plus undrawn committed facilities, against the floor of [amount]. Exclude uncommitted lines.
5. The lowest point, its week, and the main payments driving it.
6. Any week where liquidity falls below the floor or a minimum-liquidity covenant.
```

**Output:** Tie results, a forecast accuracy check, assumption tests, a weekly liquidity line and the low point.

**Moody's:** Not needed.

**Follow-ups:**
- "What actions (collections, payment timing, revolver draw) would lift the low point, and what does each cost?"

**Adjust for:** Turnaround: run weekly and lead the deal lead's note with it. Growth equity: add months of runway at the current burn and the date cash reaches [months] of runway.

### PM-10 Maturity wall and refinancing readiness

**Use case:** See when debt falls due and whether the company is ready to refinance.

**Inputs:** Debt schedule with every facility; latest covenant test; the current year's forecast.

**Prompt:**
```
For [Company], show every facility with amount, maturity, amortisation, interest basis and any call or prepayment terms. Flag any maturity within [18] months of [date].

Then assess refinancing readiness: leverage and interest cover today and at the maturity date on the current forecast; track record against covenants; the state of audited accounts and reporting; and the open questions a new lender would ask.

Write it as a readiness checklist with what is ready, what is missing and who owns each item.
```

**Output:** A maturity profile, a flag list and a readiness checklist.

**Moody's:** For the company itself, if its group has a rated issuer (rule 9b): `searchAllDocuments` scoped to the issuer with keywords on liquidity, revolver, term loan and maturities, and `penalizeOlderResearch` set to true, finds the credit opinions and rating actions that state facility maturities, revolver covenants and leverage (rule 11b). `getCreditOpinion` (sections `Summary`, `FactorsLeadingToDowngrade`, `KeyIndicatorsTable`) gives Moody's-adjusted leverage and downgrade triggers. Use only the most recent document for each facility and give its date; an amendment after that date will not show. Then `getEntityPeers` (clean the list under rule 12a; compare corporate family ratings), and `getEntityRatings` and `getCreditOpinion` (`FactorsLeadingToDowngrade`) on the rated peers, show where comparable credits sit and the triggers Moody's has set for them. Label this as context from rated peers; it is neither a refinancing quote nor a view on lender appetite. `searchAllDocuments` with sector and region filters adds Moody's research on credit conditions in the sector.

**Tested:** On McAfee, UKG and STADA, maturities came from credit opinions found with `searchAllDocuments`: McAfee's $1bn revolver matures on 1 March 2027 (opinion of 4 February 2026), inside the 18-month flag. The Key Indicators table came back for one of the three. A 2025 opinion still showed STADA's revolver expiring in December 2026, since replaced by a facility due 2032, so check document dates. The peer list included the company's own financing vehicle and two entities from one group. In a re-test, McAfee and UKG received the same eight software peers, apparently the sector's most recently rated issuers, so justify each peer before using it.

**Follow-ups:**
- "Which rated peers have refinanced or had rating actions in the last [12] months? Use getEntityRatings history and searchNews."

**Adjust for:** Large-cap: add any traded debt prices from the fund's market-data source. Lower mid-market: expect few rated peers and use PD implied ratings, labelled as model outputs.

---

## Early warning

### PM-11 Portfolio sweep

**Use case:** Find external events on portfolio companies since the last run.

**Inputs:** The portfolio list with registered legal names; the previous sweep's output, with dates and readings.

**Prompt:**
```
Run the early-warning sweep for [list of portfolio companies, or "the attached portfolio register"] for events since [date of last run].

1. Resolve all names in one findEntity call and print each match. Stop for any name with two plausible matches, or where the returned name differs from the name searched.
2. For each company, name the entity that carries the acquisition debt and the entity that holds the corporate family rating (rule 9b); resolve them if they were not on the list. Run the checks below on those entities.
3. For each company:
   - searchNews over [time range] with importance set to significance. Open each item and confirm the date, the entity and the facts. Log items about another subject as noise, with the reason.
   - getEntityRatings with history: any rating or outlook change since the last run. State the rating class; an instrument downgrade is not a corporate family rating downgrade.
   - getEntityProbabilityOfDefault_v2 with early_warning and a history window of [period]: the current PD and PD implied rating, labelled as model outputs, the PD history, the latest risk category, and the change in PD against the previous reading. Expect the risk category for the latest month only. If no PD comes back, as is common for rated borrowers, write [MOODY'S: NOT COVERED] and rely on the rating. Treat a one-notch implied-rating move that reverses within three months as noise unless the PD trend is sustained.
4. Compare every reading with the stored reading from the last run. Where there is no stored reading, say so.

Open the output with the retrieval date and the line in rule 13a. Return one row per signal: company, entity matched, signal type (assigned rating, model output, news, filing), event date, retrieval date, what changed, source. A company with no signals is listed as "no signals found", which does not mean healthy. Record coverage for each name: covered, not covered or stale. List noise items suppressed, with reasons.
```

**Output:** A signal table, a coverage table and a list of names with no stored prior reading.

**Moody's:** As written. `searchCompanyFilings` adds filings for names that file; the first call may start an ingestion, so run it again after about three minutes. A PD on a private operating company reflects that entity's own accounts; the acquisition debt may sit at a holding company, so say which entity each reading belongs to. Run PD and news on the borrower or operating company; a rating-only holding entity (for example Cloud Software Group Holdings, Inc.) made a batched PD call fail outright and returned unrelated news. If a batched PD call fails, drop such entities and retry.

**Tested:** On six sponsor-owned groups, the sweep found the two material events in 90 days (a first-lien downgrade with a $2bn tender at Cloud Software Group; an upgrade to B2 with refinancing at PureGym). The PureGym holding company first matched had no rating; the upgrade sat at Pinnacle Bidco plc. Cloud Software Group, LLC returned a bank-facility rating, not the corporate family rating. With early_warning and a 12-month window, the tool returned PD history and one risk-category row. PD was not available for three rated US borrowers. Nine of ten news items for one name were about something else.

**Follow-ups:**
- "Pass the signals to the materiality ranking (PM-13)."
- "If more than a fifth of names flagged this run, test for a data or mapping fault before anything else."

**Adjust for:** Turnaround: run daily for those companies. Buy-and-build: add add-on targets in the pipeline and recently acquired businesses. Carve-out: add the former parent while transition services run. Thematic funds: add `searchAllDocuments` on the theme's policy and sector events.

### PM-12 Counterparty exposure check

**Use case:** Check the credit standing of a portfolio company's largest customers and suppliers.

**Inputs:** The company's top customers and suppliers with revenue or spend share; the previous check.

**Prompt:**
```
For [Company], check its top [ten] customers and top [ten] suppliers, plus any above [5%] of revenue or spend.

1. findEntity in one call for all names; print each match and stop on any doubtful one.
2. For each: getEntityRatings (assigned rating, its class and outlook). If the matched entity is unrated, check whether another entity in its group is rated and say which entity the rating belongs to. Only where no group entity is rated, getEntityProbabilityOfDefault_v2 with early_warning (PD and PD implied rating, labelled as model outputs; do not add the dynamic PD implied rating without explaining that it is a different model output); searchNews over [time range] with importance set to significance; getEntityUltimateOwners (threshold 25.01) for any change of owner.
3. Table: counterparty, share of revenue or spend, rating or PD implied rating (labelled), change since last check, notable events, coverage.

Flag any counterparty above [share] whose credit measure has worsened or which has a change of owner. Searches use public names only; no internal figures go into a search.
```

**Output:** A counterparty table with exposure, credit standing, changes and flags.

**Moody's:** As written. The company supplies the customer and supplier list and the size of each relationship; Moody's does not. Credit opinions describe channels and customer types, rarely named counterparties; `searchAllDocuments` scoped to the company with keywords on partners, distributors and acquisitions sometimes names key partners and affiliates (in a re-test it named the public cloud providers as key partners of Cloud Software Group and an affiliate, Arctera, to be brought into its restricted group). Ultimate-owner records for listed companies often show the company itself with an old information date, so they detect a change of controlling owner only for closely held counterparties.

**Tested:** On five large technology and telecoms names (an illustrative list), one findEntity call resolved all five. One entity with no rating of its own returned a PD implied rating of Ba3 and a dynamic PD implied rating of Caa1 on the same day.

**Follow-ups:**
- "For each flagged counterparty, what does the contract say about payment terms, termination and change of control?"

**Adjust for:** Turnaround: add credit insurers and critical suppliers at the top of the list. Growth equity: add key channel partners. Specialist funds: add regulators' or payers' actions from the house signal list.

### PM-13 Materiality ranking

**Use case:** Cut a long list of signals to the few a deal lead should read.

**Inputs:** PM-11 and PM-12 outputs; the underwriting case; the latest covenant headroom.

**Prompt:**
```
Rank these signals for [portfolio or company] by materiality.

Score each on three factors from 1 to 3: severity of the signal (1 routine; 2 negative outlook, sustained deterioration over three readings, profit warning, senior departure, material litigation; 3 downgrade into distressed territory, step-change in a model-based default measure, insolvency filing, loss of licence); size of exposure (1 under [5%] or a peer; 2 [5% to 15%]; 3 over [15%], the company itself, or its lender); relevance to the thesis (1 touches no underwriting assumption; 2 touches one; 3 touches the main return driver or a covenant test).

Materiality = the product, from 1 to 27. Bands: Alert [18 to 27], Watch [8 to 17], Log [1 to 7].

Merge duplicates (the same event from several sources) and suppress noise (routine announcements, repeats of an event already logged), keeping every suppressed item in the log with its reason. Check that collected = alerted + watched + logged + suppressed + merged.
```

**Output:** A scored, banded list, a suppression log and a count check.

**Moody's:** Not needed at this step.

**Follow-ups:**
- "Draft an alert memo for each Alert: the event, the exposure, and the question to ask management."
- "Which Watch items have had no owner review for [ten] business days?"

**Adjust for:** Large-cap: tighten suppression, since public names generate more routine news. Lower mid-market: show the coverage table first, because many names will have no data.

### PM-14 Thesis drift

**Use case:** Say whether each company is still on the investment thesis.

**Inputs:** The underwriting case and value-creation plan; the latest KPI review; covenant status; early-warning signals.

**Prompt:**
```
For [Company], list each pillar of the original investment thesis and value-creation plan.

For each pillar: what the thesis assumed; what the evidence now shows (internal KPIs, milestones, external signals, each cited); and a label:
- On: evidence supports the pillar.
- Watch: one adverse indicator, or a milestone late, or an adverse trend over three periods.
- Off: the pillar no longer holds on current evidence.

Give an overall label for the company, the pillar that drives it, and what evidence in the next [quarter] would move it up or down.
```

**Output:** A pillar-by-pillar drift table and an overall label with triggers.

**Moody's:** `searchAllDocuments` scoped to the company's rated issuer (rule 9b), recent research, for Moody's view of its performance against each pillar. Then `searchAllDocuments` with sector and region filters and recent research only, for Moody's sector views on market pillars; discard results about other issuers. Figures Moody's quotes on the company's own definitions (for example company-adjusted EBITDA) are management figures: label them "as stated". Do not call `getEntitySectorOutlook` raw; its output is too large to use.

**Tested:** On PureGym and STADA, the most useful evidence came from each issuer's own credit opinion and issuer comment; most other results in the sector searches were about unrelated issuers.

**Follow-ups:**
- "For each Off pillar, what would a revised plan need to include?"

**Adjust for:** Thematic funds: add a pillar for the theme's leading indicators. Buy-and-build: add pace of add-ons and integration milestones. Carve-out: add transition-service exits against plan.

---

## Board and valuation

### PM-15 Board pack review and questions

**Use case:** Read a board pack before the meeting and arrive with the right questions.

**Inputs:** The board pack; the latest KPI review; the previous board minutes and action list.

**Prompt:**
```
Review [Company]'s board pack for the meeting on [date].

1. Summarise what management says in [five] lines, labelled "as stated".
2. List where the pack differs from our KPI review or the previous pack, with both figures.
3. Check each action from the previous minutes: done, in progress, or not mentioned.
4. Flag anything significant that the pack does not cover (a known issue, a missed milestone, a covenant point).
5. Write [ten] questions for the board, most important first, each with the follow-up for a vague answer.
```

**Output:** A summary, a difference list, an action check, omissions and ranked questions.

**Moody's:** Not needed. PM-11 signals on the company can feed step 4.

**Follow-ups:**
- "Which questions are better raised privately with the chief executive before the board?"

**Adjust for:** Minority: frame questions within the information rights and board role the shareholder agreement gives us. Turnaround: lead with liquidity and lender matters.

### PM-16 Quarterly mark support

**Use case:** Build the evidence for the quarterly fair-value mark, for the valuation committee to decide.

**Inputs:** The fund's valuation policy; last quarter's approved valuation pack; this quarter's approved KPI and covenant workbooks; multiples from the fund's licensed market-data source.

**Prompt:**
```
Prepare the mark support for [Company] as at [valuation date]. Fair value is the price that would be received to sell the holding in an orderly transaction between market participants at that date; the fund's policy says which standard and methods apply.

1. Method: confirm it matches the policy and last quarter. Any change needs a stated reason and is flagged for the committee. Show that the entry calibration (the inputs that reproduced the price paid) has been carried forward and updated.
2. Comparables: list peers with a reason for each inclusion and exclusion, and any change to the set since last quarter with its effect on the median. Multiples come only from the attached licensed export, with date and basis.
3. Earnings and net debt: from the approved KPI and covenant workbooks for the same date, reconciled if they differ.
4. Bridge from last quarter's approved mark to this quarter's proposed range: earnings, multiple, net debt, currency, other. Show any residual.
5. Cross-check by [discounted cash flow / recent transaction] and a sensitivity table whose centre cell equals the base output.

Propose a range with reasons. Do not present a single figure as the valuation.
```

**Output:** A method check, a reasoned peer set, a reconciled earnings and net debt line, a quarter-on-quarter bridge, a cross-check and a proposed range.

**Moody's:** `getEntityPeers` (clean the list under rule 12a) then `getEntityFinancials_v2` gives peer growth, margins and leverage to justify inclusions where Moody's holds them; call it without filters first, since filters can return empty results. For rated companies, the Key Indicators table in `getCreditOpinion` gives group revenue and Moody's-adjusted leverage (rule 11a). Check each entity's figures under rule 9c. `getEntityRatings` and `getEntityProbabilityOfDefault_v2` add credit context, each labelled; a PD computed on a holding company's partial accounts is not evidence for a higher mark. Moody's does not supply equity multiples.

**Tested:** On STADA, the rated holding company returned revenue of €0.9m against debt of €5.3bn (2024, its own accounts). Of the German private peers, one returned no financials and one a registry balance sheet with no revenue line. Group leverage came only from the credit opinion. In a re-test, the Key Indicators tables in the peers' own credit opinions gave revenue, EBITDA margin and leverage for two peers; one peer's table was in US dollars, so check the currency before comparing.

**Follow-ups:**
- "Which inputs are older than one quarter, and which peer financials are older than two?"

**Adjust for:** Buy-and-build: split earnings into like-for-like and acquired, and explain any blended multiple. Turnaround: lead with the policy's alternative method if earnings multiples are not meaningful. Growth equity: test the last round (date, size, investors, terms) and value the fund's instrument through the preference stack.

### PM-17 Argue the write-down and the write-up

**Use case:** Test the proposed mark from both directions with equal rigour.

**Inputs:** PM-16 output; the latest KPI review, covenant status, thesis drift label and early-warning signals.

**Prompt:**
```
For [Company]'s proposed mark range of [range] at [date], argue two cases with the same rigour.

Write-down case: the strongest evidence that fair value is below the range (earnings stress, covenant headroom, adverse signals, peer multiple compression, thesis pillars at Watch or Off). Build the lower value it implies.

Write-up case: the strongest evidence that fair value is above the range (outperformance, peer re-rating, a transaction or offer, de-risking). Build the higher value it implies.

For each case, cite every input. Then say which case the evidence supports more, and whether the range should move or widen. The valuation committee decides.
```

**Output:** Two argued cases with implied values and a view on the range.

**Moody's:** Credit signals from PM-11 (rating changes, PD early_warning moves) feed the write-down case, labelled as assigned rating or model output. A PD implied rating above the assigned rating is not write-up evidence (rule 11). Compare the latest credit opinion's forecasts with the previous opinion's: a cut in Moody's leverage or cash-flow forecast is write-down evidence. Debt Moody's reports outside the restricted group (for example a holding-company PIK facility) belongs in the equity bridge even though it is outside the borrower's credit metrics.

**Tested:** On STADA, Moody's research showed a €1.4bn PIK facility outside the restricted group, and the forecast for 2026 free cash flow fell from about €60m (May 2025) to "weak" with about €5m in 2027 (October 2026). No purchase price for the 2026 change of control was found.

**Follow-ups:**
- "Draft the committee paper section explaining the movement since last quarter."

**Adjust for:** Thematic funds: test policy dependence and adoption timing in the write-down case. Take-private: use the offer price as the calibration point.

---

## Investor reporting

### PM-18 Quarterly report sections

**Use case:** Draft the sections of the quarterly investor report that come from approved records.

**Inputs:** Fund-administrator records for the quarter; approved KPI one-pagers and valuation memos; the report template; the side-letter obligations register.

**Prompt:**
```
Draft the quarterly report sections for [Fund] for [quarter], as at [date].

Define once: DPI (distributions to paid-in capital, cash returned divided by capital paid in); RVPI (remaining net asset value divided by paid-in capital); TVPI (total value to paid-in, DPI plus RVPI); IRR, gross (before fees, expenses and the manager's profit share) and net (after them).

1. Fund performance: copy each figure from the administrator's records and label it Final, Unaudited or Estimate with its date. Do not compute official performance. Recompute TVPI from DPI and RVPI as a check only.
2. Check that holding values plus other net assets equal fund net asset value.
3. One update per company of [150 to 250] words, drawn only from the approved one-pager and valuation memo. Explain any holding whose value moved more than [10%] from the approved valuation memo.
4. List side-letter reporting obligations due this quarter with a status.

Keep investor-specific data out of general sections. No forecasts, targets or assurances about future returns or exits. Mark wording you drafted as drafted.
```

**Output:** Performance figures tied to the administrator, a net asset value check, company updates and a side-letter status list.

**Moody's:** `searchNews` on each company for dated public events in the quarter, with a date range equal to the quarter, on the operating company and on the rated issuer (rule 9b); open each item (rule 12b). `searchAllDocuments` with sector and region filters and `penalizeOlderResearch` for a market paragraph, cited to named research. A Moody's forecast may be quoted as the agency's forecast with its date; it is not the fund's view. The fund must check its Moody's licence terms before any Moody's-sourced data appears in a document sent to investors.

**Follow-ups:**
- "Produce a source-trace table with one row per number in the draft."

**Adjust for:** Buy-and-build: split like-for-like and acquired growth and list add-ons completed. Carve-out: report transition services exited and standalone cost run-rate. Growth equity: state the basis of each mark (round price or other). Minority: check confidentiality terms before disclosing any company figure. Specialist funds: use the house KPI set with a glossary. Thematic funds: report theme indicators and impact KPIs with the same source trace; cut impact claims with no measured figure.

### PM-19 Credit appendix

**Use case:** Add an optional view of the portfolio's credit standing to the investor report.

**Inputs:** Portfolio list with values; the Moody's connector.

**Prompt:**
```
Build a credit appendix for [Fund]'s portfolio as at [date].

1. Resolve every company in one findEntity call and print the matches.
2. For each company, name the entity that carries the acquisition debt and the entity that holds the corporate family rating (rule 9b); these are often a Bidco or Holdings entity, not the company owned. getEntityRatings on the rating holder, using the long-term corporate family rating, not an instrument rating. Only where no entity in the group is rated, getEntityProbabilityOfDefault_v2 (PD and PD implied rating).
3. Two separate tables, never combined or averaged: (a) assigned ratings, with the distribution by rating category; (b) model outputs for groups with no assigned rating, with the distribution of PD implied ratings. A group with an assigned rating never appears in table (b). Label each table plainly.
4. A coverage table: share of portfolio value with an assigned rating, with a model output only, and not covered. If coverage is below [half] the portfolio by value, offer only the coverage table.
5. An entity note for each figure: the legal entity, its identifier, and whether it is the borrower, the rating holder or the operating company.
6. State the retrieval date and that this is not a monitored series (rule 13a).
```

**Output:** Two labelled distributions, a coverage table and entity notes.

**Moody's:** As written. The fund must check its Moody's licence terms before any Moody's-sourced data appears in a document sent to investors.

**Tested:** On a six-company portfolio, the PureGym holding company first matched was unrated while its Bidco held a B2 corporate family rating; run on the first match, PureGym would have shown as a Ba3 model output. No PD came back for three of the five rated groups.

**Follow-ups:**
- "Show changes in each distribution since last quarter."

**Adjust for:** Lower mid-market: expect low coverage and lead with the coverage table. Growth equity: the appendix will rarely add much; say so.

### PM-20 Due-diligence questionnaire answers

**Use case:** Answer an investor's due-diligence questionnaire (DDQ) from the fund's approved answer library.

**Inputs:** The questionnaire; the approved answer library with approval dates; current figures from the source trace.

**Prompt:**
```
Answer [Investor]'s questionnaire dated [date] using only the approved answer library attached.

For each question: the answer; the library reference; and a match label of Verbatim, Adapted, New text or No answer. Refresh every figure inside an answer from the current source trace; never copy a figure from the library. Flag any library answer last approved more than [12] months ago as stale.

Where the library has no answer, write "No approved answer. Owner: [role]" and do not draft from general knowledge. List every Adapted and New text item for compliance review.
```

**Output:** A completed questionnaire with references and labels, a stale list and a compliance review list.

**Moody's:** Not needed.

**Follow-ups:**
- "Group the No answer items by owner and draft the request to each."

**Adjust for:** Large-cap: more investors use bespoke templates and side letters, so check each answer against the side-letter register.

### PM-21 Investor question response

**Use case:** Draft a reply to a specific question from an investor.

**Inputs:** The investor's question; approved reports, valuation memos and library answers; the side-letter register.

**Prompt:**
```
Draft a reply to this question from [Investor]: "[question]".

Answer only from approved material: the latest quarterly report, approved valuation memos and the answer library. Cite each source. Check the side-letter register for anything this investor is or is not entitled to receive, and whether the same information must go to all investors.

If the answer needs information that is not approved for release, say what is missing and who would approve it. No forecasts or assurances. Mark the draft for review by [head of investor relations] and [compliance officer].
```

**Output:** A cited draft reply, an entitlement check and a list of items needing approval.

**Moody's:** Not needed.

**Follow-ups:**
- "Shorten the reply to one paragraph for email."

**Adjust for:** Minority: check company confidentiality terms before sharing any company figure. Turnaround: have counsel clear any wording on lender discussions.
