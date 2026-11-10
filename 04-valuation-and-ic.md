# Valuation and investment committee prompts

Use these once diligence has produced fund-accepted EBITDA, a net debt schedule, revenue cases and a findings register, and the deal team is preparing the paper for the investment committee (the fund's decision-making body for new investments). Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach the documents named under **Inputs**, fill the bracketed fields and run it.

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
| Structure and returns | IC-01 to IC-04 |
| Financing and stress | IC-05 to IC-06 |
| Valuation and the case for the price | IC-07 to IC-08 |
| Committee paper and challenge | IC-09 to IC-11 |
| Conditions | IC-12 |

---

## Structure and returns

### IC-01 Sources and uses

**Use case:** Set out where the money comes from and where it goes, before any return is calculated.

**Inputs:** Bid letter or process letter, financing term sheets from the fund's advisers, the net debt and debt-like items schedule from diligence, adviser fee estimates.

**Prompt:**
```
Build the sources and uses table for the acquisition of [Company] at a headline enterprise value of [amount, currency] on a [cash-free, debt-free / locked-box / other] basis, with completion assumed on [date].

Uses: equity purchase price; refinancing of existing debt; debt-like items settled at completion; transaction fees (legal, financial, tax, other advisers); financing fees by tranche; separation or integration costs paid at completion; cash left on the balance sheet. Every fee is shown with its source or stated as zero.

Sources: each debt tranche with amount and source document; rollover equity from management or the seller; co-investment; sponsor equity.

Then:
1. Show the bridge from headline enterprise value to the equity price paid, line by line, using the diligence net debt schedule.
2. Check that sources equal uses exactly, and that sponsor equity equals uses less debt, rollover and co-investment.
3. Show entry multiples on fund-accepted EBITDA, management-adjusted EBITDA and reported EBITDA in three separate rows.
4. List every input that is a fund assumption with no document behind it.
```

**Output:** A sources and uses table, an enterprise-to-equity bridge, two tie-out results, entry multiples on three EBITDA bases and an assumptions list.

**Moody's:** Not needed.

**Follow-ups:**
- "Which debt-like items will the seller argue belong in the peg instead of the price, and what is the effect on equity if they win?"
- "Show the equity cheque if the senior tranche comes in [amount] lower."

**Adjust for:** Carve-out: put one-off separation costs and the first year of transition services (services the former parent keeps providing for a fee after completion) in uses. Take-private: build the price from the undisturbed share price plus premium on fully diluted shares, and add public-to-private costs. Growth equity: replace the table with the round's primary and secondary split and the capitalisation table after the round.

### IC-02 Returns model specification

**Use case:** Write the specification for a returns workbook an associate can build and a principal can audit.

**Inputs:** IC-01 output, the quality-of-earnings bridge, the revenue cases from commercial diligence, financing term sheets, the fund's return hurdle, maximum leverage and typical hold period.

**Prompt:**
```
Write the specification for a returns model for [Company]. Do not compute returns yet; specify the workbook.

Sheets, in order: Assumptions (every input with source or "fund assumption", inputs in blue); Sources and Uses; Operating Case; Debt Schedule; Exit and Returns; Attribution; Cases; Sensitivities; Checks.

Four cases, each defined once:
- Base: the fund's view, starting from fund-accepted EBITDA of [amount] for [period].
- Management: the seller's plan as stated, unedited.
- Lender: the case financing would be sized on, with lower growth and no unproven initiatives.
- Downside: the downside revenue case from commercial diligence plus an exit multiple below entry.

For each case, list the drivers that change and the source of each value. Exit multiple in the base case is no higher than entry unless a named, evidenced reason is given. Hold period is [years].

Checks sheet, each reported as Pass, Fail or Not built, before any multiple of invested capital (MOIC, total value returned divided by equity invested) or internal rate of return (IRR, the annualised return from dated cash flows) is shown:
R1 sources equal uses; R2 sponsor equity equals uses less debt, rollover and co-investment; R3 each tranche rolls forward (opening plus drawings less repayments equals closing); R4 balance sheet balances each period, or Not built; R5 cash ties across cash flow, balance sheet and debt schedule; R6 minimum cash held and revolver within its limit; R7 one period's interest per tranche recomputed independently; R8 any circular calculation converges, with the method stated; R9 exit net debt matches the debt schedule; R10 MOIC and IRR recompute from dated equity cash flows; R11 attribution sums to value created within [5%]; R12 entry EBITDA and revenue cases equal the diligence workbooks; R13 centre cell of each sensitivity grid equals the model result; R14 every figure in the memo matches its model cell.

Rule for the model and for you: never change an input to reach the hurdle of [hurdle]. If the base case falls short, show the return on supported inputs and, separately, what would have to be true to reach the hurdle.
```

**Output:** A sheet-by-sheet specification, four case definitions with driver sources, and a checks list ready to build.

**Moody's:** `getEntityPeers` then `getEntityFinancials_v2` on each peer gives margin and growth ranges to sanity-check the operating case. State each peer's EBITDA basis (before or after rent, reported or Moody's-adjusted) and compare the operating case on the same basis; add the largest named competitors if the peer list omits them. Moody's does not supply debt pricing or exit multiples; those come from the fund's financing advisers and licensed market-data source.

**Follow-ups:**
- "Audit the built workbook against this specification and list every check at Fail or Not built."
- "Which inputs on the Assumptions sheet have no source? Write the request for each."

**Adjust for:** Buy-and-build: add an Add-ons sheet (entry multiple, acquired EBITDA, integration cost, evidenced synergies only) and label pro forma figures. Turnaround: put the 13-week cash flow and funding need to stabilisation ahead of the operating case, and make time to recovery the main sensitivity. Growth equity and minority: replace the leveraged model with a capitalisation table, dilution from later rounds and the preference stack at exit; use runway and burn instead of leverage.

### IC-03 Returns attribution at entry

**Use case:** Show where the return comes from, so the committee can see how much rests on multiple expansion.

**Inputs:** The returns model (base case and downside at minimum).

**Prompt:**
```
For [Company], attribute equity value created between entry on [date] and exit in [year] in each case, in a table.

Three components:
1. EBITDA growth: (exit EBITDA minus entry EBITDA) times entry multiple.
2. Multiple change: (exit multiple minus entry multiple) times exit EBITDA.
3. Debt paydown and cash generation: entry net debt minus exit net debt.

Show leakage separately (fees, management incentive pool, dividends paid out). Value created must equal exit equity minus entry equity. Any residual above [5%] of value created is shown as its own line with an explanation; do not absorb it into a component.

Then say, for the base case: what share of value created depends on the exit multiple being higher than entry, and what share depends on growth beyond the lender case.
```

**Output:** An attribution table per case with a residual line and two dependency statements.

**Moody's:** Not needed.

**Follow-ups:**
- "Split EBITDA growth into revenue growth and margin change."
- "Show the attribution if the exit multiple equals entry."

**Adjust for:** Buy-and-build: split EBITDA growth into organic and acquired, and show the multiple arbitrage on add-ons as its own line. Growth buyout: the committee will lean on the growth component, so add the evidence grade for each growth driver beside it.

### IC-04 Sensitivity grids

**Use case:** Show how returns move with the inputs that matter most, with a check that the grid is wired correctly.

**Inputs:** The returns model.

**Prompt:**
```
Build sensitivity grids for [Company]'s base case returns (MOIC and IRR) for these pairs:
1. Entry multiple against exit multiple.
2. Revenue growth against EBITDA margin in the exit year.
3. Exit year against exit multiple.
4. [Pair of the deal team's choice, for example leverage at entry against interest rate].

Each grid is odd-sized (for example five by five), symmetric around the base case, with step sizes stated. The centre cell of every grid must equal the model result exactly; report this check for each grid as Pass or Fail before showing the grid. Shade cells below the hurdle of [hurdle].

Then list the three single inputs whose change most moves IRR, ranked by the effect of a one-step move.
```

**Output:** Four checked grids and a ranking of the most sensitive inputs.

**Moody's:** Not needed.

**Follow-ups:**
- "For each grid, which cell matches the lender case and which the downside case?"
- "Find the break-even exit multiple at which MOIC equals 1.0x."

**Adjust for:** Turnaround: make time to stabilisation one axis of every grid. Carve-out: add stranded costs (parent costs that stay with the business after separation) against standalone cost savings. Growth equity: use exit value against dilution from future rounds.

---

## Financing and stress

### IC-05 Covenant and liquidity stress

**Use case:** Find out when and why the proposed debt package would first breach, and how tight it gets.

**Inputs:** The returns model's debt schedule, the financing term sheet or draft credit agreement with covenant levels and step-downs, the lender and downside cases.

**Prompt:**
```
Stress the proposed financing of [Company] under the base, lender and downside cases, quarter by quarter for [years].

Use the covenant levels and step-downs in [term sheet or draft agreement, with clause references]. Define headroom once:
- Maximum test (for example net debt to EBITDA): headroom = (covenant level minus actual ratio) divided by covenant level.
- Minimum test (for example interest cover): headroom = (actual ratio minus covenant level) divided by covenant level.
Positive headroom is a pass. Also show the fall in EBITDA that would cause a breach, holding debt, cash and interest at their modelled values.

Report:
1. For each case, the first quarter in which any test breaches, or "no breach in the period".
2. The tightest headroom in each case, the test and the quarter.
3. Liquidity each quarter: cash plus undrawn committed facilities, against a floor of [amount]. Exclude uncommitted lines.
4. A ranking of drivers by how much each brings the first breach forward: revenue growth, margin, working capital, capital expenditure, interest rate, timing of any add-on.

Use the term sheet's EBITDA definition where it gives one, and say where it differs from fund-accepted EBITDA.
```

**Output:** First breach by case, tightest headroom, a quarterly liquidity line and a ranked driver list.

**Moody's:** Not needed for the test itself. `getEntityRatings` and `getCreditOpinion` (section `KeyIndicatorsTable`, one peer per call) on rated peers show how comparable credits sit on leverage and cover, labelled as context. If the section returns no key indicators, take the figures from the Summary text or mark them [GAP] (standing rule 11a).

**Follow-ups:**
- "What equity cure (new sponsor equity counted towards the test) would be needed at the first breach in the downside case, and does the term sheet allow it?"
- "Rerun with the step-downs pushed back by [four] quarters."

**Adjust for:** Buy-and-build: run headroom with and without pro forma acquired EBITDA and synergy add-backs, each within the definition's cap and time window. Carve-out: check that standalone costs and transition-service charges are treated as the definition says. Turnaround: run weekly liquidity for the first [13] weeks before the quarterly view. Growth equity: replace covenant tests with months of runway (cash divided by monthly net burn) under each case.

### IC-06 Financing context with Moody's

**Use case:** Describe where comparable rated credits sit, as labelled context for the committee.

**Inputs:** Proposed leverage and cover at entry from the model; a list of peers and any rated borrowers in the sector; the Moody's connector if available.

**Prompt:**
```
Prepare financing context for [Company]'s proposed structure: total leverage of [x] times on [EBITDA basis] and interest cover of [y] times at entry.

1. Resolve [Company], its likely borrower entity and [peers] in one findEntity call. Print each match.
2. For each peer and for the target: getEntityRatings. Quote assigned ratings as ratings, with their type. Use the corporate family rating where one exists; if the matched entity carries only an instrument rating (for example a senior secured facility), say so and look for the corporate family rating at the parent. Where a company is unrated, getEntityProbabilityOfDefault_v2 and quote the PD implied rating, labelled as a model output.
3. For rated peers: getCreditOpinion with sections FactorsLeadingToDowngrade, RatingOutlook and KeyIndicatorsTable, one peer per call. Record each peer's downgrade leverage trigger and its latest Moody's-adjusted leverage with the period end. If KeyIndicatorsTable does not return key indicators, take leverage from the Summary or RatingOutlook text and mark the rest [GAP]. Compare the opinion date with the latest rating action; if the opinion is older, say so.
4. Scorecard: getCreditOpinion with RatingFactors for the target or closest rated peer, to show the published scorecard. Try getEntityScorecardV2 with a what-if at the proposed leverage and cover only if its retrieval shows current subfactor values for the entity, and label any result as a hypothetical model output, never a rating.
5. Table: company, rating or PD implied rating (labelled), outlook, Moody's-adjusted leverage and period, downgrade trigger, gap between the two.

Then write one paragraph placing the proposed leverage against this table. Label the whole section "Context from rated peers. This is neither a financing quote nor a view on lender appetite." Note that Moody's-adjusted leverage includes adjustments (for example leases) that our model's leverage may not.
```

**Output:** Entity matches, a peer credit table with triggers, any scorecard what-if, and one labelled paragraph for the memo.

**Moody's:** As written. In a live test, `getCreditOpinion` on a B2-rated sponsor-backed borrower returned a downgrade trigger of debt to EBITDA sustained above 7.5x against Moody's-adjusted leverage of 8.2x, which is the kind of comparison this table is for. A PD on a private operating company reflects that entity's own accounts; acquisition debt may sit at a holding company above it, so say which entity each figure belongs to.

**Tested:** On two B2-rated software borrowers (McAfee, Epicor) the scorecard what-if returned "No subfactor values available to score for this methodology"; the retrieval (about 70,000 characters) held the methodology grid but no entity values. `RatingFactors` gave the published scorecard instead. Across six rated software peers, ratings and downgrade triggers came back in two calls, but `KeyIndicatorsTable` returned no key indicators in a multi-peer call and the liquidity section when asked alone; `getEntityFinancials_v2` has no Moody's-format figures for these private borrowers. One peer matched an LLC carrying only a facility rating, and its credit opinion predated a downgrade by 11 months.

**Follow-ups:**
- "Which peers have rating actions in the last [12] months? Use getEntityRatings history and searchNews."
- "Run the scorecard what-if at the downside case leverage in year two."

**Adjust for:** Lower mid-market: expect few rated peers and say so; use PD implied ratings from `getEntityProbabilityOfDefault_v2` with the `peers` category for percentile position. Large-cap: add tranche-level ratings and any traded debt prices from the fund's market-data source.

---

## Valuation and the case for the price

### IC-07 Valuation cross-check

**Use case:** Test the entry price against comparable transactions and trading multiples, keeping each data source in its lane.

**Inputs:** Comparable transaction and trading multiples exported from the fund's licensed market-data source, with dates; the entry multiple from IC-01; the peer list.

**Prompt:**
```
Cross-check the entry price for [Company] of [x] times [EBITDA basis] for [period].

1. Comparable transactions: from the attached export, list each deal with date, target, buyer, enterprise value, multiple, basis (trailing or forward) and source. Exclude deals older than [years] or outside [criteria] and say why for each exclusion.
2. Trading multiples: listed peers with enterprise value to EBITDA and to revenue, date and basis, from the attached export.
3. For each peer, put beside the multiple its growth, EBITDA margin and leverage, so the committee can see why it trades where it does.
4. Show where our entry multiple sits against the median and range of each set, and on the same EBITDA basis.
5. State reasons the target deserves a premium or discount, each with evidence from diligence.

Every multiple must come from the attached export. If a multiple is missing, write [GAP] and do not fill it.
```

**Output:** Two comparable tables with exclusions, peer fundamentals beside multiples, and a positioning view with evidenced reasons.

**Moody's:** `getEntityPeers` then `getEntityFinancials_v2` supplies peer growth, margins and leverage for step 3, and `getEntityRatings` adds credit context (quote the rating type). Filter the peer list per standing rule 12a and say for each peer whether its margin and leverage are reported, company-adjusted or Moody's-adjusted; do not mix bases in one median without saying so. For a recent take-private target, take its own figures from the annual report or the buyout borrower's credit opinion (standing rule 9c). Moody's does not supply equity multiples or transaction values.

**Tested:** On a UK instrumentation group, two of seven returned peers were the target's own group companies and one had been acquired in 2023; a fourth peer's older rows showed EBITA above revenue. With no market-data export, every multiple stayed [GAP], as it should.

**Follow-ups:**
- "Recompute the medians excluding the two least comparable peers, and say which they are."
- "Which comparable deal is the seller most likely to cite, and how does the target differ from it?"

**Adjust for:** Take-private: add the undisturbed share price history and premium analysis. Specialist funds: start with the fund's own prior deals in the sector. Growth equity: use revenue multiples and test the last round's terms.

### IC-08 What must be true

**Use case:** Turn the return into a short list of conditions the deal team must believe, each with a way to check it.

**Inputs:** Returns model, attribution, commercial hypotheses and their evidence grades, findings register.

**Prompt:**
```
For [Company] to deliver the base case return of [MOIC and IRR], list what must be true. Aim for five to eight statements.

For each statement: the assumption in plain words; the model input it drives and its value; the evidence we hold and its grade (strong / partial / none); what would show it is false; how and when we would know after closing (the first metric or event to watch).

Then list separately what would have to be true to reach the hurdle of [hurdle] if the base case does not reach it. Do not change the base case to get there.
```

**Output:** A must-be-true table with evidence grades and early indicators, plus a separate hurdle gap list where needed.

**Moody's:** Not needed. Where a statement depends on a counterparty or sector, `searchAllDocuments` with sector and region filters adds Moody's research as context.

**Follow-ups:**
- "Which statement has the weakest evidence and the largest effect on returns?"
- "Turn the early indicators into the first 100-day monitoring list."

**Adjust for:** Thematic funds: add a statement on adoption of the theme and a case where it arrives [five] years late. Carve-out: add a statement on achieving standalone costs by [date].

---

## Committee paper and challenge

### IC-09 Investment committee memo draft

**Use case:** Draft the committee paper from diligence outputs, with every number traced.

**Inputs:** The returns model and checks, IC-03 to IC-08 outputs, financial, commercial and management diligence summaries (management at role level), the findings register, the fund's memo template.

**Prompt:**
```
Draft the investment committee memo for [Company] using the [fund template / sections below].

Sections: recommendation in three lines from PROCEED / PROCEED WITH CONDITIONS / DO NOT PROCEED; the deal in one paragraph (price, structure, timetable); investment thesis; what must be true; financial diligence (three EBITDA rows: reported, management-adjusted, fund-accepted); commercial diligence; management at role level; returns with the checks table first, then four cases, attribution and sensitivities; financing and stress, including the labelled context from rated peers; valuation cross-check; key risks and mitigants; conditions; open items.

Rules:
- Every number matches its model cell or diligence report; cite it.
- A CONFIRMED CRITICAL finding, a breach of a fund hard rule without an evidenced mitigant, or any check at Fail or Not built caps the recommendation at PROCEED WITH CONDITIONS, and the memo says which applies.
- Keep the seller's claims, our evidence and our conclusions visibly apart.
- Mark the draft "Draft for partner review. Not an approved position."
```

**Output:** A full memo draft with a traced recommendation and visible checks.

**Moody's:** Include the IC-06 table and the external corroboration table from DD-36 as an appendix.

**Follow-ups:**
- "Cut this to a two-page summary that leads with the effect on price."
- "List every sentence that states a conclusion without a citation."

**Adjust for:** Buy-and-build: add the add-on pipeline and integration capacity section. Carve-out: add a separation-risk section. Take-private: flag takeover-rule and financing-certainty points for counsel and restrict distribution. Minority: add governance, information rights and exit mechanics.

### IC-10 Pre-mortem

**Use case:** Find the most likely ways this deal loses money before the committee commits.

**Inputs:** The memo draft, the findings register, the returns model.

**Prompt:**
```
Assume it is [year of exit] and [Company] has returned less than the equity we invested. Write the story of why.

Give the five most plausible causes, each one: what happened; the earliest sign we would have seen; the finding or assumption in today's material it traces back to (cite it); what the downside case already captures and what it misses; and what we could do now (price, structure, condition, plan) to reduce it.

Then argue the bear case in one page with the same rigour as the base case in the memo. End with whether any cause should change the recommendation, and why.
```

**Output:** Five traced failure stories, a bear case and a view on the recommendation.

**Moody's:** `getCreditOpinion` on the group's rated issuer (standing rule 9b), with `CreditChallenges`, `FactorsLeadingToDowngrade`, `RatingOutlook` and `KeyIndicatorsTable`, gives the causes a rating analyst already sees; trace each cause to one of them where possible. `getEntityProbabilityOfDefault_v2` with a 12-month history on the borrowing entity shows whether the model sees deterioration. `searchNews` on the target and, for business-to-business companies, on its largest customers over the past [365] days, with `getEntityProbabilityOfDefault_v2` and `early_warning` on those customers.

**Tested:** On a UK gym group with no memo or model, the rated borrower's credit opinion (interest cover near 1.0x, negative free cash flow, a 6.0x downgrade trigger against a 6.2x forecast) and a debt-funded acquisition announced after it supplied the two leading causes. The "what the downside case captures" column needs the fund's model and stays [GAP] without it. The early-warning history was not returned for a date window; only the latest category came back.

**Follow-ups:**
- "Build a model case that combines the two most likely causes and show the return."
- "Which cause would the committee raise first?"

**Adjust for:** Turnaround: include a cause where liquidity runs out before the plan works. Growth equity: include a cause where the next round fails or prices lower.

### IC-11 Committee question rehearsal

**Use case:** Prepare for the hardest questions the committee will ask, with answers grounded in evidence.

**Inputs:** The memo draft, IC-10 output, the findings register, the open items list.

**Prompt:**
```
List the [15] hardest questions the investment committee is likely to ask about [Company], ordered by how much a weak answer would damage the case.

For each question: the answer in no more than three sentences; the evidence with locators; what we do not know and admit; and the follow-up a sceptical member would ask next, with its answer.

Include questions on price, the downside case, management, the largest risk in the register, the exit route and buyer universe, and anything in the memo with an evidence grade of "none".
```

**Output:** A ranked question set with evidenced answers, admitted gaps and follow-ups.

**Moody's:** Not needed.

**Follow-ups:**
- "Which questions can we not yet answer, and what would close each gap before the meeting?"

**Adjust for:** Large-cap: add questions on syndication of equity and on merger-control timing.

---

## Conditions

### IC-12 Conditions tracker for signing and closing

**Use case:** Track every condition that stands between the committee's approval and closing.

**Inputs:** The committee's decision and conditions, the challenged findings register, the draft purchase agreement and financing commitments.

**Prompt:**
```
Build the conditions tracker for [Company].

One row per condition: ID; condition in plain words; source (committee minute, finding ID, agreement clause); type (before signing, before closing, post-closing undertaking); evidence required to treat it as met; owner; due date; status (open / in progress / evidence received / met / waived, with who waived it); and the date and locator of the evidence.

Include: conditions set by the committee; purchase-agreement conditions such as regulatory clearances and third-party consents; financing conditions; and diligence items marked as conditions in the register. Flag any condition whose due date falls after the planned signing or closing date of [date].

Do not mark a condition met without the evidence named in its row.
```

**Output:** A conditions tracker with owners, evidence tests and date flags.

**Moody's:** Not needed.

**Follow-ups:**
- "List every condition that is open within [ten] business days of its due date."
- "Draft the status note for the managing partner on conditions still open."

**Adjust for:** Carve-out: add the transition services agreement and each separation milestone as conditions. Regulated sectors: add regulator approval of the new owner. Minority: add the shareholder agreement terms on information rights and board seats.
