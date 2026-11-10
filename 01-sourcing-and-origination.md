# Sourcing and origination prompts

Use these before any company has sent material: to set what the fund is looking for, map a market, find the owners who may transact, and decide whom to call first. Each prompt stands alone: paste the standing rules from `00-standing-rules.md` first, attach or connect what is named under **Inputs**, fill the bracketed fields and run it. Outputs are desk research from public and licensed sources. Once a teaser or information memorandum arrives, move to `02-screening.md`.

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
| Thesis and buy box | SO-01 to SO-02 |
| Market mapping | SO-03 to SO-05 |
| Target identification and signals | SO-06 to SO-09 |
| Strategy-specific screens | SO-10 to SO-13 |
| Target brief | SO-14 |
| Scoring and prioritisation | SO-15 to SO-16 |
| Relationship and outreach | SO-17 to SO-20 |

---

## Thesis and buy box

### SO-01 Thesis and buy box

**Use case:** Turn a loose idea into a written thesis and a buy box (the measurable criteria a company must meet) that the team can screen against.

**Inputs:** The fund mandate or fund profile; the partner's notes on the idea; any sector reading the team already holds.

**Prompt:**
```
We are a [strategy: buyout platform / buy-and-build / carve-out / take-private / turnaround / growth buyout / growth equity / minority] fund in the [lower mid-market / mid-market / large-cap] band. Our idea is: [two or three sentences].

1. Restate the thesis in one paragraph: what is changing in the market, why companies of this kind benefit, and why an owner like us adds value.
2. Write three to five conditions a company must meet to fit. Make each one measurable and say what evidence would show it (for example "recurring or contracted revenue above [60%] of the total, shown in filed accounts or a company statement").
3. Write the buy box as a table: sector or value-chain stage, geography, revenue range [x to y], EBITDA range [x to y], ownership type sought, control sought, hard rules (for example no customer above [25%] of revenue), and exclusions.
4. Split the criteria into knock-outs (pass or fail, no points) and scored criteria.
5. List what would make us drop the thesis: the facts that, if found, mean the idea does not work.

Flag any condition that cannot be tested from public information before first contact.
```

**Output:** A thesis paragraph, measurable conditions, a buy-box table split into knock-outs and scored criteria, and kill conditions.

**Moody's:** `searchAllDocuments` with the sector and region filters and recent research first gives Moody's sector context to cite in the thesis paragraph. Discard hits from unrelated sectors, and check the results really are in the sector: unresolved sector names are dropped from the filter without warning. Give the publication date of every research item cited and label anything older than [two] years as older research. Issuer comments on recent deals in the sector can supply dated deal facts (see standing rule 11b). `getEntityCountryProfile` adds country context for each target geography; it needs an entity ID, so resolve one well-known company in each target country with `findEntity` first. Do not call `getEntitySectorOutlook` raw: in a test it returned far more text than a prompt can use.

**Tested:** For a UK and Western European gym thesis, two filtered searches returned 22 documents, of which 4 were relevant (others covered pubs, cruise, sports data and consumer-loan securitisations). The only sector-specific piece was from September 2022 and still ranked first with recent research favoured. A one-month-old issuer comment gave a dated add-on purchase multiple. `getEntityCountryProfile` on a UK company returned the UK sovereign credit opinion.

**Follow-ups:**
- "Which condition is doing the most work? What happens to the size of the target list if we relax it?"
- "Write the buy box as a one-page note we can send to intermediaries."

**Adjust for:** Buy-and-build: add a condition on fragmentation (many owners, none dominant) and a separate add-on size range relative to the platform. Carve-out: apply the size test to the division, and add a condition on parent pressure. Take-private: add free float and valuation conditions, with the data sourced from the fund's market-data provider. Turnaround: replace growth conditions with a fixable cause of decline and a liquidity runway. Growth equity and minority: add conditions on governance rights and an exit route.

### SO-02 Theme exposure test

**Use case:** For a thematic fund, test how much of a company's business the theme actually drives.

**Inputs:** A short list of companies; the fund's theme statement; filings, product pages and company statements for each.

**Prompt:**
```
Our theme is [theme, in one sentence]. For each company on the list below, answer two questions.

1. What share of revenue is driven by the theme? Use segment disclosure, product lists or company statements, each cited. If the share is not evidenced, write [GAP: theme revenue share not disclosed]. Do not estimate it.
2. What happens to this business if the trend arrives five years late? Answer in three or four lines: does another part of the business carry it, and how much does it depend on policy support or subsidy?

Classify each company: Pure play (above [75%] of revenue from the theme), Majority ([50%] to [75%]), Minority ([20%] to [50%]), Incidental (below [20%]), or Unknown (not evidenced). Score Unknown conservatively and turn it into a question for first contact.

Companies: [list with country].
```

**Output:** A table of evidenced theme share, class, a five-years-late view and open questions.

**Moody's:** `findEntity` once for all names, then `getEntitySegmentFinancials` gives disclosed segment splits where a company reports them; use them for shares only and check the currency against the annual report (see standing rule 12a and the SO-11 test note). `getEntityEsg` adds emissions and ESG scores for rated companies, which can support a decarbonisation theme (credit-impact scores exist only for rated companies; emissions and temperature alignment are available more widely; none of these is evidence of theme revenue, and check whether Scope 3 is marked estimated before comparing years). Coverage of small private companies will be thin; mark [MOODY'S: NOT COVERED] and carry on.

**Follow-ups:**
- "Which companies depend on a single subsidy or regulation? Name it and its expiry date where public."
- "Rank the Majority and Pure play names by the strength of evidence behind their share."

**Adjust for:** Specialist funds without a theme: replace the theme with the house definition of the niche and test what share of revenue sits inside it. Growth equity: add a line on whether theme revenue is growing faster than the rest.

---

## Market mapping

### SO-03 Value-chain map

**Use case:** Map the activities from raw input to end customer, and who earns the margin at each step, before listing any company.

**Inputs:** The thesis from SO-01; any sector studies, trade-body material or research the team holds.

**Prompt:**
```
Map the value chain for [sector or niche] in [geography].

For each stage, give: what is done; who the customers of that stage are; how margin is earned and the evidence for where margin sits; how fragmented the stage is (many small owners or a few large ones), with evidence; the typical owner type (founder, family, sponsor, corporate, listed); and where companies at this stage can be found (registries, licensing bodies, trade associations, exhibitor lists, rankings).

Then mark each stage In scope or Out of scope for our thesis, with the reason. Mark any stage where the evidence on margin or fragmentation is thin, so we do not choose it on assumption.

Present the map as a table, one row per stage, in order from input to end customer.
```

**Output:** A stage-by-stage value-chain table with scope decisions and evidence gaps.

**Moody's:** `searchAllDocuments` with sector and region filters and recent research for credit research that describes the sector's structure and pressures. `getEntityPeers` on one well-known company at each stage, then `getEntityFinancials_v2` on the peers, gives filed margins to support where margin sits; clean the peer list first (see standing rule 12a). Give the date of every research item cited. Moody's does not size markets.

**Follow-ups:**
- "Which in-scope stage has the most owners likely to transact in the next [three] years, and why?"
- "Where would a new technology or AI tool remove a stage altogether?"

**Adjust for:** Thematic funds: start from the theme's value chain and include enablers and suppliers outside the obvious sector codes. Buy-and-build: rate each stage on fragmentation first. Carve-out: add a column for which listed groups have a division at each stage. Generalist funds: run it one sector at a time.

### SO-04 Universe build and funnel

**Use case:** Build the full list of companies in scope and record every name that drops out, so a partner can audit the list.

**Inputs:** The buy box from SO-01 and the in-scope stages from SO-03; the fund's licensed bulk company dataset if held; directories, registries and lists.

**Prompt:**
```
Build the company universe for [thesis] in [geography].

Start by telling me whether we hold a licensed bulk company dataset (a complete extract of companies by industry, geography and size). If we do, name it and its extraction date: [dataset name, date / none]. If we do not, label every output "Indicative map: built from named sources only".

Work the sources in this order: the bulk dataset; trade association directories, registries, licensing bodies and exhibitor lists; rankings and press; disciplined web search. One row per record with its source. Build the whole list before filtering anything.

Then run the funnel and log counts at each stage: records gathered; after de-duplication; after knock-out filters [list the knock-outs]; after scoring against the thesis conditions; after feasibility (could we realistically transact). At every stage, count in must equal count out plus removed. Keep an exclusion log with one row per removed name: name, stage, the rule that fired and its evidence. "Not a fit" is never a reason. A name with an unknown field stays in, with the gap logged.

Show the funnel table first, then the surviving list, then the exclusion log.
```

**Output:** A completeness label, a reconciled funnel table, the surviving list and an exclusion log.

**Moody's:** Use the connector to resolve and enrich names you already have. Do not rely on a criteria search through `findEntity` even as a seed (see standing rule 13): in one test "logistics companies in Texas" returned airlines, railroads and trusts, mostly outside the state; in another, four phrasings for UK gym and fitness operators returned nothing or a tool error. `getEntityPeers` on two or three well-known companies is a better seed (prefer unlisted or listed operators: peers of a rated borrower tend to be other rated borrowers from a broad sector); clean each peer list before adding it (see standing rule 12a) and log peers as a named source. A complete universe needs a licensed bulk dataset, from Moody's under a separate entitlement or from another provider. After the list exists, `findEntity` and `getEntityFinancials_v2` add filed size and margin for covered names, and `getEntityUltimateOwners` can fire a control knock-out (a candidate wholly owned by a competitor).

**Tested:** For UK and Western European gyms with EBITDA of £10m to £60m, the criteria search contributed 0 records, peers of one operator contributed 8 (2 survived), and the team's own list 15 (9 survived). The connector removed 4 duplicates and 8 knock-outs with cited evidence, including one candidate wholly owned by a listed competitor. The output was correctly labelled "Indicative map".

**Follow-ups:**
- "Re-check a sample of [20] excluded names, or [10%] if larger, for wrongful exclusion."
- "Which source produced the most surviving names, and which produced none?"

**Adjust for:** Lower mid-market: registries, trade bodies and licensing lists matter most, and many names will have no public financials. Large-cap: the universe is small and well disclosed, so spend the effort on depth per name. Carve-out: keep corporate-owned names, tag them and send them to SO-11. Take-private: build listed names only from the fund's market-data source and send them to SO-12.

### SO-05 De-duplication and entity resolution

**Use case:** Make sure each business appears once, under the legal entity the fund would actually buy.

**Inputs:** A list of company names from several sources, with country and any registry numbers or websites.

**Prompt:**
```
Here is a list of [number] records for [thesis]. Resolve each to one legal entity.

1. Group records that refer to the same business: trading names, former names, spelling variants, subsidiaries and branches of one group.
2. For each group, choose the entity the fund would buy (usually the top operating or holding company below any sponsor or corporate parent) and write down the rule you used.
3. Record for each resolved entity: legal name, country, registry or other identifier, match basis (registry number, exact legal name, website, address), and the records merged into it.
4. Put records you could not resolve in an Unresolved list. They stay in the counts.

Return a merge log (each record and the entity it went to), the resolved list, and the Unresolved list. Report counts before and after.
```

**Output:** A merge log, a resolved list with identifiers and match basis, and an Unresolved list.

**Moody's:** `findEntity` with all names in one call and `getAlternatives` set to true (`maxAlternatives` 3 to 5) returns identifiers and the other group entities behind each name (see standing rules 9 and 9a). Build the merge log from the name-accounting table: the connector removes duplicates without saying so and does not say which record produced which entity, so a record with no entity of its own goes in the log as "absorbed, mapping inferred" for checking. Trading names often resolve to a rated Bidco or Midco rather than the operating company; treat that as a group match, and choose the entity to buy only after `getEntitySubsidiaries` or `getEntityUltimateOwners` (threshold [50.01]) confirms the tree. Search by registered name or registry number where you have it.

**Tested:** Eight records for UK gym operators ("PureGym; Pure Gym Limited; Pinnacle Topco; David Lloyd Leisure; Virgin Active; The Gym Group; Gym Group plc; Third Space") came back as six entities with no mapping. "Pinnacle Topco" disappeared; "PureGym" returned the rated Pinnacle Bidco plc, with the Swiss PureGym AG as its alternative; "David Lloyd Leisure" returned the rated Deuce Midco Limited, with four David Lloyd entities (one a property company) as alternatives. With alternatives switched on, every record could be placed in the right group.

**Follow-ups:**
- "List every match where the country differs from the source record."
- "For the Unresolved list, what extra detail would resolve each one?"

**Adjust for:** Buy-and-build: keep each site or brand of a multi-site group visible as a sub-row, because add-on logic often depends on locations. Large lists: for several thousand records, use a record-linkage tool and keep this prompt for checking the hard cases.

---

## Target identification and signals

### SO-06 Ownership and succession signals for a list

**Use case:** Read the public ownership and director record across a list for dated facts that suggest an owner may transact.

**Inputs:** A resolved list of companies (from SO-05) with identifiers.

**Prompt:**
```
For each company on this list, record dated ownership and succession facts:

1. Owner type: founder, family, partners, financial sponsor (a private equity owner), corporate, listed, or unknown. Name the ultimate owner and the chain from the company to it.
2. Length of current ownership, with the source of the start date. An ownership record's information date is not a start date.
3. Director appointments and resignations in the last [36] months, with dates. If only current directors are available, say resignations were not checked.
4. A holding company inserted above the operating company recently, with the date.
5. Any public statement by an owner about succession, partners, growth capital or retirement, quoted and dated.

Use filed and published professional facts only. Do not infer or report age, date of birth, health or family circumstances, even where a source supplies them. A signal dated within [12] months counts as current; older than [3] years is context only.

Return one table, one row per company, with a column for each signal, its date and its source, and a short "why this matters" line where a signal is present. Mark companies with no signal as such.
```

**Output:** A signal table across the list, with dates and sources.

**Moody's:** `getEntityUltimateOwners` (threshold [25.01]) for the chain, `getEntityShareholders` for direct holders and their percentages, and `getEntityManagersDirectors_v2` with `includeDetails` set to true for appointment dates. Run them across the whole list; this is the reliable use of the connector in sourcing. The chain can stop at an acquisition vehicle: a test on a company taken private stopped at its buyout vehicle. Climb the chain with `getEntityShareholders` on each vehicle. It does not return owner entity IDs, so resolve each vehicle with `findEntity` by its BvD ID where `getEntityUltimateOwners` gives one, otherwise by its exact name with `getAlternatives`, and check that `findEntity` returned that vehicle and not an operating or sister company; stop after two layers with no named owner and record the chain as ending at a vehicle. Treat owner types and information dates as leads (see standing rule 12a); classify the owner as "unknown" until a filing or announcement confirms it. The directors tool lists current officers only, so resignations need the registry's officer history; lists can be thin (one test returned only the chief executive), so treat a short list as a gap. With `includeDetails` the tool also returns dates of birth, ages and sometimes biographies; do not use them.

**Tested:** On seven UK leisure and fitness operators, the tools gave sponsor chains for two, a named private individual (through a holding company) for one, and dated board appointments for all five private companies. Brait PLC was typed "Bank" and a Cayman vehicle "Corporate"; listed companies returned themselves as owner with a 2013 date. A name search for the Cayman vehicle returned the operating company instead; in a re-run, "Deuce Holdco Limited" returned a Belgian operating company, and climbing the Third Space chain found a third vehicle layer with no named sponsor. One director biography combined at least three different people's careers.

**Follow-ups:**
- "Which companies show two or more current signals? Rank them."
- "List every company where the ownership chain stopped at a vehicle with no named owner above it."

**Adjust for:** Buyout platform and lower mid-market: founder and family signals carry most weight. Minority: add who controls the company and whether a controlling holder would admit a minority investor. Growth equity: add funding rounds and investors from the fund's licensed data, since Moody's coverage of young companies is often thin.

### SO-07 Sponsor-owned companies and estimated hold period

**Use case:** Find companies already owned by another sponsor and estimate when they may come to market.

**Inputs:** A resolved list of companies; the fund's view of typical hold periods in the sector.

**Prompt:**
```
From this list, identify every company owned wholly or in part by a financial sponsor.

For each: the sponsor and the fund or vehicle that holds the stake; percentage held; the date the sponsor invested, with its source (filing, press release or dated announcement); years held as of [today's date]; any add-on acquisitions since entry, dated; and any public sign of a sale process (adviser appointed, press report), quoted and dated.

The date attached to an ownership record is the date the information was captured, which may differ from the investment date. Find the investment date separately and say when you could not. Where no investment date is found, write years held as [GAP] and do not apply the hold-period flag.

Flag companies held longer than [4] years as possible near-term processes. Treat these as intelligence: they will usually come to market through an advised auction, so the action is to build a relationship with the sponsor and its likely advisers. Do not plan a direct approach to the company.
```

**Output:** A sponsor-ownership table with entry dates, years held, add-on history and process signals.

**Moody's:** `getEntityUltimateOwners` shows the sponsor fund where the chain reaches it: for a UK gym operator, the chain ran through a topco to the sponsor's fund. Where it stops at a bidco, call `getEntityShareholders` on the bidco. Owner types need checking (see standing rule 12a). Moody's ownership records do not give investment dates, and `searchNews` rejects windows longer than two years (standing rule 12b), so entry announcements older than that need the fund's deal database or licensed transaction data; a vehicle's incorporation year from `getEntityProfile` is an indication only. For rated sponsor-backed borrowers, find the rated issuer (standing rule 9b) and run `searchAllDocuments` scoped to it: issuer comments name all sponsors, including minority co-investors the ownership tools miss, and give maturities and deal facts (standing rule 11b). `searchNews` on each company shows add-ons and process news. `getEntityRatings` and `getCreditOpinion` add leverage and refinancing context.

**Tested:** On four sponsor-type owned UK fitness groups, sponsors were named for two, add-ons in the last month were found for two, and no investment date could be found for any; a 2018 to 2026 news window returned an error. A Moody's issuer comment named a minority co-investor in PureGym that the ownership tools did not show.

**Follow-ups:**
- "Which of these sponsors have other companies in our sectors? List them as a relationship map."
- "For each company held more than [5] years, when does its main debt facility mature, where public?"

**Adjust for:** Buy-and-build: a sponsor-owned competitor that is buying add-ons is also a rival bidder for your add-ons; note it. Large-cap: add secondary buyouts and continuation vehicles as possible exits. Minority: sponsor-owned companies may sell a minority stake to return cash; add that as a scenario.

### SO-08 Hiring and expansion signals from public sources

**Use case:** Spot companies professionalising or expanding, which often comes before a transaction.

**Inputs:** A list of companies with websites; access to their careers pages, public job postings and press releases.

**Prompt:**
```
For each company on this list, look for hiring and expansion signals in public sources over the last [12] months:

Hiring: postings for a chief financial officer, chief operating officer, head of strategy, head of mergers and acquisitions, head of integration or finance director; sudden growth in the number of open roles; country manager roles; postings that mention professionalising, scalable systems or a next phase of growth.
Expansion: new sites, new countries, new subsidiaries, new product lines, major contract wins, a refreshed website with new leadership or investor pages.

For each signal give the source, the date, and a quote or description. Several articles repeating one press release count as one source. If a careers page or posting cannot be loaded, say so; do not infer what it showed.

Then rate each company's signal as strong, moderate, weak or none, with one line of reasoning.
```

**Output:** A per-company signal table with sources and a strength rating.

**Moody's:** Not a hiring source. `getEntitySubsidiaries` and `getEntityBranches` can confirm new entities or locations (subsidiary lists may be partial, see standing rule 12a; branch records are registry entries, not site lists, and rarely show new clubs or shops), and `getEntityFinancials_v2` gives filed employee numbers over several years where reported.

**Follow-ups:**
- "Which companies hired a finance leader in the last [six] months? Who did they hire and from where?"
- "Turn the strong signals into one-line reasons to call."

**Adjust for:** Growth buyout and growth equity: hiring velocity is a main signal; add sales and engineering headcount trends. Carve-out: look for divisional finance or separation roles at the parent. Turnaround: look for interim managers, restructuring officers and departures in finance.

### SO-09 News signals

**Use case:** Catch events across a watchlist that change the case for calling a company.

**Inputs:** A resolved watchlist; the period to cover.

**Prompt:**
```
Search news on each company on this watchlist for the last [90] days. Keep only items that matter for sourcing:

- Ownership: sale process, adviser appointed, new investor, founder stepping back, management change.
- Corporate activity: acquisitions made, disposals, new subsidiaries, refinancing.
- Commercial: large contract won or lost, new geography, new product.
- Stress: profit warning, covenant issue, litigation, regulatory action, rating action.
- Competitor and sector: a direct competitor bought by a sponsor, a new consolidator, regulation favouring scale.

For each item: company, date, category, a one-line summary, source, and what it means for us (call now, watch, or no action). Several outlets repeating one story count as one item. Drop anything older than the window.
```

**Output:** A dated event table with a suggested action per item.

**Moody's:** `searchNews` for each entity (one entity per call) with `timeRange` set to the window and `importance` set to significance; follow standing rule 12b on noise. If nothing meets the relevance minimum the tool retries without it, so small or generically named companies can return only unrelated articles: keep an item only if it names the company or its brand, drop share-price bulletins, and write [MOODY'S: NOT COVERED] when nothing remains. `searchType` keyword keeps every result at the relevance minimum. `getEntityRatings` history and `getEntityProbabilityOfDefault_v2` with `early_warning` add credit events (standing rule 11 applies). A watchlist of more than about ten names needs batching across sessions.

**Tested:** On seven UK leisure and fitness companies over 90 days, it surfaced two add-on acquisitions by sponsor-owned consolidators, a dividend story and a rating upgrade. About 30 of 39 items were noise or repeats; for the two smallest companies every item was unrelated (an academic paper, a Spanish municipal tender).

**Follow-ups:**
- "Which items should change a company's score in SO-15? Show the old and new score."
- "Group the items by sector and say which sector is moving fastest."

**Adjust for:** Buy-and-build: flag every acquisition made by a competitor of your platform. Carve-out: add parent portfolio reviews and activist campaigns. Take-private: follow the rules in SO-12 and use public news only. Turnaround: put the stress category first.

---

## Strategy-specific screens

### SO-10 Add-on pipeline around a platform

**Use case:** Build and rank the add-on candidates around one platform, keeping "would it help" apart from "can we buy it".

**Inputs:** The platform's description, size, sites and main customers; the purpose of add-ons; a candidate list from SO-04; the fund's portfolio list for the conflict check.

**Prompt:**
```
Our platform is [Platform], which [what it does, where, for whom]. Add-ons are for [new geography / new service line / density in existing areas / capability / customers]. Size range for an add-on: [x to y revenue].

For each candidate:
1. Conflict check against our portfolio [list]: owned by us or a portfolio company; competes with a portfolio company; material customer or supplier of one; wanted by more than one of our platforms. Status: Clear, Potential conflict or Conflict. Do not clear anyone; compliance decides.
2. Fit score out of 100: overlap with the add-on purpose [25]; geographic logic [15]; size relative to the platform [10]; revenue quality [15]; customer effect, including concentration after combining [15]; ease of integration from public evidence [20].
3. Actionability score out of 100: owner type [30]; dated succession or hold-period signal [30]; other transaction signals [25]; relationship and access [15].
4. Action: Engage (fit [75]+ and actionability [55]+); Cultivate (fit [75]+, actionability below [55]); Watchlist (fit [55] to [74], any actionability); Pass (fit below [55], naming the weak dimension).

Score any dimension with no evidence at [40%] of its weight and mark it None. State the currency of each size figure; do not convert currencies without a dated rate from our market-data source, and leave the size test open rather than failed when a rate is missing.

Disqualify, with the rule logged, any candidate whose addition would break our hard rule on customer concentration ([25%]), falls outside the size range, or is not available (for example wholly owned by a competitor). Keep sponsor-owned candidates as intelligence only. Never average the two scores.
```

**Output:** A candidate table with conflict status, two separate scores, an action per name and a disqualification log.

**Moody's:** `findEntity` on the platform and all candidates in one call; `getEntityUltimateOwners` and `getEntityShareholders` across the list for owner type and sponsor ownership; `getEntityFinancials_v2` for filed size and margins where covered (expect gaps for very small companies; apply standing rule 9c, for example EBITDA with no revenue marks a franchisor or holding entity). Brand names often resolve to a single-site subsidiary: if the matched name differs from the brand or the employee count is very small, climb to the group holding with `getEntityUltimateOwners` and re-resolve it with `findEntity` using the holding's BvD ID from that result (a name search can return the same single site again). `getEntitySubsidiaries` shows the platform's past add-ons only when run on its operating company; on a topco it may list one direct subsidiary (standing rule 12a). Moody's ownership data supports the conflict check; it is not sanctions or adverse-media screening.

**Tested:** Around PureGym with seven UK and European gym candidates, ownership checks found one candidate wholly owned by a listed competitor, one sponsor-owned and one founder-owned. "TruGym Limited" returned a 22-employee single-site company and "SportCity Holding B.V." a single club; two names were not found. One candidate scored fit 60 and actionability 12, which matched no band in the earlier wording.

**Follow-ups:**
- "Which Engage names does platform management already know? Record who holds each relationship."
- "Show the platform's revenue and customer mix after the top [three] add-ons, using filed figures only."

**Adjust for:** Buy-and-build is the home case. Specialist funds: score integration against the house playbook from past add-ons. Lower mid-market: many candidates will have abbreviated accounts or none; score size from evidenced proxies (sites, staff) and label them. Large-cap: add-ons may need merger-control review; add a column for combined share where public.

### SO-11 Carve-out candidates

**Use case:** Find divisions of listed or large groups that the public record suggests are non-core.

**Inputs:** A list of parent groups or a sector; their annual reports and earnings-call transcripts if held.

**Prompt:**
```
Screen these parents for carve-out candidates in [sector] within [geography]: [list]. The division must fit [revenue range].

For each parent:
1. Segment financials for [three to five] years: each segment's revenue, profit, share of group, margin and growth against the group. Check that segments sum to group revenue after stated eliminations. Where the segment set changes between years, say so and do not compute growth across the change.
2. Dated, quoted statements from filings and calls about strategy, core and non-core activities, portfolio reviews, disposals, held-for-sale assets or discontinued operations. Check first whether a disposal of the segment has already been announced or completed; if so, record it as intelligence and do not score it.
3. Pressure on the parent: assigned ratings, outlook and history; any deleveraging target, quoted; activist involvement from public sources.
4. Whether the segment lines up with identifiable legal entities in the corporate tree.

Grade the non-core evidence for each segment: Stated (the parent has said so, quoted and dated), Indicated (two or more independent indications), or Inferred (financial pattern only). An Inferred grade caps the band at Watchlist. A weak segment is not by that fact non-core.

Score each candidate perimeter out of 100: mandate fit [15], size [10], non-core evidence [25], pressure on the parent [20], perimeter clarity [15], standalone attractiveness [15]. List open questions on separability (central cost allocation, shared contracts, systems, people, transition services) for every name above Pass. Never say a division is for sale unless the parent has said so.
```

**Output:** A segment table per parent, quoted statements, pressure evidence, graded and scored perimeters, and separability questions.

**Moody's:** `getEntitySegmentFinancials` for segment revenue and profit; segment profit includes allocated central costs and is not a standalone figure. `searchCompanyFilings` on annual reports and `earnings_call_transcript` for strategic statements (see standing rule 12c; the held-for-sale note in the annual report is the quickest source of a Stated grade). `getEntityRatings` and `getCreditOpinion` with `RatingOutlook` and `FactorsLeadingToDowngrade` show leverage triggers that could push a disposal. `getEntitySubsidiaries` maps the tree.

**Tested:** On three UK-listed industrial groups (Smiths Group, IMI, Morgan Advanced Materials), `getEntitySegmentFinancials` returned two to five years by business and geography, and the business segments summed exactly to group revenue in every year checked. A segment present in one year and absent in the next marked a disposal. Cautions from the same tests:
- The header said GBP while the values sat in the USD column and were US dollar conversions of the sterling figures, about 1.23 times the reported sterling amounts, and 1.32 for another group's 2025 figures, so the rate changes by year (one group's older segment names carried the sterling figure inside the name). Use the tool's figures for shares and margins only, and take sterling amounts from the filing (standing rule 12a).
- Segment names can be garbled or carry numbers ("Ate and orpora non-headline", "UK 180.1"); correct them from the filing.
- Segment sets changed between years at two of the three groups, so multi-year segment growth was often impossible.
- The obvious candidates at two groups had already been announced for sale or sold, found in the annual reports' held-for-sale notes and a credit opinion.

**Follow-ups:**
- "For the top three perimeters, which entities in the tree appear to belong to the division, and which are shared?"
- "What would the parent's leverage be after selling each division at [a stated multiple from our market-data source]?"

**Adjust for:** Carve-out is the home case. Large-cap: add merger-control and works-council questions to the separability list. Specialist funds: compare each division with the fund's prior carve-outs. Turnaround: a division of a stressed parent may itself need a fix; add a line on its own trend.

### SO-12 Take-private screen from public filings

**Use case:** Rank listed companies that may merit a partner's and counsel's attention, using public information only.

**Inputs:** A list of listed companies in scope from the fund's licensed market-data source; confirmation from compliance that none is on the restricted list.

**Prompt:**
```
Run a research screen on these listed companies in [sector, markets]: [list]. Compliance has confirmed none is on our restricted list as of [date].

Strict rules: use public information only (filings, published accounts, regulatory announcements, published major-shareholder disclosures, our licensed market data, Moody's data). If any source appears to contain non-public information, stop work on that name, do not record it, and tell me. Do not suggest, model or estimate stake-building. Draft no contact with the company, its board, shareholders or advisers.

For each company:
1. Valuation against peers and its own [five]-year history, using share prices, market capitalisation and multiples I supply from [market-data source and date]. Do not source these anywhere else. Record the undisturbed price and its reference date.
2. Public reasons for any discount: profit warnings, guidance changes, governance disputes, litigation, sector events, quoted and dated.
3. Register: holders above the disclosure threshold, founder or family and management holdings, free float, each dated where the source gives a date. Say the register is partial, and do not add a parent's and its fund's holdings together without checking they are separate.
4. Fundamentals and peers: margins, growth, leverage, cash conversion from filed accounts.
5. Indicative leverage capacity against our posture of [maximum leverage, minimum interest cover], labelled "indicative context, not a financing quote".
6. Rated debt: ratings and outlook; note public bonds or loans counsel should review for change-of-control terms.

Score out of 100: mandate fit [15], business quality [20], valuation gap net of public explanations [20], register structure [15], leverage capacity [15], execution complexity scored inversely [15].

End every output with: "Internal research screen from public information. Not investment advice. Not a recommendation to deal in any security. Any approach or dealing requires prior advice from legal counsel."
```

**Output:** A scored screen with valuation context, public explanations, register, leverage range and rated-debt notes, plus the required footer.

**Moody's:** `searchCompanyFilings` with document types annual, interim and `earnings_call_transcript` for fundamentals and public explanations. Start it first, run the other steps, and retry at the end (standing rule 12c); if it is still ingesting, leave steps 2 and 6 open and say so. `getEntityShareholders` gives the register without dates. `getEntityFinancials_v2` and `getEntityPeers` for peer fundamentals (clean peers per standing rule 12a); check interest expense against debt, since a sharp fall can be a classification change. `getEntityRatings` and `getCreditOpinion` for rated debt. Moody's does not supply share prices, market capitalisation or equity multiples; those come only from the fund's market-data source, and share prices quoted in news bulletins are not a substitute. If no market data is supplied, leave the valuation dimension unscored and say so at the top. Search with the company's public name only, never alongside the fund's name.

**Tested:** On The Gym Group and Hollywood Bowl Group, the register, fundamentals and model PDs came back cleanly, and both were confirmed unrated. Annual filings for both stayed "ingesting" across three attempts several minutes apart. Shareholder records carried no dates and listed some parents and funds separately. One company's interest expense fell from £12.04m to £0.46m year on year with debt rising, which needs checking against the annual report.

**Follow-ups:**
- "For the top three names, list the questions counsel must answer before any further work."
- "Which valuation gaps disappear once the public explanations are taken into account?"

**Adjust for:** Take-private is the home case and the rules above never relax. Large-cap: add financing certainty and antitrust as gating items. Mid-market: small listed companies often have concentrated registers; give register structure more weight. Minority: a listed minority stake raises its own disclosure rules; refer to counsel before any work.

### SO-13 Turnaround and credit-stress watchlist

**Use case:** Find companies whose credit signals suggest underperformance an operational owner could fix.

**Inputs:** A resolved list of companies in the fund's sectors.

**Prompt:**
```
For each company on this list, collect credit-stress evidence over the last [24] months:

1. Assigned ratings and outlook, with every change and its date.
2. Model-based probability of default and the implied rating, with the trend over the window and the model's confidence description. Label these as model outputs. Show any gap of more than two notches between a model output and the assigned rating in a separate table.
3. The rating agency's stated downgrade triggers (for example a leverage level), quoted, and where the company stands against them, using the credit opinion's own adjusted metrics.
4. Public events: profit warnings, covenant waivers, refinancing, auditor changes, senior departures, quoted and dated.
5. Which entity carries the debt. If the measures above relate to an operating company while the debt sits at a holding company, say so.

Then sort the list into: stressed with a stated cause we could address (operations, cost base, pricing, management, capital structure), stressed for reasons outside an owner's control (sector decline, regulation), not stressed, and not assessable (wrong or vehicle entity). Sort on assigned ratings where they exist, not on model outputs. For the first group, give the evidence for the cause in three lines.
```

**Output:** A credit-stress table with ratings, model outputs, triggers and events, sorted by type of stress.

**Moody's:** `getEntityRatings` for assigned ratings and history; `getEntityProbabilityOfDefault_v2` with `early_warning` and `risk_drivers` for model-based measures; `getCreditOpinion` with `FactorsLeadingToDowngrade` for triggers (a test opinion gave explicit leverage and cash-flow thresholds) and `KeyIndicatorsTable` for where the company stands (standing rule 11a). Standing rules 9b and 11 govern which entity and which measure to use: PD is calculated on the entity's own accounts, so a test operating company showed a low PD while buyout debt sat at a holding company above it. If the rating is WR (withdrawn) or the entity is a financing vehicle, find the current rated borrower before using any measure; `searchAllDocuments` on the company name can surface it from rating-action text. `searchNews` with significance over 365 days adds dated events such as sale approaches.

**Tested:** On eight UK and European consumer and leisure companies, ratings history, quoted triggers and news identified one stressed name with a capital-structure cause (Caa1, EBIT/interest expected below 1.0x, sale approaches reported in September 2026). PD implied ratings were 2 to 9 notches better than assigned ratings for every rated borrower; two PDs came from a "benchmark model with no financials", one of them on the Caa1 company (Ba1). "Merlin Entertainments Limited" returned an entity with ratings withdrawn in 2019; the current issuer appeared only in a rating action found through `searchAllDocuments`. A securitisation financing vehicle returned a PD that looked usable but rested on no financials.

**Follow-ups:**
- "For the first group, what liquidity runway do filed accounts show?"
- "Which of these companies are owned by a sponsor that may prefer to sell over injecting equity?"

**Adjust for:** Turnaround is the home case. Buyout platform and buy-and-build: use the same evidence to avoid targets, and to find distressed add-ons at a sensible price. Carve-out: apply it to parents, where stress drives disposals.

---

## Target brief

### SO-14 One-company cited brief

**Use case:** Give a principal a short, fully cited view of one company before anyone spends time on a call.

**Inputs:** The company's name, registered name if known, country and the reason for looking; any press, website or intermediary note quoting figures.

**Prompt:**
```
Write a screening brief on [Company] (registered name [legal name], country [country]). We are looking because [reason]. The deal would be tagged [strategy].

Work through these steps in order:
M1 Entity pack: resolve the legal entity and print the matched name, country and identifier; stop and show me if it does not match. Map the group: operating company, consolidating holding company and rated issuer. Then profile, ownership chain, direct shareholders, corporate tree, directors with dates, and corporate-structure red flags. If the entity check fails, stop after M1.
M2 Financials: [three to five] years of filed figures, with period end, currency, units and whether consolidated or entity level.
M3 Peers: up to [eight] peers, with the reason each one is comparable, and the company's position on margin, growth and leverage. Drop peers from the company's own group and property, financing or non-operating entities, and say how many remain.
M4 Counterparties: for any customers or suppliers named in public sources, their ownership and credit standing.
M5 Sector frame: recent sector research relevant to this company only.
M6 Credit: ratings or model-based measures, labelled as which.

Then build the three-column table, one row per claim that matters (revenue, EBITDA, employees, ownership, sites, group structure): "Public or management claim" with locator and date, "Moody's shows" with period and citation, and "Fund accepts" or "open" with the question that would close it. A difference above [5%] goes in the contradiction log. Never compare a consolidated figure with an entity-level one without saying so.

Close with ownership and succession signals (dated), questions the evidence does not answer, a recommended next step, and the value test: facts we did not have before, claims contradicted with their size, and what Moody's did not cover.
```

**Output:** A two to three page brief: identity and ownership, three-column table, contradiction log, peer position, credit context, signals, open questions, next step and value test.

**Moody's:** M1: `findEntity`, `getEntityProfile`, `getEntityUltimateOwners` (threshold [25.01]), `getEntityShareholders`, `getEntityBeneficiaryOwners`, `getEntitySubsidiaries`, `getEntityManagersDirectors_v2`, `getShellCompanyIndicators`. M2: `getEntityFinancials_v2`, `getEntitySegmentFinancials`. M3: `getEntityPeers`, then `getEntityFinancials_v2` on each and `getEntityProbabilityOfDefault_v2` with `peers`. M4: `findEntity` on counterparties, then `getEntityRatings` or `getEntityProbabilityOfDefault_v2`. M5: `searchAllDocuments` with sector and region filters, discarding irrelevant hits. M6: `getEntityRatings`, `getCreditOpinion`, `getEntityProbabilityOfDefault_v2`.

Worked tip from testing: "PureGym Limited" resolved to a Swiss company; the registered name "Pure Gym Limited" resolved to the correct UK entity, and its ownership chain ran through Pinnacle Topco Ltd to the sponsor's fund, Green Equity Investors VII LP. Search by registered name, and check whether the financials belong to the group parent or a single entity (standing rules 9b and 9c). A company taken private can return financials that stop at the take-private date; mark them stale. Apply standing rule 12a to the peer list and to owner types. Treat shell-indicator counts as leads: they give counts, never names (standing rule 13). The full brief takes about 14 tool calls; run M1 to M3 first.

**Tested:** On Virgin Active International Limited, every step returned data: ownership through a British Virgin Islands vehicle to Brait PLC (typed "Bank") and Coronation Fund Managers, five years of financials (latest 31 Dec 2024, so marked stale), leverage of 7.96x and EBITDA cover of 1.45x calculated from reported figures, and a model PD at the 66th percentile of its peer group. Of eight peers returned, one was the company's own UK subsidiary, one a competitor's property entity, one an equipment maker and one a US company with withdrawn ratings. The shell indicators reported 12 directors and 13 flagged directors, against 2 directors in the directors record; record such a mismatch in the contradiction log. No customers or suppliers are named publicly for a consumer gym business, so M4 had nothing to run on.

**Follow-ups:**
- "Which entity in the group carries the acquisition debt? Pull its financials and PD too."
- "Rewrite the brief as five bullet points for the Monday meeting."

**Adjust for:** Buy-and-build: add whether the company could be a platform or an add-on. Carve-out: run M1 on the parent and use segment financials, stating that segment figures include allocated costs. Take-private: use SO-12 rules and do not prepare outreach. Turnaround: lead with M6. Growth buyout and growth equity: expect thin Moody's coverage; add funding history from the fund's licensed data. Lower mid-market: say when filed accounts are abbreviated or unaudited. Thematic funds: add the SO-02 test.

---

## Scoring and prioritisation

### SO-15 Score a list against the mandate

**Use case:** Rank a list of companies out of 100 against the mandate, showing business fit and likelihood of a deal separately.

**Inputs:** A list with the evidence gathered in SO-04 to SO-09; the fund's scoring rubric and weights.

**Prompt:**
```
Score each company on this list out of 100 using our sourcing rubric:

Fit subtotal: sector fit [20]; geography fit [10]; revenue scale [15]; recurring revenue [10]; market position [10]; management quality [10].
Actionability subtotal: ownership and succession [15]; acquisition signal strength [10].

For every dimension give the score, the evidence with its source, and an evidence grade: Documented (filing or Moody's record), Reported (press or company statement), or None (scored conservatively, gap logged). Management quality cannot be judged from a directors list; score it only on cited evidence such as awards or a complete, named team, and otherwise mark it None.

Bands: Priority [75] and above, Watchlist [55] to [74], Pass below [55]. Show the fit and actionability subtotals beside the total, and flag every company with high fit and low actionability as "cultivate". Exclude anything on our exclusion list [list] even if it scores well, and say why. Score only names that passed the SO-04 knock-outs; mark any other name "outside buy box" and do not band it. Check that every total equals the sum of its dimensions.
```

**Output:** A scored table with subtotals, evidence grades, bands and a cultivate flag.

**Moody's:** `getEntityFinancials_v2` lets revenue scale be scored from filed figures; `getEntityUltimateOwners` and `getEntityManagersDirectors_v2` give dated facts for ownership and succession; ownership information dates are capture dates and owner types need checking (standing rule 12a). Mark scores built without them "limited data". A peer average never stands in for a missing company figure.

**Follow-ups:**
- "Which companies would change band if one Reported item turned out to be wrong?"
- "Re-score with sector fit at [15] and signal strength at [15]. Who moves?"

**Adjust for:** Specialist and thematic funds: replace sector fit with value-chain position and fit with thesis conditions or theme exposure. Buy-and-build: use the SO-10 scores. Carve-out and take-private: use the SO-11 and SO-12 rubrics. Growth equity: give growth evidence its own dimension. Lower mid-market: expect many None grades on revenue; allow a stated proxy (sites, staff) and label it.

### SO-16 Challenge the ranking

**Use case:** Test the ranked list before it goes to a partner, so weak evidence does not reach the top.

**Inputs:** The scored list from SO-15 or SO-10, with its evidence and exclusion log.

**Prompt:**
```
Act as a sceptical reviewer of this ranked list for [thesis or platform].

1. For every Priority name (or the top [five] names if there are none), re-open each source behind its score and confirm the quote and context. Mark each dimension Confirmed, Revised or Unsupported, and re-score.
2. For the top [five] names, argue the case for the band below. Say what evidence would settle it.
3. Check signals: is any "signal" really a non-signal (being in a consolidating sector, an old website, the company's age alone)? Is any signal older than [12] months scored as current? Are several articles from one press release counted more than once?
4. Re-check a sample of [ten] excluded or Pass names for wrongful exclusion.
5. Confirm every entity match has a legal name, country and identifier.

Return the revised list, a change log (what moved and why), and the names where the evidence is too thin to rank.
```

**Output:** A revised list, a change log and a thin-evidence list.

**Moody's:** Re-run `findEntity` with `getAlternatives` on any name whose country, legal form or size looks wrong (standing rules 9 and 9a), and `getEntityFinancials_v2` to check the period end behind each size score. Check that no ownership signal rests on an information date read as an acquisition date (standing rule 12a).

**Follow-ups:**
- "Which change matters most for this week's calls?"

**Adjust for:** Large-cap: add a check that public figures are on the same accounting basis across names. Thematic funds: test whether theme exposure shares are evidenced or assumed.

---

## Relationship and outreach

### SO-17 Relationship check

**Use case:** Before anyone calls, find out whether the fund already knows the company or has passed on it.

**Inputs:** A list of companies; exports or search access to the fund's CRM, deal log and mailbox.

**Prompt:**
```
For each company on this list, search our CRM, deal log and mailbox under its legal name, trading names and the names of its owners and directors.

Label each one:
- New: no prior contact found. Say which searches you ran.
- Existing: summarise the last contact, its date, who at the fund owns the relationship, and anything promised.
- Previously passed: quote the recorded reason and date of the pass, and who made it.

For Previously passed names, list what has changed since (ownership, size, signals) and say whether it answers the reason we passed. Do not move a Previously passed name forward unless I confirm.

Also note any link between the company and our portfolio companies (customer, supplier, competitor) or our lenders and advisers.
```

**Output:** A labelled list with contact history, pass reasons, changes since, and portfolio links.

**Moody's:** `getEntityUltimateOwners` and `getEntityManagersDirectors_v2` give the owner and director names to search for in the CRM, which catches contacts filed under a person.

**Follow-ups:**
- "Which Existing relationships have gone quiet for more than [six] months?"
- "Draft a note to each relationship owner asking for an update."

**Adjust for:** Buy-and-build: add platform management's own contacts as a source. Take-private: refer every Existing name to compliance before continuing, because existing relationships are where inside information most often sits.

### SO-18 Owner outreach draft

**Use case:** Draft a short first message to an owner that gives a real reason for writing.

**Inputs:** The SO-14 brief or signal evidence; the relationship label; the name and role of the recipient; who at the fund will sign it.

**Prompt:**
```
Draft a first message from [sender, role] to [recipient, role] at [Company]. The relationship label is [New / Existing], and compliance has recorded no conflict.

Rules: under [150] words; open with one specific, cited thing the company has done (a new site, a contract, an acquisition, an award); say in one line who we are and what kind of owner we are ([strategy, typical hold, control or minority]); ask for a short conversation, with no attachment and no mention of price or valuation. Do not refer to private information about the owner, their age, health or family, or to anything that could only come from a non-public source. Do not claim the company is for sale.

Give two versions: email and a shorter LinkedIn message. Then list the facts used and the source of each, so the sender can check them before sending.
```

**Output:** Two short drafts and a fact list with sources. Drafts only; the sender reviews and sends.

**Moody's:** Not needed. Use facts already cited in the SO-14 brief.

**Follow-ups:**
- "Write a follow-up for [three] weeks later if there is no reply, with a different reason to talk."
- "Rewrite for an owner who has said publicly they are not selling."

**Adjust for:** Lower mid-market founders: plainer language, and lead with continuity for staff and customers. Minority and growth equity: lead with what the capital would fund and that the owner keeps control. Carve-out: address the parent's corporate development team; do not write to the division's managers. Take-private: do not use this prompt; no contact without counsel's advice.

### SO-19 Intermediary coverage plan

**Use case:** Decide which advisers and brokers to cover so the fund sees the right processes early.

**Inputs:** The buy box; the fund's deal log with the adviser on each deal seen; public deal announcements in the sector.

**Prompt:**
```
Build an intermediary coverage plan for [sector or thesis] in [geography].

1. From our deal log, list every adviser (corporate finance boutiques, banks, accountancy firms, lawyers, brokers) who brought us a deal in the last [three] years, with the number of deals and how many fitted our buy box.
2. From public deal announcements in the sector over the same period, list advisers who acted on deals of our size and type, with the deal and its source. Mark those we have never heard from.
3. For each adviser, give: coverage owner at the fund, contact cadence, what they last sent us, and the gap (for example strong in the sector but sends us deals outside our size range).
4. Rank the top [ten] advisers to prioritise and give one line on what to tell each about our buy box.

Do not guess who acted on a deal; list only advisers named in a source.
```

**Output:** A coverage table with owners and cadence, a list of uncovered advisers and a ranked priority list.

**Moody's:** `searchNews` on completed deals in the sector can show advisers where articles name them; check each at source. Search by the deal (acquirer and target names together) or by the acquirer entity, keep only items that name an adviser, and note the two-year window limit (standing rule 12b). Advisers are rarely named; an adviser's own press release is the most common source. Moody's does not hold adviser league tables.

**Follow-ups:**
- "Draft the one-page buy box we send to the top [ten]."
- "Which advisers sent us deals we passed on for reasons they could have screened out?"

**Adjust for:** Lower mid-market: add accountants, lawyers and business brokers who advise founders, since many deals never reach a bank process. Large-cap: focus on banks and the leveraged-finance desks that know refinancing timetables. Carve-out: add corporate development teams and the advisers who run divestments.

### SO-20 Weekly sourcing digest

**Use case:** Give the deal team one page each week on what changed in the pipeline and what to do next.

**Inputs:** The current watchlist with scores and owners; this week's signal and news results; CRM activity.

**Prompt:**
```
Write this week's sourcing digest for [fund or team], covering [date range].

Sections:
1. Headline: three lines on what changed.
2. New names added, with score, band and the signal that brought them in.
3. Movers: names whose score or band changed, old and new score, and the reason.
4. Events: significant news, ownership or credit changes on watchlist names, each dated and sourced.
5. Outreach: messages sent, replies, meetings booked, owner of each.
6. Actions for next week: up to [five], each with an owner and a date.
7. Stale items: names with no activity for [60] days, and signals now older than [12] months.

Keep it to one page. Every fact carries its source. If the scored watchlist or CRM export is not attached, write the affected sections as [GAP] and name the file needed; do not invent scores or activity. Draft for [reviewer]; do not send.
```

**Output:** A one-page digest with actions and owners.

**Moody's:** `searchNews` on watchlist entities (one per call) with `timeRange` of [7] days, `importance` set to significance and `searchType` keyword, so a quiet week returns nothing rather than loosely related items; apply standing rule 12b, and expect generic names to return unrelated articles. `getEntityRatings` and `getEntityProbabilityOfDefault_v2` with `early_warning` for credit changes (standing rule 11); `getEntityUltimateOwners` re-run on Priority names for ownership changes. Each run is a fresh retrieval; it does not track changes since last week unless the fund keeps last week's results (standing rule 13a).

**Tested:** For a seven-name UK leisure watchlist, the Moody's sections (headline, events, credit) were accurate and dated: one acquisition, one disclosed stake increase, a buyback, and no rating changes. A keyword search returned a clean "No news found" for one company; for a company with a generic name ("Third Space") it still returned ten unrelated items. Four of seven sections depended on CRM and scores and were left as [GAP].

**Follow-ups:**
- "Which Priority name has gone longest without a contact attempt?"
- "Compare this week's pipeline counts with last month's."

**Adjust for:** Buy-and-build: add a section per platform. Take-private: circulate to the deal partner and General Counsel only, using code names. Funds with several vehicles by size band: split the digest by fund.
