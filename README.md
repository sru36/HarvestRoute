# HarvestRoute 🌾
### A clearer way to compare where a harvest may actually pay

HarvestRoute is a proposed decision-support tool for smallholder vegetable farmers. It helps compare feasible selling markets using the estimated price for a crop lot, its grade and quantity, and the costs involved in taking it to market.

The goal is not to predict a guaranteed selling price. The goal is to make the available information easier to compare, explain the assumptions, and highlight when the information is incomplete or needs verification.

> **Prototype status:** Any sample prices or costs used in a demo must be labelled as illustrative unless they have been checked against a reliable source. A reported mandi price is not a guaranteed offer for an individual farmer's produce.

## The problem

Farmers may see different reported prices across markets, but the market with the highest headline price may not provide the highest amount after transport, loading, commission, grade deductions, and other selling costs. Price records can also be stale, incomplete, or difficult to compare when units and grades differ. Connectivity may be unreliable, so the age of cached information must remain visible.

## Workflow

1. **Enter harvest details**  
   The farmer or facilitator enters the crop, grade/variety where available, quantity and unit, origin, and any relevant selling deadline.

2. **Collect or select market price records**  
   Candidate markets are added using available price records or verified quotes. Each record should identify the market, price type, value, unit, source, observation time, and retrieval time where available.

3. **Validate the data**  
   The system checks whether crop, grade, unit, and timestamp information is sufficient for a fair comparison. Missing, stale, or conflicting information is flagged. Incomparable records are not silently ranked.

4. **Confirm costs and assumptions**  
   The farmer or facilitator enters or confirms transport, loading, commission, and other selling costs. Any expected loss or downgrade is an explicit assumption. Unknown costs are not silently treated as zero.

5. **Calculate estimated net realisation**  
   The calculation is performed by deterministic code, so the result can be reproduced and checked.

   - **Saleable quantity** = entered quantity × (1 − assumed loss percentage / 100)
   - **Estimated net realisation** = saleable quantity × matched price − transport − loading − commission − other selling costs

   Units must be consistent before calculating. For example, 1 quintal = 100 kg.

6. **Compare feasible options**  
   The system presents the estimated net amount for each comparable market along with its price, costs, assumptions, and data-quality status. It ranks markets only when the required inputs are sufficiently complete.

7. **Explain uncertainty and request verification**  
   The result should show the source and age of price information, missing inputs, conflicting records, and any assumptions. If the data is insufficient, the system should say so instead of inventing a “best market.”

8. **Human verification and farmer decision**  
   The farmer or facilitator verifies the obtainable quote, grade, costs, and route practicality. HarvestRoute supports the decision; the farmer retains final authority.

9. **Optional outcome recording**  
   Where practical, the actual sale price, costs, and outcome can be recorded later to help evaluate the quality of estimates. This is optional and should not be presented as proof of improved income until evaluated.

## AI, code, and human responsibilities

- **AI (optional):** Parse Hindi or mixed-language inputs and explain comparisons in plain language, grounded in the displayed data.
- **Deterministic code:** Validate inputs, convert units, apply freshness rules, calculate net realisation, and rank eligible markets.
- **People:** Verify quotes, grades, costs, and route practicality; decide whether and where to sell.

AI must not invent prices, turn uncertain estimates into guarantees, or independently make selling decisions.

## Handling unreliable data and connectivity

- **Source unavailable:** Use a timestamped cache with a clear non-live warning, or show that no usable data is available.
- **Missing price, unit, or time:** Mark the record incomplete and avoid unsupported rankings.
- **Conflicting sources:** Show the disagreement and request verification.
- **Unknown cost:** Ask for the cost or show a clearly labelled scenario; never silently assume zero.
- **Invalid quantity or unit:** Block the calculation and explain what needs correction.
- **Ambiguous AI-parsed input:** Ask the user to confirm it before calculating.
- **Offline use:** Display cached information with its timestamp and make clear that it may not be current.

## Initial scope

The first version is intended to support **one crop and 3–4 candidate markets**. It focuses on comparing estimated net realisation from entered or reported information.

### Out of scope for the initial version

- Guaranteed future-price prediction or automated harvest timing
- Automated buying, selling, payments, or buyer contracts
- Claims of nationwide or real-time data coverage without verification
- Automated crop diagnosis or agronomic prescriptions
- Claims of increased income or reduced waste before evaluation

## Suggested feasibility tests

| Test | Expected behaviour |
|---|---|
| A market has a higher headline price but higher costs | Compare estimated net realisation, not headline price alone |
| A price record is stale | Display a stale-data warning and request verification |
| A selling cost is unknown | Request the input or clearly label a scenario |
| Price units differ (kg vs quintal) | Convert correctly only when the records are otherwise comparable |
| Two sources disagree | Show the conflict instead of silently selecting one |
| Cached data is used offline | Keep the cache timestamp and non-live warning visible |
| An input parsed by AI is ambiguous | Ask for confirmation before calculation |

## Data and responsible use

Public price sources such as AGMARKNET/OGD may provide reported market information, but a reported minimum, maximum, or modal price is not interchangeable with a confirmed transaction quote. Data access, licensing, API limits, update frequency, and local coverage must be verified before integration.

HarvestRoute is a decision-support prototype, not a guarantee of price, yield, profit, or income. Estimates depend on the accuracy and freshness of inputs and should be checked with local market participants before action.

## Repository documents

The `docs/` folder contains the supporting hackathon materials:

- `HarvestRoute-Final-Solution-Presentation.pptx` — solution presentation
- `HarvestRoute_Final_Basic_SRS_Implementation_Plan.pdf` — requirements and implementation plan
- `Synergy_Decision_Trail.docx` — record of the team's problem-framing and design decisions

## Future implementation steps

1. Audit available price data for one crop and 3–4 markets.
2. Implement the calculation and unit tests.
3. Build a mobile-friendly input and comparison interface.
4. Add data-quality checks and timestamped caching.
5. Run the feasibility tests above and a supervised shadow test.
6. Document results, failures, limitations, and evidence needed for the next version.

---

**AI Synergy Hackathon 2026 · Agriculture Case 02 · HarvestRoute**
