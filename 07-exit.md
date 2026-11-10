# Exit prompts

Use these from about [12 to 18] months before an intended sale, and run the value attribution bridge each quarter after the valuation mark is approved. Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach the material named under **Inputs**, fill the bracketed fields and run it. Nothing here is a recommendation to sell or to deal in any security; the decision on when and how to sell belongs to the deal partner.

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
| Value attribution | EX-01 to EX-02 |
| Readiness and vendor-side diligence | EX-03 to EX-05 |
| Buyers and the equity story | EX-06 to EX-07 |
| Process preparation | EX-08 to EX-10 |

---

## Value attribution

### EX-01 Value attribution bridge

**Use case:** Show what created the value so far, in a bridge that sums and ties to the fund's books.

**Inputs:** Entry figures from the closing documents (revenue, EBITDA, enterprise value, net debt, equity invested); current trailing twelve-month revenue and EBITDA and net debt; the latest approved valuation mark; dividends paid and equity injected with dates, from the fund administrator.

**Prompt:**
```
Build the value attribution bridge for [Company] from entry on [date] to [date], for 100% of the company on one EBITDA definition (earnings before interest, tax, depreciation and amortisation). Do the arithmetic in code.

Notation: 0 is entry, 1 is now; R revenue, m EBITDA margin, E EBITDA, M valuation multiple (enterprise value divided by EBITDA), ND net debt.

Value created = current equity value + dividends paid - equity injected after entry - entry equity value.

Components:
- Revenue effect = (R1 - R0) x m0 x M0
- Margin effect = R1 x (m1 - m0) x M0
- EBITDA effect = revenue effect + margin effect, which must equal (E1 - E0) x M0
- Multiple effect = (M1 - M0) x E1
- Net debt effect = ND0 - ND1 + dividends paid - equity injected

Prove the components sum to value created. Show the residual (the difference between the bridge and the fund-administrator figure) on its own line with its causes (minority interests, preferred instruments, management equity, transaction costs, currency); never spread it across the other lines. Flag if the residual exceeds [5%] of value created.

State the convention: earnings growth is valued at the entry multiple and the multiple change on current EBITDA, so the cross term sits in the multiple effect. Note the other orderings (multiple change on entry EBITDA, or the cross term (E1 - E0) x (M1 - M0) on its own line), and the same choice between revenue and margin. Keep the convention fixed quarter to quarter.

Then the fund's share: value created times ownership, adjusted for dilution. Tie invested capital, distributions and multiple of invested capital to the fund administrator's records. If the EBITDA definition changed since entry, show the change as its own line.

The bridge describes where value sits. It does not prove which decision caused it and credits nothing to a person.
```

**Output:** A bridge that sums, a disclosed residual with causes, the fund's share and a tie-out to fund-administrator figures.

**Moody's:** Not needed. The valuation multiple comes from the fund's approved mark; Moody's does not supply equity multiples.

**Follow-ups:**
- "Show the same bridge under the convention that puts the cross term on its own line."
- "Link each component to the initiatives in the benefit ledger that drove it, where the ledger shows Realised benefits."

**Adjust for:** Turnaround: where entry EBITDA is negative or near zero a multiple means little; bridge enterprise value directly and say so. Take-private: show public-company costs removed as a named part of the margin effect. Growth equity: equity injected is often large, so check the net debt line carefully. Minority influence without control: use the fund's share throughout.

### EX-02 Organic versus acquired growth

**Use case:** Stop purchased earnings from looking like operating improvement.

**Inputs:** EX-01 bridge, each add-on's EBITDA at purchase and since, purchase dates, prices paid and equity used, the synergy tracker (VC-36).

**Prompt:**
```
For [Company], split EBITDA growth since entry into:
- Organic: growth of businesses owned throughout, plus each add-on's growth after its first full year of ownership (state the rule you use);
- Acquired: EBITDA at purchase of each add-on;
- Synergies: realised only, from closed accounts, kept apart from both.

Check the three sum to total EBITDA growth. Then show the EBITDA effect in the value bridge split the same way, with the equity and debt used to buy each add-on in the net debt line.

List add-ons that do not yet meet the platform's definition of "integrated", and show adjusted EBITDA with and without them.
```

**Output:** An organic, acquired and synergy split that sums, and a bridge using it.

**Moody's:** `searchNews` on the operating company with importance set to significance confirms the public record of each add-on deal (the live test found a portfolio company's add-on this way). Search news on the operating company, not the bond-issuing Bidco: news on a Bidco is dominated by fund filings that hold its bonds. `getEntitySubsidiaries` shows only direct or key subsidiaries (rule 12a), so confirm acquired entities from the subsidiary note of the latest annual report via `searchCompanyFilings` (rule 12c). Credit opinions and issuer comments on the rated issuer can give an add-on's price and EBITDA at purchase (rule 11b); the purchase multiple they quote is a fact about that deal, never a valuation benchmark.

**Tested:** Pure Gym group, November 2026. `getEntitySubsidiaries` listed 5 of 26 group companies and none of the acquired US or Swiss entities; the annual report listed all of them. Moody's research gave the one acquired EBITDA figure available. Without the fund's add-on figures the split could not be shown to sum, and the prompt correctly stopped a false organic figure. Round 2: research scoped to the rated issuer gave an add-on's first-full-year EBITDA on the company's own basis, which let the split sum with two stated gaps; news on the Bidco returned only bond-holding filings.

**Follow-ups:**
- "What was organic growth per year, and how does it compare with the underwriting case?"

**Adjust for:** Buy-and-build: mandatory for every quarterly run. Lower-mid-market: where an add-on's accounts before purchase are unaudited, label its acquired EBITDA as stated.

---

## Readiness and vendor-side diligence

### EX-03 Exit readiness scorecard

**Use case:** Find out whether the company would stand up to a buyer's diligence today, and what must be fixed.

**Inputs:** Monthly management packs, audit status, top contracts, organisation chart, legal and tax registers, the data-room index if one exists.

**Prompt:**
```
Score [Company]'s readiness for sale in six areas. Rate each Ready, Fixable before launch (with months needed) or Not ready.

1. Financial reporting quality: month-end close within [10] working days, monthly pack, audit status, reconciliations, forecast accuracy against the last [three] budgets.
2. Earnings quality as a buyer's adviser will see it: every adjustment graded Supported / Reasonable / Unsupported, normalised both ways, working capital explainable.
3. Customer and contract hygiene: top [20] contracts signed and producible within [48] hours, change-of-control terms mapped, retention computable from data.
4. Management depth: roles filled, dependence on one person, succession. Facts and gaps only; no assessment of anyone.
5. Data-room readiness: index against a standard request list; documents current and executed.
6. Open legal and tax items: disputes, licences, filings, structure, intellectual property held in the right entity.

For each rating: evidence with locator (or Missing), effect on a buyer's view (price, protection or delay), and a fix with owner and date.
```

**Output:** A six-area scorecard with evidence, buyer effect and dated fixes.

**Moody's:** `getEntityPeers` (screened under rule 12a) then `getEntityFinancials_v2` gives peer margins and growth to set the company's figures in context. `searchCompanyFilings` on the top holding company returns the latest annual report: the auditor's report and its signing date, going-concern statement, restated figures and alternative performance measures are evidence for areas 1 and 2 (rule 12c). The rated issuer's credit opinion shows the figures lenders will test (rules 9b and 11a). Moody's has nothing on contracts, data rooms or management depth beyond directors on record.

**Tested:** Pure Gym group, November 2026 (first run). The annual report gave the auditor, signing date (about four months after year end), going concern, a restated membership figure and four EBITDA bases; areas 3 to 6 rested almost entirely on private material.

**Follow-ups:**
- "Which fixes take longer than the time left before launch?"
- "Re-score in six months and show what moved."

**Adjust for:** Carve-out: add exits from every transition service agreement, systems separation, and 12 months of evidenced standalone cost. Turnaround: add 12 months of stable cash. Lower-mid-market: start with monthly accounts that tie to the bank and a deferred-revenue account where customers pay in advance.

### EX-04 Red-flag scan of our own company

**Use case:** See our company the way a buyer's diligence team will, before they do.

**Inputs:** The same material a buyer would see: accounts, contracts, registers, board minutes.

**Prompt:**
```
Scan [Company] for the red flags a buyer would raise, in these areas: active litigation; ownership of intellectual property; undisclosed material contracts; dependence on one or two people; restated accounts; regulatory breaches; customer concentration; debt covenant problems; adjustments to EBITDA that only ever increase it; related-party transactions.

For each flag: what you found, quoted evidence with locator, severity (CRITICAL / MAJOR / MINOR / INFO), confidence (high, medium, low), how a buyer is likely to use it (price, indemnity, escrow, delay), and the fix or disclosure we should prepare.

Give an overall signal: RED if any CRITICAL flag or any high-confidence MAJOR flag; AMBER if any other MAJOR; GREEN otherwise. "No flags" is acceptable in an area.
```

**Output:** A flag table with buyer impact and fixes, and an overall stoplight.

**Moody's:** Map the group and find the rated issuer first (rule 9b). `searchCompanyFilings` on the top holding company returns its annual report: the adjusted-EBITDA footnotes, prior-period corrections and related-party note are where most red flags sit. The rated issuer's credit opinion gives covenant and leverage triggers (rule 11b). Then `getShellCompanyIndicators` on the company and its holding entities for structure flags a buyer will see; `searchNews` over [365] days with importance set to significance for litigation and regulatory news; `findEntity` on the top [ten] customers and suppliers, then `getEntityRatings` or `getEntityProbabilityOfDefault_v2` for the counterparty credit a buyer will check. For consumer businesses, suppliers only.

**Tested:** Pure Gym group, November 2026. The top holding company returned as unrated; the B2 Corporate Family Rating sat at Pinnacle Bidco plc and was found only through news on the holding company. The annual report gave the add-backs, a £3.8m prior-period correction and a director loan repayable on exit. News at significance returned one item in 12 months, so "no litigation" from news alone is low confidence.

**Follow-ups:**
- "Which flags must be fixed and which disclosed? Draft the disclosure wording for review by counsel."

**Adjust for:** Same method as DD-04 in `03-due-diligence.md`, run from the seller's side. Regulated sectors: add licence status as its own area.

### EX-05 Vendor due diligence scope

**Use case:** Decide which vendor due diligence reports to commission, and from whom, before launch.

**Inputs:** EX-03 scorecard, EX-04 red flags, the likely buyer types from EX-06.

**Prompt:**
```
Propose the vendor due diligence scope for [Company]: reports the seller commissions from advisers so buyers can rely on them.

For each candidate workstream (financial, tax, commercial, legal, technology and cyber, environmental and social, insurance, pensions): whether to commission it, the reason linked to a scorecard gap or red flag, the questions it must answer, the data it needs, the time needed, and whether buyers' lenders will expect reliance on it.

Also list items better handled by a disclosure, a fix before launch, or a warranty and indemnity insurance discussion. Leave adviser selection and fees to the deal partner.
```

**Output:** A scoped list of vendor reports with reasons, questions, data needs and timing.

**Moody's:** Not needed.

**Follow-ups:**
- "Turn the financial workstream scope into a request for proposal."

**Adjust for:** Lower-mid-market: a financial report alone is often enough; say what is lost without the others. Large-cap: add merger-control analysis for counsel. Take-private exits by listing: flag prospectus requirements for counsel.

---

## Buyers and the equity story

### EX-06 Buyer universe mapping

**Use case:** Build a long list of strategic and sponsor buyers from public facts.

**Inputs:** A starting list of names from the deal team and advisers, the company's sector and geographies.

**Prompt:**
```
Map the buyer universe for [Company] from this starting list: [names]. Aim for [30 to 40] researched buyers.

For each buyer: type (strategic or sponsor); matched legal entity, country and identifier; ultimate owner and group structure; relevant subsidiaries; acquisitions in the sector in the last [two] years from public news (two years is the news tool's maximum window; for older deals use the buyer's annual report); overlap with [Company]'s products, customers and geographies.

For sponsor-owned companies in the sector: the sponsor and the date it bought, giving hold period (an ownership information date is not a purchase date). Treat a hold of [4] years or more as a signal the sponsor may sell, labelled as inference.

Mark buyer intent as inference every time. Use public facts only. Flag strategic buyers with likely merger-control exposure for counsel. Ownership facts are not sanctions, integrity or antitrust clearance.
```

**Output:** A buyer table with entity, ownership, group, acquisition history, overlap and sponsor hold periods.

**Moody's:** `findEntity` on all names in one call (print matches and stop on any wrong match), then `getEntityUltimateOwners` (threshold 50.01), `getEntitySubsidiaries`, and `searchNews` with importance set to significance for recent acquisitions. `getEntityRatings` or `getEntityProbabilityOfDefault_v2` gives a labelled view of financing capacity. Ownership chains can stop at an acquisition vehicle; ask for the vehicle's shareholders. `findEntity` criteria search does not build a sector universe, so the starting list must come from people. Sponsors do not resolve reliably by name: `findEntity` may return a portfolio company's financing vehicle or a single fund, so map sponsors through the companies they own (`getEntityUltimateOwners` on those companies). `searchNews` windows are limited to two years (rule 12b). Budget one news call per buyer; for 30 to 40 buyers, split the work across several runs. Research may quote a buyer's purchase multiples; cite each as a fact about that deal, never as a valuation benchmark (rule 11b).

**Tested:** A topic-only news search ("gym fitness club operator acquisition private equity", past year) returned one relevant item in ten. Searching news by entity found a portfolio company's add-on acquisition at once. Search news one buyer at a time, by entity, and add a keyword such as "acquisition". In a second run (Pure Gym, November 2026), a three-year news window was rejected, and two of four sponsors matched the wrong entity ("TDR Capital" to Bellis Finco PLC; "Providence Equity Partners" to a Hong Kong fund vehicle). Ten buyers were researched in a 12-call budget.

**Follow-ups:**
- "Which [three] buyers look most serious, and what evidence supports each?"
- "Which strategic buyers have made a deal of similar size in the last two years?"

**Adjust for:** Minority influence without control: map the exit mechanics in the shareholder agreement first (tag-along, drag-along, put options). Large-cap: add rated-debt context for buyer capacity and flag merger control per strategic buyer. Lower-mid-market: expect a shorter list, weighted to trade buyers and smaller sponsors.

### EX-07 Equity story with verifiable claims

**Use case:** Write the three to five claims a buyer will pay for, each tied to evidence they can check.

**Inputs:** EX-01 and EX-02, the value-creation plan and benefit ledger, cohort and commercial analyses, the forecast.

**Prompt:**
```
Draft the equity story for [Company] in [3 to 5] claims.

For each claim: the claim in one sentence; [3 to 5] years of evidence with locators; how a buyer would verify it (which data, which report); the strongest counter-argument a buyer will raise, and our answer with evidence.

Rules: a claim without evidence is marked Asserted and kept out of the headline. Outperformance claims need a peer comparison with each peer's period and basis. Growth claims separate organic from acquired. Forecast claims show the company's record of hitting its budgets.

End with a claim-evidence table: claim, evidence, status (Evidenced / Partly evidenced / Asserted).
```

**Output:** Draft claims with evidence, buyer challenges and a claim-evidence table.

**Moody's:** `getEntityPeers` (screened under rule 12a) then `getEntityFinancials_v2` tests outperformance claims against peers. `searchAllDocuments` with sector and region filters gives the sector outlook a buyer will also read; it supports context, never a company-specific growth claim. For a rated company, the credit opinion's key-indicators table gives up to five years of revenue, margins and leverage on Moody's-adjusted figures, plus Moody's forecasts, which must be labelled as Moody's view (rules 9b, 11a). If no sector research exists, say [MOODY'S: NOT COVERED] and do not cite a sector outlook.

**Tested:** Pure Gym group, November 2026 (first run). The credit opinion supplied five years of evidence for three claims; the peer comparison moved an outperformance claim to Asserted; no fitness sector research was found; Moody's research and the annual report gave inconsistent member counts, which a seller must reconcile.

**Follow-ups:**
- "Which claim is weakest, and what could we do before launch to strengthen it?"

**Adjust for:** Growth equity and growth buyout: add cohort and retention evidence. Thematic funds: add the theme's indicators to the evidence. Buy-and-build: exclude add-ons that miss the integrated definition from adjusted earnings in the story.

---

## Process preparation

### EX-08 Data-room preparation

**Use case:** Build the sale data room so a buyer finds what they need and nothing surprises them.

**Inputs:** EX-03 scorecard, EX-04 flags, the company's document stores.

**Prompt:**
```
Prepare the data-room plan for the sale of [Company].

1. Index structure by workstream, mapped to a standard buy-side request list.
2. For each item: the document, owner, status (ready / needs update / missing), whether it is executed and current, and the date it will be ready.
3. Sensitive items: customer names, pricing, employee data. Propose staging (which items open in phase one, which only to the final bidders) and redaction, for counsel to confirm.
4. Consistency checks: every figure in the information memorandum and vendor reports must tie to a document in the room.

List gaps that would block launch.
```

**Output:** A dated index plan with owners, staging and redaction proposals, and launch blockers.

**Moody's:** Not needed.

**Follow-ups:**
- "Draft the questions-and-answers log template and the routing of questions to owners."

**Adjust for:** Carve-out: separate documents of the target perimeter from the parent's. Minority influence without control: confirm what the shareholder agreement lets us disclose.

### EX-09 Exit timing and route considerations

**Use case:** Set out the factors behind when and how to sell, for the deal partner's decision.

**Inputs:** EX-01 bridge, EX-03 scorecard, the forecast, the fund's remaining term, the credit agreement, EX-06 buyer map.

**Prompt:**
```
Set out the considerations on timing and route for selling [Company], for the deal partner to decide.

Timing: trailing twelve-month EBITDA now and on the forecast for the next [four] quarters; readiness fixes still open and their dates; the fund's remaining term and any extension; debt maturities and prepayment terms; sector and credit conditions from cited sources.

Routes: sale to a strategic buyer, sale to a sponsor, partial sale or recapitalisation, a continuation vehicle, and listing where relevant. For each route: what it needs, the time it takes, and the main risks. Do not estimate valuation multiples; use the fund's approved mark and cite it.

Present the factors side by side. Do not recommend a date or a route.
```

**Output:** A side-by-side view of timing factors and routes, with no recommendation.

**Moody's:** Run the debt questions on the rated issuer (rule 9b): its credit opinion, found with `searchAllDocuments` scoped to the issuer, gives debt maturities, Moody's-expected leverage and coverage paths and the upgrade and downgrade thresholds (rules 11a and 11b). `searchAllDocuments` with sector and region filters and recent research for sector and credit conditions; if no sector report exists, say so and use rated comparables' rating actions. `getEntityCountryProfile` for each key jurisdiction (if it fails, retry with another entity in that country). For rated buyers or peers, `getEntityRatings` gives financing context. None of it supplies exit multiples; a purchase multiple quoted in research is a fact about that deal, never a valuation benchmark (rule 11b). For call and change-of-control terms, look for a Covenant Quality snapshot on the issuer in `searchAllDocuments`; check that it covers the notes now outstanding, because snapshots exist only for some issues.

**Tested:** Pure Gym group, November 2026. The rated issuer's credit opinion supplied maturities (notes due 2032), the leverage path, triggers and a sponsor's preferred-equity claim at exit. No fitness sector report was found; one country profile was unavailable. Research quoted a 9.4x add-on purchase multiple, which must not be read as an exit multiple.

**Follow-ups:**
- "What would have to change for a sale [12] months later to be better for the fund?"

**Adjust for:** Take-private: a return to public markets is one route; flag it for counsel. Minority influence without control: start from the shareholder agreement's exit rights. Turnaround: test 12 months of stable cash before any route is listed as ready.

### EX-10 Questions buyers will ask, with evidence-backed answers

**Use case:** Prepare management for buyer diligence with answers that rest on documents.

**Inputs:** EX-03 to EX-08 outputs, the forecast, the equity story.

**Prompt:**
```
List the questions buyers are most likely to ask about [Company], by workstream: financial, commercial, management, legal, technology, tax, environmental and social.

For each question: why a buyer will ask it (link to a red flag, scorecard gap or equity-story claim); a draft answer; the evidence behind the answer with locators; and who in management answers it. Where the evidence is weak or missing, say so and give the fix or the honest answer.

Put first the questions that could move price or protection.
```

**Output:** A prioritised question bank with evidence-backed draft answers and named respondents.

**Moody's:** Start with the company's rated issuer (rule 9b): `getEntityRatings` for the assigned rating and `getCreditOpinion`, or `searchAllDocuments` scoped to the issuer, for its latest credit opinion. Buyers and their lenders will ask about the rating, its upgrade and downgrade triggers, Moody's-adjusted leverage and the debt terms, and draft answers should match Moody's text (rules 11a and 11b). Quote the assigned rating first and label any PD implied rating as a model output (rule 11). Then `findEntity` and `getEntityRatings` or `getEntityProbabilityOfDefault_v2` on top customers and suppliers for counterparty questions; for consumer businesses, suppliers only.

**Tested:** Pure Gym group, November 2026. The rated issuer's credit opinion answered the financing questions a buyer asks first. The PD implied rating (Ba3) sat three notches above the assigned B2 rating, and the model flagged its input as outside its sample.

**Follow-ups:**
- "Turn the top 20 into a two-hour management rehearsal."

**Adjust for:** Growth equity: add questions on retention, acquisition cost payback and hiring capacity. Carve-out: add standalone cost and separation questions.
