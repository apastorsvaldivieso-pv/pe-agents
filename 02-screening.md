# Screening prompts

Use these when a teaser or information memorandum arrives, to decide quickly and on evidence whether a deal deserves the cost of diligence. Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach the documents named under **Inputs**, fill the bracketed fields and run it. Screening works from the seller's material and a short external check. Everything it produces is a first view; deals that advance move to `03-due-diligence.md`.

**How each entry is laid out**

- **Use case:** the job, in one line.
- **Inputs:** what to attach or connect.
- **Prompt:** the text to paste.
- **Output:** what should come back.
- **Moody's:** which connector tools to use and what they add. Every prompt also works without the connector.
- **Follow-ups:** prompts for drilling into the answer.
- **Adjust for:** how to change it for other strategies, size bands and fund types.

**Use cases in this file**

| Area | IDs |
|---|---|
| First read | SC-01 to SC-03 |
| Numbers and risks | SC-04 to SC-06 |
| Fit and external evidence | SC-07 to SC-09 |
| Price and returns | SC-10 to SC-11 |
| Decision and next steps | SC-12 to SC-15 |

---

## First read

### SC-01 Teaser read

**Use case:** Decide in a few minutes whether a teaser (the short anonymised summary an adviser sends first) is worth signing a confidentiality agreement for.

**Inputs:** The teaser; the fund's buy box.

**Prompt:**
```
Read this teaser and give me a first view against our buy box: [sector or sectors], [geography], revenue [x to y], EBITDA [x to y], [control / minority], hard rules [for example no customer above 25% of revenue], exclusions [list].

1. Extract, as stated: what the business does, geography, revenue, EBITDA and margin with period and currency, growth, recurring share, customer and employee numbers, ownership, reason for sale, process and timetable, adviser.
2. Check each buy-box criterion: Meets, Fails or Unknown, with the teaser locator.
3. List the three things we would most need to see in the information memorandum.
4. Say what the anonymised description could correspond to only if the teaser makes it obvious; otherwise leave identity blank. Do not guess.

Verdict: sign the confidentiality agreement, ask a question first, or decline. Give the single criterion that decides it.
```

**Output:** Extracted facts, a buy-box check, three questions and a verdict with its deciding criterion.

**Moody's:** Not needed until the company is named. Once it is, run SC-08.

**Follow-ups:**
- "Draft a two-line reply to the adviser declining, with the reason we can share."
- "What would the business need to show for us to change a Fails to Meets?"

**Adjust for:** Buy-and-build: add whether the teaser describes a platform or an add-on for an existing platform. Carve-out: teasers often show segment figures; note whether they include allocated central costs. Growth equity: add funding history and the use of proceeds. Specialist funds: check the house KPIs the teaser reports and the ones it leaves out.

### SC-02 Information memorandum teardown

**Use case:** Pull every material fact out of the information memorandum, as stated, with where it came from.

**Inputs:** The information memorandum (also called a confidential information memorandum or CIM) and any appendices.

**Prompt:**
```
Extract the facts from the information memorandum for [Company] into structured sections. Label every figure "as stated" and give its locator (page and section or table).

Sections: business description; products and services with revenue share; customers (numbers, top customer shares, sectors, tenure); geography; market claims and their cited sources; competitors named; historical financials by year (revenue, gross profit, EBITDA, adjusted EBITDA, capital expenditure, net debt, cash flow, with period, currency and basis); forecast by year; management team and ownership; employees and sites; transaction terms (perimeter, process, timetable, what is for sale).

For each section, list what the document does not say that a buyer would expect it to say, as [GAP: ...]. Re-add every total and percentage you extract. Put any total that does not foot on the Contradicted list with the arithmetic.
```

**Output:** A structured fact base with locators, gaps per section, and footing checks.

**Moody's:** Not needed for extraction. Use SC-08 to compare the extracted figures with filed accounts.

**Follow-ups:**
- "Show every number that appears more than once in the document and whether the figures agree."
- "List the forecast years and the growth rate each implies."

**Adjust for:** Carve-out: add a section on the perimeter, the services the parent provides and any transition arrangements described. Take-private: there is usually no memorandum; run the same extraction on the annual report and investor presentation. Lower mid-market: memoranda are often short; expect more gaps and say so plainly.

### SC-03 Claims tracker

**Use case:** Separate what the seller asserts from what the material supports.

**Inputs:** The information memorandum or SC-02 output; any management presentation.

**Prompt:**
```
List every claim in the material on [Company] that a buyer would rely on: market position, market size and growth, customer satisfaction and retention, pricing power, technology, pipeline, cost savings, forecast drivers, and any superlative ("leading", "unique", "best-in-class").

Put each claim in exactly one list:
- Asserted: stated with no supporting data in the material.
- Contradicted: another part of the material, or its own numbers, disagree. Show both passages and the arithmetic.
- Missing: the claim depends on data we do not have. Write the request that would close it.

Also mark claims that are supported by data within the material as Supported, with the locator of the data. For each claim, give the locator of the claim itself and one line on why it matters to the price.
```

**Output:** A claims table split into Supported, Asserted, Contradicted and Missing.

**Moody's:** `getEntityFinancials_v2` and `getEntityPeers` can move a claim about scale or margin from Asserted to Supported or Contradicted; record the Moody's figure beside the claim. Check that the largest named competitors appear in the peer list; if not, add them by name with `findEntity`. A rated competitor's credit opinion (`getCreditOpinion`) can test claims about market position. Where the material is retrieved by search (`searchCompanyFilings`), say that a claim is unsupported "in the pages retrieved".

**Follow-ups:**
- "Which three Asserted claims carry most of the forecast?"
- "Turn the Missing list into the NEED MORE request (SC-14)."

**Adjust for:** Growth buyout and growth equity: put claims about retention, pipeline and unit economics first. Thematic funds: add claims about how much revenue the theme drives. Turnaround: add claims about the cause of decline and recovery already under way.

---

## Numbers and risks

### SC-04 First-pass earnings adjustments

**Use case:** Decide how much of management's adjusted EBITDA to believe at first look.

**Inputs:** The adjusted EBITDA schedule in the information memorandum; the historical profit and loss.

**Prompt:**
```
For [Company], list every adjustment between reported and management-adjusted EBITDA for [years and last twelve months]. For each: description, amount by year, locator, and type (non-recurring item, owner or related-party cost, accounting-policy normalisation, run-rate or pro forma).

Grade each one:
- Supported: documented in the material and plausibly one-off.
- Reasonable: plausible, but only partly documented.
- Unsupported: no documentation, recurs in more than one year, relies on an event that has not happened, or is a future synergy.

Look for one-off income that was not removed (asset sales, insurance receipts, provision releases, grants) and list it.

Show three rows by year: reported EBITDA, management-adjusted EBITDA, and our first-pass EBITDA (reported plus Supported items, less one-off income). Give the gap between management-adjusted and first-pass, in amount and percentage, and the share of adjusted EBITDA that comes from adjustments.
```

**Output:** An adjustment schedule with grades, a three-row bridge and the gap.

**Moody's:** `getEntityFinancials_v2` gives filed reported EBITDA for the legal entity to compare with the memorandum's reported row; say whether the entity matches the perimeter for sale. For rated companies, or where the group has a rated issuer (standing rule 9b), `getCreditOpinion` with `KeyIndicatorsTable` gives Moody's adjusted figures as a third view. If the section returns no key indicators, take the figures from the Summary text or mark them [GAP] (standing rule 11a).

**Follow-ups:**
- "Show first-pass EBITDA if every Reasonable item is also accepted, and the effect on the implied multiple."
- "Which adjustments should be the first questions on the first call?"

**Adjust for:** Lower mid-market: expect owner costs (salaries, vehicles, family members on payroll); test each against what a replacement would cost. Carve-out: add a line for standalone costs the parent provides today, marked as missing unless the memorandum quantifies them. Growth equity: adjustments matter less than burn and unit economics; keep the list short.

### SC-05 Concentration quick check

**Use case:** See quickly how dependent the business is on a few customers, suppliers, products or people.

**Inputs:** The information memorandum; any customer or supplier tables.

**Prompt:**
```
For [Company], measure concentration from the material:

1. Customers: share of revenue from the largest 1, 5 and 10 customers by year; any customer above our hard rule of [25%]; tenure and contract status of the largest [five].
2. Suppliers: any supplier above [20%] of purchases, and whether alternatives are named.
3. Products and channels: share of revenue from the largest product and the largest channel.
4. Geography: share of revenue from the largest country or region.
5. People: whether the founder or one person is named on the largest customer relationships or contracts.

For each measure, say whether the material gives it, partly gives it or omits it. A breach of the hard rule makes the deal NEED MORE at best until mitigated; state the mitigant if the material offers one.
```

**Output:** A concentration table, a hard-rule check and the mitigants offered.

**Moody's:** If customers are named, `findEntity` on the largest [five] in one call, then `getEntityRatings` on each (state the rating type: corporate family, issuer or senior unsecured) and `getEntityProbabilityOfDefault_v2` with `early_warning`. Show the assigned rating and the PD implied rating side by side (standing rule 11) and flag a gap of three notches or more. A concentrated customer with weak credit is a bigger risk than concentration alone. Where there is no memorandum, the customer note in the annual report (`searchCompanyFilings`) names customers above the 10% disclosure threshold; cite it by document and page, because the returned link expires after five minutes.

**Tested:** On a US auto supplier the 10-K gave the three largest customers (44%, 15% and 13% of 2025 sales). Their ratings were of three different types, and one customer rated Baa3 had a PD implied rating of B3 with early warning "Severe". The supplier's own record showed a rating withdrawn in 2017 because Moody's rates another entity in its group (standing rule 9b).

**Follow-ups:**
- "What happens to EBITDA if the largest customer leaves, using the gross margin in the material?"

**Adjust for:** Buy-and-build: show concentration after combining with the platform. Specialist funds: compare with concentration in the fund's prior deals in the sector. Lower mid-market: owner dependence is usually the larger risk; put it first.

### SC-06 Red-flag scan

**Use case:** A first stoplight on the issues that most often stop deals at screening.

**Inputs:** The information memorandum and any other seller material.

**Prompt:**
```
Scan the material on [Company] for these red flags: customer concentration; margin deterioration; ownership or capitalisation-table issues; owner or key-person dependence; a high share of adjusted EBITDA from adjustments; licences held by the seller personally or that will not transfer; missing or unaudited financials; active litigation or regulatory investigation; any mandate exclusion [list].

For each area, report either [NONE FOUND IN MATERIALS] or the evidence, quoted with its locator, its severity (CRITICAL / MAJOR / MINOR / INFO), and the question it raises.

Overall signal: RED if any CRITICAL flag; AMBER if any MAJOR flag; GREEN otherwise. Do not invent concerns to look thorough.
```

**Output:** A flag table with evidence and an overall signal.

**Moody's:** `findEntity` on the company and its parent (standing rule 9a: if the parent is missing from both lists, take its BvD ID from `getEntityUltimateOwners` and resolve it by ID). `getEntityRatings` on the rated issuer in the group (standing rule 9b): a rating of Caa or below, or a recent default, is itself a CRITICAL flag. `getShellCompanyIndicators` on the company and its parent adds corporate-structure indicators; report director flags as INFO unless another category is also found (standing rule 13). `searchNews` by entity with litigation and regulatory keywords; for a widely covered company run it in quarterly windows, because a single [365]-day call can fill with the last few days (standing rule 12b).

**Tested:** On a UK water utility under creditor-led recapitalisation, a 365-day news search returned only the last five days; the parent was dropped by `findEntity` without appearing in "not found"; 77 of 86 directors were flagged. The assigned rating (Caa3) was the clearest single flag.

**Follow-ups:**
- "Which flags could be cleared by one document? Name it."

**Adjust for:** Regulated sectors: add licence status and regulator correspondence as separate areas. Turnaround: add liquidity, covenant breaches and overdue creditors. Carve-out: add entanglement with the parent (shared contracts, systems, people).

---

## Fit and external evidence

### SC-07 Mandate fit and score

**Use case:** Score the deal against the fund's thesis and give a verdict.

**Inputs:** Outputs of SC-02 to SC-06; the fund's screening rubric and weights.

**Prompt:**
```
Screen [Company] against our mandate and score it out of 100:

Thesis and mandate fit [15]; revenue quality [20]; profitability and earnings quality [15]; market position [10]; management strength [10]; growth outlook [10]; exit potential [10]; deal and structure [10].

For each dimension: score, evidence with locator, and evidence grade (Documented, Reported, Asserted only). Bands: Strong [75] and above (advance), Conditional [55] to [74] (advance with conditions or NEED MORE), Pass below [55].

If a dimension cannot be evidenced, mark it Not scored, rescale the total to 100 and say so; do not score management on reputation or impressions.

A breach of a hard rule [list] caps the result at Conditional, whatever the score, until mitigated. An off-mandate sector or geography, or a mandate exclusion, is a PASS.

Give the verdict as ADVANCE, PASS or NEED MORE, with the single deciding criterion and its evidence. A PASS names the rule that failed. A NEED MORE names the exact document needed.
```

**Output:** A scored rubric with evidence grades, the band, any hard-rule cap and a verdict with its deciding criterion.

**Moody's:** Scores for revenue quality, earnings quality and market position gain weight when SC-08 has corroborated the figures; mark which dimensions it supports.

**Follow-ups:**
- "Which two dimensions would have to improve for the deal to reach Strong, and what evidence would do it?"
- "Argue the case for the band below."

**Adjust for:** Carve-out: replace management strength with perimeter clarity and standalone cost visibility. Take-private: replace deal and structure with execution complexity under takeover rules. Turnaround: replace growth outlook with fixability of the decline and liquidity runway. Growth equity and minority: replace deal and structure with governance rights and exit route. Specialist funds: score market position against house benchmarks.

### SC-08 Quick external check with Moody's

**Use case:** In under ten minutes, check who the company is, who owns it, what its filed accounts say, its credit standing and recent news.

**Inputs:** The company's registered name and country; the headline figures from the memorandum.

**Prompt:**
```
Run a quick external check on [Company] (registered name [legal name], country [country]). Keep it short.

1. Identity: matched legal name, country, identifier. If the match looks wrong, stop and show me the alternatives.
2. Owner: ultimate owner and the chain to it. If the chain stops at an acquisition vehicle, name the vehicle and its direct shareholders.
3. Filed financials: revenue, EBITDA, net debt and employees for the latest [three] years, with period end and whether consolidated or entity level. Mark stale periods and missing years. If the company's revenue is below that of one of its own subsidiaries, the figures are entity level; say so and do not use them as group figures.
4. Credit: run getEntityRatings on the company and on every holding or financing entity found in steps 2 and 3 (names with Holdings, Bidco, Topco, Midco or Finco). Quote any assigned rating with its type and outlook. Only for unrated entities, give the PD and PD implied rating, labelled as model output, and say which entity it relates to.
5. News: significant items in the last [180] days.

Then a short table: memorandum says, Moody's shows, difference. Put any difference above [5%] on the Contradicted list. End with three lines: what we learned that the memorandum did not say.
```

**Output:** Identity, owner, filed figures, credit standing, news, a comparison table and three lines of new information.

**Moody's:** `findEntity`, `getEntityUltimateOwners` (threshold [25.01]), `getEntityShareholders` if the chain stops at a vehicle, `getEntityFinancials_v2`, `getEntityRatings` or `getEntityProbabilityOfDefault_v2`, and `searchNews` with `importance` set to significance. Search by registered name: in a test, a trading-style name matched a company in the wrong country. Filed financials belong to the legal entity named, which may not be the group for sale; PD is on that entity's own accounts, and acquisition debt may sit at a holding company above it.

**Tested:** For one sponsor-owned UK leisure group the matched entity returned EBITDA higher than its revenue, so it was a property or financing company inside the group. Standing rule 9c catches this; check the subsidiaries and ultimate owner to find the trading and borrowing entities. On a UK restaurant group the named company's 2025 revenue (GBP 221.0m) was below its own subsidiary's (GBP 480.53m), the rated issuer (B2, outlook negative) sat in its subsidiary list, and a name search for the holding vehicle returned a different company; a BvD ID lookup found the right one (standing rule 9a).

**Follow-ups:**
- "Pull the same check for the holding company that carries the debt."
- "Add five peers with their margins and leverage, with the reason each is comparable."

**Adjust for:** Carve-out: run it on the parent and use `getEntitySegmentFinancials` for the division. Take-private: add `searchCompanyFilings` on the latest annual report and earnings call. Lower mid-market: expect abbreviated accounts or no coverage; mark [MOODY'S: NOT COVERED] and use the registry filing directly.

### SC-09 AI exposure at first look

**Use case:** Classify early whether AI helps or threatens the business, before spending on diligence.

**Inputs:** The memorandum's description of products, customers and cost base.

**Prompt:**
```
From the material on [Company], classify its exposure to AI as one of:
- Improves the business: AI lowers costs or improves the product without changing the model.
- Forces change: real upside, but the company must change how it works or sells.
- Threatens the model: AI could make the core product or service unnecessary or much cheaper.

Answer briefly, with evidence from the material where it exists: What do customers pay for, and could an AI tool or a customer's own team do it? Which parts of the cost base are tasks AI already performs? Does the memorandum mention AI at all, and does it address the risk?

Give your confidence (high, medium, low) and the two questions that would most change the classification. Say what this could mean for the exit multiple a buyer would pay in [exit year], in words only.
```

**Output:** A classification with evidence, confidence, two questions and an exit implication in words.

**Moody's:** `searchAllDocuments` with the sector filter and recent research for Moody's writing on technology disruption in the sector; cite only relevant hits.

**Follow-ups:**
- "Which revenue lines are most exposed? Give their share of revenue as stated."

**Adjust for:** Growth equity: add whether the company's own product uses AI and whether that is a lasting advantage. Specialist software funds: test against the house view on AI substitution. Long holds: weigh the threatens-the-model case more heavily.

---

## Price and returns

### SC-10 Valuation sanity check

**Use case:** Work out what the asking price implies and what a buyer at that price is betting on.

**Inputs:** The asking price or price guidance; SC-04 first-pass EBITDA; peer and precedent multiples from the fund's market-data source.

**Prompt:**
```
The guidance for [Company] is [price or range] for [perimeter], on a [cash-free, debt-free / other] basis.

1. Compute the implied enterprise value multiple on management-adjusted EBITDA and on our first-pass EBITDA, for the last year, the current year and the first forecast year.
2. Set these beside the peer and precedent multiples I supply from [market-data source and date]. Do not source multiples anywhere else.
3. Write what the price implicitly bets on: the growth, margin, adjustments and exit multiple that must hold for a buyer at this price to earn [target return]. List each as a plain statement.
4. Say which of those bets the material supports, which it asserts, and which it does not address.

Show all arithmetic in a table. This is a screening check. It is not a valuation.
```

**Output:** Implied multiples, a comparison with supplied benchmarks, the implicit bets and their evidence status.

**Moody's:** Moody's does not supply share prices, market capitalisations or equity multiples; those come from the fund's market-data source. `getEntityPeers` with `getEntityFinancials_v2` gives peer margins and growth to test whether a premium multiple is backed by better performance (filter the peer list per standing rule 12a). Where the target or its buyout borrower is rated, `getCreditOpinion` gives Moody's-adjusted EBITDA and forecasts (standing rule 11a); show it beside management-adjusted EBITDA, because the gap between the two is part of what the price bets on.

**Tested:** On a UK instrumentation group taken private in 2025, the delisted company's record held only entity-level 2025 figures (revenue GBP 32.6m, negative EBITDA) and no earlier years. Group EBITDA came from the 2024 annual report (`searchCompanyFilings`, third attempt) and from the buyout borrower's credit opinion; management-adjusted 2024 EBITDA was about 41% above Moody's-adjusted. News search also returned 2026 articles describing the company as still listed; discard items that contradict a known delisting.

**Follow-ups:**
- "At what price does the deal work on our first-pass EBITDA and base case?"
- "How much of the implied value depends on the forecast years over the historical years?"

**Adjust for:** Take-private: replace guidance with the undisturbed share price and a stated premium range, both from the market-data source. Growth equity: use a revenue multiple and pre-money valuation, and add the preference terms. Carve-out: include standalone costs before computing the multiple. Turnaround: value on normalised earnings and state the assumption.

### SC-11 Quick returns sketch

**Use case:** See whether the deal can plausibly reach the fund's return hurdle before diligence.

**Inputs:** Price from SC-10; first-pass EBITDA from SC-04; the fund's leverage posture and return hurdle.

**Prompt:**
```
Build an indicative returns sketch for [Company]. Label every output "Indicative, for screening only".

1. Sources and uses: enterprise value [x]; fees [x% or amount]; debt at [x] times first-pass EBITDA, within our posture of [maximum leverage]; equity as the balance.
2. Simple projection for [hold period] years: EBITDA growth at [base rate], cash conversion at [x%], all free cash flow to debt repayment.
3. Three exit cases: downside (EBITDA growth [x%], exit multiple [x]); base ([x%], [x]); upside ([x%], [x]).
4. For each case: exit enterprise value, net debt at exit, equity value, money multiple and approximate IRR.

Put every assumption in a separate table with its source: the memorandum, our first-pass work, or a fund assumption. Anything not from the material is a fund assumption and must say so. Show the calculation in a table or code.
```

**Output:** Sources and uses, a simple projection, three exit cases with money multiple and IRR, and an assumptions table.

**Moody's:** For rated peers, `getCreditOpinion` with `FactorsLeadingToDowngrade` shows the leverage levels at which ratings come under pressure, and `getEntityRatings` shows how similar borrowers are rated. Use them as context for the leverage assumption. Neither is a financing quote. Their triggers are on Moody's-adjusted figures, which include leases; say whether the sketch's leverage is on the same basis (for lease-heavy businesses, EBITDA before or after rent changes the multiple materially).

**Follow-ups:**
- "What entry price gives our hurdle of [x] in the base case?"
- "Which single assumption moves the base-case return the most?"

**Adjust for:** Growth buyout: use lighter leverage and make growth the main return driver. Growth equity and minority: no acquisition debt; model ownership share, dilution and the preference stack. Buy-and-build: add a line for add-on spend and the EBITDA bought, at stated multiples. Turnaround: add a cash injection and a recovery period before growth. Large-cap: add financing fees and a refinancing assumption.

---

## Decision and next steps

### SC-12 Go / no-go memo

**Use case:** Put the screening decision to a partner in two pages, with the case on both sides.

**Inputs:** Outputs of SC-02 to SC-11.

**Prompt:**
```
Write a two-page go / no-go memo on [Company] for [partner].

Sections:
1. Recommendation: ADVANCE, ADVANCE WITH CONDITIONS, NEED MORE (name the document) or PASS, with the score and the deciding criterion, consistent with the SC-07 verdict.
2. Deal snapshot: company, sector, geography, size, price guidance, implied multiple, process and timetable.
3. The case for: up to five points, each with evidence.
4. The case against: up to five points, each with evidence.
5. Must be true: the three to five conditions that must hold for the deal to work, each with the diligence work that would test it and its likely cost.
6. Red flags and open gaps.
7. Indicative returns, labelled as such.
8. Next step and decision needed, with a date.

Write the case against as strongly as the case for. Mark the memo "Initial draft for partner review".
```

**Output:** A two-page memo with a recommendation, balanced cases, must-be-true conditions and the next step.

**Moody's:** Include the SC-08 comparison table as an appendix, and say which facts in the memo came from it.

**Follow-ups:**
- "Which must-be-true condition is cheapest to test first?"
- "Cut this to half a page for the weekly pipeline meeting."

**Adjust for:** Take-private: circulate only to the deal partner and General Counsel and add the not-advice footer from SO-12. Carve-out: add standalone cost and separation complexity to the must-be-true list. Minority and growth equity: add governance rights and exit route to the must-be-true list.

### SC-13 First-call questions

**Use case:** Use the first call with the adviser or management on what will decide the screen.

**Inputs:** The claims tracker, red flags, earnings adjustments and must-be-true list.

**Prompt:**
```
Prepare a [45]-minute first call on [Company] with [the adviser / management].

Give: the [ten] most important questions in order, each tied to a gap, contradiction or must-be-true condition; a follow-up for a vague answer to each; what answer would move us towards PASS; and the documents to ask for at the end of the call.

Open and neutral questions first, specific ones later. Do not reveal our price view or our concerns about specific numbers in the wording.
```

**Output:** An ordered question list with follow-ups, deal-breaking answers and a document request.

**Moody's:** Not needed. Contradictions from SC-08 feed the questions.

**Follow-ups:**
- "After the call: here are my notes. Update the claims tracker and the score."

**Adjust for:** Lower mid-market founder calls: fewer questions, more time on why they are selling and what they want afterwards. Carve-out: add questions for the parent on separation and transition services. Take-private: do not use this prompt; contact is a matter for counsel.

### SC-14 NEED MORE request

**Use case:** Turn the screening gaps into an email to the adviser that gets the right documents back.

**Inputs:** The Missing list from SC-03; gaps from SC-02 and SC-05; the verdict from SC-07.

**Prompt:**
```
Write an email from [sender] to [adviser name, firm] asking for the information we need to complete our screen of [Company / project name].

Rules: a short opening thanking them and confirming our interest; a numbered list of requests, most important first, each naming the exact document or data, the period, and the format (for example monthly, customer level, Excel); one line on why we need the first [three] items; a requested date of [date]; a closing line offering a call.

Ask for underlying data and executed documents, so the request cannot be answered with a summary. Keep to what decides the screen, at most [eight] items. Do not reveal our score, our price view or the specific concerns behind each request. Plain, courteous tone. Draft for my review; do not send.
```

**Output:** A sendable email draft with a numbered request list.

**Moody's:** Not needed.

**Follow-ups:**
- "Which of these requests is the adviser most likely to resist, and what is the fallback ask?"

**Adjust for:** Lower mid-market: write to the owner or their accountant in plainer language and ask for ledgers or bank statements where schedules do not exist. Carve-out: address the parent's deal team and ask how central costs are allocated. Growth equity: add the capitalisation table and existing investor rights.

### SC-15 Screen a batch of teasers

**Use case:** Sort a week's inbound teasers into advance, hold and pass in one sitting.

**Inputs:** Several teasers; the fund's buy box.

**Prompt:**
```
Screen these [number] teasers against our buy box: [sectors], [geography], revenue [x to y], EBITDA [x to y], [control / minority], hard rules [list], exclusions [list].

For each teaser, one row: project name, adviser, sector, geography, revenue and EBITDA as stated with period, the buy-box criteria it fails or leaves unknown, the most interesting point, and a decision:
- Advance: meets the buy box on the stated figures; sign the confidentiality agreement.
- Hold: one or two Unknowns that a single question would settle. Write the question.
- Pass: fails a named criterion. Name it.

Sort the table Advance, Hold, Pass. Flag any two teasers that may describe the same company or competing companies. Do not guess the identity of anonymised companies.
```

**Output:** A sorted triage table with decisions, questions for Hold and the failed criterion for Pass.

**Moody's:** Not needed at this step. Run SC-08 on each Advance name once it is identified.

**Follow-ups:**
- "Draft short decline notes for the Pass list, each giving the criterion we can share."
- "Which Hold teasers came from advisers we rank highly in SO-19?"

**Adjust for:** Buy-and-build: add a column for which platform each teaser could join as an add-on. Funds with several vehicles by size band: add a column routing each teaser to the right fund. Specialist and thematic funds: add a column for value-chain stage or theme exposure.
