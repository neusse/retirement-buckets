# Research notes: buckets and guardrails

**Video:** [I Tested 4 Ways to Live Off $1 Million in Retirement — Here’s What Won](https://www.youtube.com/watch?v=IHm81JJyZX8), Antonio Cibella – Vision Wealth Management, published September 11, 2026. The notes below summarize the speaker’s ideas; they are not a transcript or personal investment recommendation. **Scope finding:** this is about retirement-income planning, not job-search organization. Its financial rules should not be added to the AI Job Search workflow as if they were job-search logic.

## Video summary

The speaker compares four ways a hypothetical couple might draw income from a $1 million portfolio: a conventional 60/40 portfolio with scheduled withdrawals, an income-first portfolio, a portfolio combined with guaranteed lifetime income, and a bucket portfolio with spending guardrails. He reports testing 69 rolling 30-year retirement periods, a 35-year longevity case, and selected difficult and favorable start dates. Those are the video’s reported modeling choices and results, not an independent reproduction of the analysis. See the [video description](https://www.youtube.com/watch?v=IHm81JJyZX8) and the strategy discussion around [16:00](https://www.youtube.com/watch?v=IHm81JJyZX8&t=960s), [18:00](https://www.youtube.com/watch?v=IHm81JJyZX8&t=1080s), and [24:00–29:30](https://www.youtube.com/watch?v=IHm81JJyZX8&t=1440s).

In the example bucket allocation, $94,000 goes to cash (about two years of the example paycheck), $235,000 to a “stable income” bucket (roughly five more years), and $671,000 to long-term growth. The paycheck is initially paid from cash; favorable markets can be used to refill shorter-term buckets, while the middle bucket is meant to bridge periods when selling growth assets is unattractive. The video does not specify a complete investable-security mix for the middle bucket or a mechanical definition of when markets “cooperate.”

The speaker’s simplified guardrail illustration starts at a 4.7% withdrawal rate. At an annual review, if the current withdrawal rate is more than 20% above the initial rate, planned spending is reduced by 10%; if it is at least 20% below the initial rate, planned spending is raised by 10%. The speaker describes cuts as temporary until recovery and raises as lasting, but does not fully specify the temporary-cut recovery/reset calculation in the video. He explicitly says this simplified test is less nuanced than the system he uses with clients. Do not silently fill in that missing behavior and call it an exact reproduction.

The reported results emphasize a trade-off: flexible spending can respond to poor or strong markets, but the strategy can require substantial cuts in a bad start and may leave a smaller final balance in a good start because more money was spent. For example, the speaker reports eight cuts and three raises in his 1966-start scenario, and seven raises without cuts in the 1988-start scenario. These are the speaker’s outputs for his assumptions; they do not establish that the rules are suitable for another household.

## How to implement the financial idea as a separate, deterministic planner

The AI Job Search project is not a retirement planner. If a retirement calculator is desired, keep it as a separate local tool or project with an auditable calculation engine. Do not put financial assets, withdrawal rules, or trade execution into the candidate profile or job-ranking files. The sequence below turns the video’s concepts into a reviewable model while marking assumptions the video leaves open.

### 1. Collect and label inputs before calculating

Capture, with “as of” dates and sources:

- investable portfolio balance, by account and tax treatment; exclude a home or other assets unless the plan explicitly models their use;
- essential and discretionary annual spending, taxes, health costs, and one-time expenses;
- expected guaranteed income by start date and inflation treatment, with Social Security estimates taken from the person’s [SSA account](https://www.ssa.gov/prepare/get-benefits-estimate) and pensions entered from plan documents;
- time horizon, household members/beneficiaries, liquidity needs, target allocation, fees, and explicit return/inflation assumptions.

Define the initial portfolio-funded annual draw `W0` consistently: project spending and taxes less other income that is actually available in that year. State whether it is gross portfolio withdrawals or spendable after-tax cash. Do not mix the two. A useful calculation is `r0 = W0 / P0`, where `P0` is the investable portfolio supporting withdrawals. The video’s 4.7% implies $47,000 on $1 million only if that is the relevant initial draw and denominator; that is arithmetic from the example, not a recommended rate.

Treat future Social Security as a dated cash-flow stream rather than an immediate offset. Review its official personalized estimate and claimed start age; SSA says its estimates depend on earnings and when a person applies ([SSA](https://www.ssa.gov/prepare/get-benefits-estimate)). If the plan uses CPI adjustments, document the series and period; BLS’s calculator uses the all-items, not-seasonally-adjusted U.S. CPI-U series ([BLS CPI calculator](https://www.bls.gov/data/inflation_calculator.htm)). A personal budget can differ from CPI-U.

### 2. Size the buckets from the plan’s cash need, not from the video’s dollar amounts

Use the example only as a transparent starting layout:

| Bucket | Video example | Planning purpose | Possible implementation choices |
|---|---:|---|---|
| Near-term cash | $94,000 (~2 years) | Pay scheduled bills without needing to sell growth assets that month | Insured bank deposits within applicable limits, or a short Treasury-bill ladder. Treasury bills mature from 4 to 52 weeks and can also be sold early, with market-price risk before maturity ([TreasuryDirect](https://www.treasurydirect.gov/marketable-securities/treasury-bills/)). FDIC coverage depends on depositor, bank, and ownership category; the standard limit is not simply “$250,000 per account” ([FDIC](https://www.fdic.gov/resources/deposit-insurance/brochures/insured-deposits)). |
| Stable income / bridge | $235,000 (~5 more years) | Provide intermediate liquidity and diversify the source of withdrawals | Specify a target duration, credit quality, and maturity ladder. Treasury securities or TIPS may be candidates, but TIPS principal can move with CPI and can be below its original amount before maturity; Treasury promises the original or adjusted principal at maturity, whichever is greater ([TreasuryDirect TIPS](https://www.treasurydirect.gov/marketable-securities/tips/)). The video does not define this bucket’s holdings. |
| Long-term growth | $671,000 | Support spending over a long horizon and replenish the near-term buckets under a prewritten rule | Use a diversified allocation chosen for the household’s time horizon and risk tolerance; the SEC explains that asset allocation depends on those factors and that rebalancing can have tax and transaction costs ([Investor.gov](https://www.investor.gov/additional-resources/general-resources/publications-research/info-sheets/beginners-guide-asset)). |

For a real plan, set cash target `B1 = years_cash × W0` and bridge target `B2 = years_bridge × W0`; set the growth remainder `B3 = P0 − B1 − B2`, then check that the remainder and risk mix are acceptable. Recalculate `W0` when reliable income begins or household spending changes. Two plus five years is the video’s example, not a universal allocation. A seven-year reserve may trade off near-term stability against inflation and growth; compare explicit scenarios rather than treating the bucket labels as risk protection.

Write the refill policy before investing. One testable choice is to pay monthly bills from B1; at a scheduled annual review, refill B1 from maturing B2 assets and cash flows, then use B3 only when its allocation is above a preselected rebalance band or another explicit rule is met. If B3 is below the band, draw from the already-funded bridge and document that the bridge is finite. Rebalancing on a calendar or preselected allocation bands is more auditable than an undefined “sell after a good year” rule; SEC materials describe both calendar- and threshold-based reviews and call out tax/fee effects ([Investor.gov](https://www.investor.gov/additional-resources/general-resources/publications-research/info-sheets/beginners-guide-asset)).

### 3. Make the guardrails executable and unambiguous

For a model that follows the video’s simplified illustration, record these policy inputs:

```text
initial_portfolio_value = P0
initial_withdrawal_rate = r0 = W0 / P0
upper_trigger = 1.20 * r0
lower_trigger = 0.80 * r0
upper_action = reduce planned annual withdrawal by 10%
lower_action = increase planned annual withdrawal by 10%
review_frequency = annual
```

At each review, record the provisional next-year withdrawal after the chosen inflation treatment and the total portfolio value on the defined valuation date. Compute `current_rate = provisional_withdrawal / current_portfolio_value`. Apply the 10% cut if the rate is above the upper trigger; apply the 10% raise if it is at or below the lower trigger; otherwise leave the guardrail adjustment at zero. With the example’s 4.7% start rate, the trigger levels are 5.64% and 3.76%. For a $47,000 starting draw, a single 10% change is $4,700, so the resulting target is $42,300 after a cut or $51,700 after a raise, before later inflation adjustments.

This is a reproducible interpretation of the video, not proof of the speaker’s proprietary client rule. Before coding a production policy, resolve and expose these choices in the settings and annual report:

1. Does `current_portfolio_value` include every investable account, and is it valued before or after that year’s distribution?
2. Is the trigger rate evaluated before the guardrail action? The suggested implementation above says yes.
3. How does an inflation raise interact with the guardrails? Apply it in a fixed order, show both pre- and post-adjustment amounts, and test alternatives.
4. What precisely ends a “temporary” cut? The video does not define a recovery condition. A conservative software default is to leave a cut active until a separately configured recovery trigger is met, and to show the restoration as a proposed change for human review. A threshold such as returning below `r0` is a design assumption that must be labeled and stress-tested.
5. Are 10% changes calculated from the current planned draw, and can multiple cuts or raises occur in consecutive annual reviews? Record each event; never hide compounding or turn a one-time action into an indefinite rule.
6. Which essential-spending floor is protected, and which discretionary categories absorb a cut? The video’s blanket 10% does not define a household floor. If a cut would cross the entered floor, stop and flag the shortfall rather than silently violate it.

There is published primary research on related Guyton–Klinger decision rules, including a 20%-above-initial withdrawal-rate capital-preservation trigger and a 10% reduction, and a 20%-below trigger with a 10% prosperity increase. That paper’s rules also include details and interactions that differ from the video’s deliberately simplified presentation; it reports trade-offs in purchasing power as well as portfolio outcomes ([Guyton & Klinger, Financial Planning Association Journal, 2006](https://www.financialplanningassociation.org/sites/default/files/2021-11/2006%20-%20Guyton%20and%20Klinger%20-%20Decision%20Rules%20and%20SWR%20%281%29.PDF)). Implementing the published strategy requires its full rule set and assumptions, not just copying the two percentages.

### 4. Add account, tax, and audit checks

Model account-level withdrawals separately from the household’s spending target. Required minimum distributions are legally determined per applicable account and tax rules, not by the bucket label: IRS Publication 590-B describes the preceding year-end IRA balance and applicable life-expectancy denominator used in its calculation ([IRS Publication 590-B](https://www.irs.gov/publications/p590b)). A planner should show an RMD separately from discretionary spending and flag where the distribution exceeds the planned draw; do not assume every account type follows IRA rules.

For each annual calculation, save the input snapshot, data sources, valuation date, rule version, formula outputs, bucket transfers, guardrail decision, and any human override with reason. The engine should be deterministic: the same dated inputs and policy version must produce the same result. Keep investment orders out of scope; the software can create a reviewable withdrawal/rebalance suggestion, while the account holder or adviser decides whether to transact.

### 5. Test the rule before relying on it

Run at least:

- rolling historical retirement start dates, preserving each year’s return order;
- 30-, 35-, and 40-year horizons;
- adverse early-return and high-inflation paths, plus favorable periods;
- the baseline fixed-inflation withdrawal rule, the guardrail-only rule, and alternative cash/bridge sizes;
- fees, taxes, Social Security start dates, RMD years, essential-spending floor, and the explicitly chosen cut-recovery rule.

Report portfolio depletion before horizon, essential-spending shortfalls, real spending delivered, number and size of cuts/raises, time spent below the starting real income, and ending balance in inflation-adjusted dollars. Do not report a single “success” percentage without the horizon, asset mix, fees, inflation, tax treatment, and success definition. The FPA study itself defines success relative to the chosen horizon and remaining portfolio and discusses purchasing-power trade-offs ([Guyton & Klinger paper](https://www.financialplanningassociation.org/sites/default/files/2021-11/2006%20-%20Guyton%20and%20Klinger%20-%20Decision%20Rules%20and%20SWR%20%281%29.PDF)). Backtests and simulations are scenario analysis, not guarantees.

## Primary references

- [Original video and description](https://www.youtube.com/watch?v=IHm81JJyZX8) — the speaker’s example, model setup, and stated limits.
- [Guyton & Klinger, “Decision Rules and Maximum Initial Withdrawal Rates” (2006)](https://www.financialplanningassociation.org/sites/default/files/2021-11/2006%20-%20Guyton%20and%20Klinger%20-%20Decision%20Rules%20and%20SWR%20%281%29.PDF) — original research on withdrawal decision rules and tested trade-offs.
- [SEC Investor.gov: asset allocation, diversification, and rebalancing](https://www.investor.gov/additional-resources/general-resources/publications-research/info-sheets/beginners-guide-asset) — official investor guidance on time horizon, allocation, rebalance approaches, taxes, and costs.
- [TreasuryDirect: Treasury bills](https://www.treasurydirect.gov/marketable-securities/treasury-bills/) and [TIPS](https://www.treasurydirect.gov/marketable-securities/tips/) — official maturity, cash-flow, and inflation-adjustment mechanics.
- [FDIC: deposit insurance](https://www.fdic.gov/resources/deposit-insurance/brochures/insured-deposits) — official deposit products and ownership-category coverage limits.
- [SSA: personalized retirement estimates](https://www.ssa.gov/prepare/get-benefits-estimate) — official earnings-based estimates and claim-date choices.
- [BLS: CPI-U calculator](https://www.bls.gov/data/inflation_calculator.htm) — official series/method description for the calculator.
- [IRS Publication 590-B](https://www.irs.gov/publications/p590b) — official IRA distribution and RMD rules.
