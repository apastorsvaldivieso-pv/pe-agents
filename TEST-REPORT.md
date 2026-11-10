# Test report: prompts against the Moody's connector

**Date:** 10 November 2026
**Connection:** the Moody's MCP connector in this Claude workspace. It exposed 24 tools, listed in `shared/moodys-evidence-layer.md`.
**Test companies:** real companies chosen because their ownership and debt structures are typical of private equity deals. Moody's data for them is quoted below as returned. Nothing here is an opinion on any of these companies.

| Company | Why it was chosen |
|---|---|
| Pure Gym Limited (UK) | Private operating company, sponsor-owned, files accounts |
| Pinnacle Topco Limited (UK) | Its holding company, which carries the buyout debt |
| Zendesk, Inc. (US) | Taken private in 2022 |
| McAfee Corp. (US) | Rated, sponsor-backed borrower |
| The Gym Group PLC (UK) | Listed peer, for take-private and filings tests |
| Virgin Active, David Lloyd Leisure | Peers, for an ownership sweep |
| Meridian Field Services | The fictional company in the original demo, for the "not covered" path |

---

## Final result: every Moody's prompt run in its current form

**Rounds:** round 1 ran 46 prompts and found the problems; the prompts and standing rules were rewritten; round 2 re-ran those 46 in their fixed form and ran the other 49 for the first time; small defects found in round 2 were fixed and the changed steps re-run. Verdicts below come from the latest run of each prompt.

| Stage | Moody's prompts | Pass | Pass with caveats | Fail | Verdict on Moody's steps only |
|---|---|---|---|---|---|
| Sourcing and origination | 19 | 2 | 17 | 0 | 3 |
| Screening | 10 | 3 | 7 | 0 | 0 |
| Due diligence | 25 | 4 | 21 | 0 | 0 |
| Valuation and committee | 5 | 0 | 5 | 0 | 0 |
| Portfolio monitoring | 10 | 1 | 9 | 0 | 6 |
| Value creation | 19 | 0 | 19 | 0 | 4 |
| Exit | 7 | 0 | 7 | 0 | 0 |
| **Total** | **95** | **10** | **85** | **0** | **13** |

**Pass** means a correct, cited, useful output with no change needed. **Pass with caveats** means the output was correct and useful, and a limit remains that the prompt now states (most often: a private input such as a data room, monthly pack, CRM or seller roster that a test cannot supply, or a connector limit listed in the standing rules). The 13 prompts in the last column ran every Moody's step, but their core input is private, so the verdict covers the Moody's steps only.

**Round 1 problems re-checked in round 2:** 200 problems tracked across the 46 re-run prompts. 142 fixed, 24 partly fixed, 31 not fixed, 3 not retested. Every problem not fixed is either a private input the prompt now names, or a connector behaviour no prompt wording can change (listed below).

**Two round 1 failures resolved:** DD-27 (integrity facts) after a redesign around what the connector supplies; VC-25 (sales capacity) on a company with a real sales force (The Sage Group plc), after the first test company turned out to sell through an app.

**My own spot checks against Moody's** (rather than trusting the testers): Pinnacle Bidco plc B2, upgraded 28 September 2026; Wagamama (Holdings) Limited B2, negative; Project Aurora Holdco 1 Limited B2; Wheel Bidco Limited Caa1 with a PD implied rating of Ba1 from a model with no financials; McAfee's revolving facility maturing 1 March 2027; PureGym's £216m revolving facility tested above 40% drawn, notes due 2032, and 2026 leverage revised from 6.2x to about 5.8x. All confirmed. One tester claim was not confirmed (that a search for Pinnacle Topco Limited returns Pinnacle Bidco plc as the main match; it returned Topco with Bidco as an alternative), and one apparent data error was a definition difference (Hollywood Bowl interest of £0.46m in Moody's against £13.94m in the annual report, most likely lease interest). My check also found that an exact plc name ("Hollywood Bowl Group plc") can resolve to a single 40-employee branch; standing rule 9a now covers this.

**Connector behaviours no prompt can fix** (each stated in the standing rules):

- Name searches for buyout vehicles and some plcs return a different company, a site or a branch; BvD ID lookups fix it.
- Some names disappear from both the matched and not-found lists.
- Early-warning history returns only the latest risk category, not monthly history.
- The credit opinion's key-indicators section is sometimes empty; the summary text holds the figures.
- Peer lists for rated or holding entities are broad sector lists.
- Owner-type labels are sometimes wrong; ownership dates are capture dates.
- No country profile was available for Switzerland in two attempts.
- No staff-cost or sales and marketing lines in the financial records tested.
- News for short or common names is mostly about other companies.
- Filings can take up to about fifteen minutes to load the first time.
- The scorecard what-if returned no result for the two companies tried.

**What was not tested:** the 60 prompts that do not use Moody's (they work only on the fund's own documents) were checked for structure, not run. Results reflect Moody's data on 10 November 2026 and one live connection.

---

## Workflow skills run end to end

The multi-agent skills in the stage folders were rewritten to use the tested Moody's steps, then three were run end to end on companies not used in the prompt tests, every seat in order, with tie-out checks reported. Outputs and test reports are in [`test-runs/skills/`](test-runs/skills/).

| Skill | Test case | Verdict | What the run found | Fixed in the skill |
|---|---|---|---|---|
| Target screening brief (sourcing) | Cognita (rated issuer Lernen Bidco Limited, B3) and Tangle Teezer (unrated) | Pass with caveats | Correct cited briefs. Cognita's peer list mixed in gyms, vets and charitable schools. Tangle Teezer was bought by a corporate in December 2024, so it is no longer sponsor-owned. | Group mapping and rated issuer first; credit opinion and research; peer screening; EBITDA checks; staleness at 15 months |
| Early-warning watchlist (monitoring) | 7 holdings across the UK, Jersey and the US, including stressed names | Pass with caveats | Merlin: secured debt downgraded to Caa2 and outlook negative on 16 September 2026 (CRITICAL alert). Cloud Software Group: first-lien ratings cut to B2 on 3 November 2026. GAIL's: leadership change. 26 of 53 news items were about other companies. | Count check now handles discarded news; group mapping in seat 01; search method in seat 02; rules for a first run, Watch items and instrument-only downgrades |
| Financial due diligence (diligence) | Hollywood Bowl Group plc, annual reports as the data room | Pass with caveats | Report and quality-of-earnings workbook computed in a script. No tie-out fails; six checks Not built because a public test has no ledgers. Management's after-rent EBITDA deducts property rent only; the fund-basis figure is 7.9% lower for FY2025. | BvD ID lookup; EBITDA rebuilt from statement lines; two lease bases with matching lease debt; year-end-only peg statement; public-information mode |

**Checked against Moody's myself:** Lernen Bidco Limited B3; the Merlin and Cloud Software Group rating actions (exact dates and rating moves confirmed in the rating action texts); Hollywood Bowl Group PLC found by BvD ID GB10229630 when name searches returned branches.

**What the skill runs added to what the prompt tests showed.** Both earlier skill drafts let the engine run credit checks on the company itself and use peer lists unscreened. The engine produced correct outputs only because it applied the standing rules on its own. Every skill's Moody's section now opens with the tested instructions (group map, rated issuer first, credit-opinion metrics, peer screening), and the M1 to M8 step definitions in `shared/moodys-evidence-layer.md` were rewritten. The other 20 skills in the stage folders received these text changes but were not run end to end. The two original skills in `Sourcing & Origination Agent/` (stored inside zip files) were not changed.

---

## Summary of the full test (round 1 of the full test)

46 prompts were run end to end on real companies through the live connector, each exactly as written, under the standing rules, by four testers in parallel. Each run is saved in [`test-runs/`](test-runs/) with the tool calls, the full output, a verdict and the suggested change.

| Stage | Prompts run | Pass | Pass with caveats | Fail |
|---|---|---|---|---|
| Sourcing and origination | 12 | 0 | 12 | 0 |
| Screening, due diligence, investment committee | 13 | 0 | 12 (13 after DD-27 redesign) | 1 (DD-27, first version) |
| Portfolio monitoring | 9 | 0 | 9 | 0 |
| Value creation and exit | 12 | 0 | 12 | 0 |
| **Total** | **46** | **0** | **45 (46 after redesign)** | **1 (0 after redesign)** |

"Pass with caveats" means the output was correct and useful, but the prompt needed a change or the data has a limit the prompt should state. No run produced an invented figure, enterprise value or equity multiple, and ratings were kept apart from model outputs in every run.

**What the connector does well for private equity**

- Filed financials for private UK and European companies, with ownership chains up to the sponsor fund.
- Assigned ratings with history, and credit opinions that give leverage, maturities, covenant triggers and deal facts (one opinion flagged a borrower's revolving facility due 1 March 2027).
- Add-on acquisitions and stress events found through entity news.
- Segment financials that sum to group revenue and reveal disposals.
- Default-probability history and peer percentiles for unrated companies.

**What goes wrong, and the rule that now handles it**

| Problem seen | How often | Standing rule |
|---|---|---|
| The assigned rating sits on a Bidco or Holdco the search did not return | 4 buyout groups (Pure Gym, The Restaurant Group, Spectris, PizzaExpress) | 9b |
| A name returns a different company, merges with another, or vanishes, with no warning | At least 9 names across three testers | 9, 9a |
| The record holds one entity's own accounts, not the group's | 3 groups | 9c |
| Model outputs far from the assigned rating | Every rated borrower: 2 to 9 notches better | 11 |
| "Adjusted" financials differ from the credit opinion's adjusted metrics | 8.08x against 5.9x for one holding company | 11a |
| Peer lists include the company's own group, property entities, duplicates | 6 of 10 peers dropped in one run | 12a |
| News about other companies with similar names | About 30 of 39 items in one run; 9 of 10 in another | 12b |
| Filings stuck "ingesting" | Two listed companies after three attempts | 12c |
| Criteria search returns nothing or unrelated companies | Every attempt | 13 |
| Director red flags come as counts, never names | Every run of the integrity prompt | 13 (DD-27 redesigned) |

**Why DD-27 failed.** It asked for per-person integrity facts on directors and owners. The tools give counts of flagged directors with no names, and the ownership chain returned for one group (a security trustee holding 100% above a holding company with 435 employees) does not describe real control. The prompt has been redesigned to what the connector supplies: the ownership chain with a coherence check, officers and appointments, counts of shell-company indicators, adverse news, and a list of what needs another source. Re-run on the Pure Gym group it passed with caveats, producing cited facts and five registry requests, including a top holding company whose share register sums to only 48.41%.

**Fixes applied after the full run.** Each tester applied its suggested changes to its own stage file; I rewrote the standing rules. Results:

| File | Tested prompts changed | Untested prompts given the same fix |
|---|---|---|
| `00-standing-rules.md` | Moody's section rewritten: rules 9 to 13a | n/a |
| `01-sourcing-and-origination.md` | 12 | 6 |
| `02`, `03`, `04` (screening, diligence, committee) | 13, including the DD-27 redesign | 4 |
| `05-portfolio-monitoring.md` | 9 | 1 |
| `06-value-creation.md`, `07-exit.md` | 12 | 6 |

All 155 prompts were re-checked after the edits: complete fields, IDs in sequence, every Moody's tool and standing-rule reference valid, every cross-reference resolving. 43 prompts carry a "Tested:" note with what the run found. The fixed prompts were not re-run, except DD-27.

---

## 1. Results by prompt (first and second rounds)

| Prompt | What was run | Result | Verdict |
|---|---|---|---|
| Standing rule 9 (entity check) | `findEntity` on "PureGym Limited", then with a UK filter, then on "Pure Gym Limited" | The first two returned PureGym AG in Switzerland. The registered name returned the right UK company (company number 06690189). | **Rule needed and works.** The prompt's instruction to print the match and stop on a mismatch catches this. |
| DD-36 External corroboration | `findEntity` (company, parent, fictional company in one call) → profile → owners → subsidiaries → financials → peers | Company and parent resolved; the fictional company returned "not found". Three years of filed financials for both entities; ownership chain up to the sponsor fund; 26 group companies; five peers. | **Works.** The "not covered" path is clean. |
| DD-36 follow-up "Which entity carries the acquisition debt?" | Financials and default probability for the operating company and its parent | See section 2. The answer changes the credit picture completely. | **Follow-up essential.** Promoted into the evidence layer as behaviour 2. |
| DD-04 Red-flag scan (Moody's part) | `searchNews`, 180 days, significance | One relevant item: an add-on acquisition on 19 October 2026 | **Works,** and doubles as a buy-and-build signal for sourcing. |
| DD-15 / sector context | `getEntitySectorOutlook`; then `searchAllDocuments` with a UK filter, recent research | Sector outlook returned about 156,000 characters, too large to use. The search returned 10 documents, 2 relevant (UK credit conditions, September 2026; European consumer, September 2026). | **Revised.** Prompts use the filtered search with a sector filter and ask Claude to discard irrelevant hits. |
| DD-22 Map the roles | `getEntityManagersDirectors_v2` on Zendesk | One name (the chief executive) | **Works with a caveat** now written into the prompt: the record can be thin. |
| DD-27 Integrity facts | Ultimate owners on Zendesk | Chain stops at Zoro Bidco, Inc. (November 2022); no sponsor shown | **Caveat added:** ask for the vehicle's own shareholders. |
| SO ownership sweep (step M8) | Ultimate owners at 25.01% for three peers in one call | David Lloyd Leisure: TDR Capital LLP, typed "private equity firm". Virgin Active: Brait PLC (typed "bank") and Coronation Fund Managers. The Gym Group: no owner above 25%. | **Works** for flagging sponsor ownership. Owner type labels need checking. |
| SO universe build | `findEntity` "top 15 privately held logistics companies in Texas by revenue"; then "logistics companies in Texas" | First: nothing. Second: 15 results including airlines, railroads and pass-through trusts, mostly outside Texas. | **Fails as a universe tool.** Sourcing prompts treat it as a seed only and require a licensed bulk universe. |
| IC financing context | Rating and credit opinion for McAfee | B2 corporate family rating, stable, upgraded from B3 in April 2024. Credit opinion of 4 February 2026: downgrade if debt to EBITDA stays above 7.5x or free cash flow to debt falls below 2%; leverage 8.2x on Moody's adjusted figures. | **Works.** Gives explicit thresholds for a financing-context paragraph. |
| PM early-warning sweep | Default probability with a 12-month history, early warning and peers, on the holding company | Monthly default probability between 1.85% and 2.26%, PD implied rating Ba3 throughout, risk category Low, 68th percentile of 82,443 UK unlisted companies | **Works.** Enough for a watchlist line and trend. |
| Take-private filings | `searchCompanyFilings` on The Gym Group (annual and transcript) | First two calls: "ingesting". Third: about 57,000 characters from the 2024 annual report (891,000 members at year end; revenue £226.3m). | **Works after a wait.** Prompts pass `reportYears` and a narrow query, and warn about the ingestion delay. |
| Stale data | Financials for Zendesk | Last period September 2022; 2022 to 2025 not available | **Rule 10 needed;** the STALE label applies. |

### Second round: prompts from the other stage files

| Prompt | What was run | Result | Verdict |
|---|---|---|---|
| SC-08 Quick external check | Financials for David Lloyd Leisure GR Limited (a sponsor-owned UK leisure group entity) | EBITDA £84.3m on revenue £60.5m (2025); debt £633.1m. The entity is a property or financing company inside the group. | **Caught by a new standing rule (12a):** sanity-check figures and find the trading entity. |
| SO-11 Carve-out candidates | Segment financials for Smiths Group plc, 2024 and 2025 | Business segments sum exactly to group revenue in 2025. Smiths Interconnect appears in 2024 and not in 2025, a disposal signal. Header says GBP while figures sit in the USD column; two segment names garbled. | **Works,** with a currency and naming check now written into the prompt. |
| IC-06 Financing context, step 4 | Scorecard retrieval, then a what-if at 6.0x debt to EBITDA, on McAfee | Retrieval returned about 70,000 characters (Software methodology, B2). The what-if returned "No subfactor values available to score for this methodology". | **Step fails for this company.** Prompt now falls back to the published scorecard in the credit opinion. |
| EX-06 Buyer universe | Topic-only news search for gym acquisitions over 12 months | One relevant item out of ten | **Revised:** search news by entity, one buyer at a time. |
| SO-06 / SO-07 Ownership and sponsor signals | Ultimate owners at 25.01% for three companies in one call | Sponsor ownership identified for one; owner types partly mislabelled | **Works,** with a check on owner type. |

## 2. The finding that matters most for private equity

The same prompt gives very different answers depending on which company in a buyout group it is run on.

| | Pure Gym Limited (operating company) | Pinnacle Topco Limited (holding company) |
|---|---|---|
| Revenue 2025 | £463.6m | £742.4m |
| EBITDA 2025 | £242.4m | £294.1m |
| Total debt 2025 | £590.6m | £2,376.6m |
| Debt ÷ EBITDA | 2.4x | 8.1x |
| EBITDA ÷ finance costs | not returned | 1.37x |
| One-year default probability | 0.20% | 1.92% |
| PD implied rating (a model output, not a rating) | A3 | Ba3 |
| Peer group used | UK unlisted consumer services | UK unlisted business services |
| Percentile in peer group | 5th | 68th |

**Correction found in the second round of testing.** Neither of these is the group's credit rating. The assigned rating sits on a third entity, **Pinnacle Bidco plc: B2 corporate family rating, upgraded from B3 on 28 September 2026, stable outlook** (`getEntityRatings`, as of 10 November 2026). A first-round search on the operating company and the top holding company did not surface it. Standing rule 11 now tells Claude to search the group's Bidco, Holdco, Holdings and Finco names for a rated issuer before calling a company unrated.

Source: Moody's, `getEntityFinancials_v2` and `getEntityProbabilityOfDefault_v2`, as of 1 November 2026, financial periods to 31 December 2025. Ratios calculated from the returned figures.

**What this means for the prompts.** Any credit, leverage or covenant question about a sponsor-owned company must run on the right entity in the group. Quote the assigned rating of the rated issuer (often the Bidco) first; use the top holding company's consolidated figures for group leverage; and use model outputs only where no rating exists. Every diligence, financing and monitoring prompt now says so, and `findEntity` results must be checked against the group structure from `getEntitySubsidiaries` and `getEntityUltimateOwners`.

## 3. What the tests did not cover

- Prompts were run as tool sequences, with the results read and checked by hand. They were not run as complete conversations producing finished documents.
- No counterparty scan on a real customer list (step M4) was run. The scorecard what-if was run once and failed for that company; it was not tried on others.
- Of the 155 prompts, those that call Moody's were sampled: about a dozen were exercised directly. Prompts that work only on the fund's own documents (most of diligence, value creation and exit) were checked for structure, not run.
- Every company tested is large enough to file detailed accounts. Smaller private companies in the lower mid-market may return much less.
- One live connection was tested. Another client's connection may expose fewer tools, depending on entitlements.
- Results depend on the data on 10 November 2026 and will change.

## 4. Changes made because of the tests

1. Standing rules 9 to 13, and 12a (sanity-check entity figures), added to `00-standing-rules.md`.
2. The verified tool map and six known behaviours added to `shared/moodys-evidence-layer.md`, replacing the earlier list built from public listings.
3. DD-22, DD-27 and DD-36 carry the specific caveats above.
4. All stage files use the filtered research search in place of the raw sector outlook, and treat the criteria search as a seed only.
5. SC-08, SO-11, IC-06, EX-06 and DD-36 carry a "Tested" note with what happened and what to do.
