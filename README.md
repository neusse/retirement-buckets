<p align="center">
  <img src="docs/assets/retirement-buckets-banner.svg" alt="Retirement Buckets banner" width="100%">
</p>

<h1 align="center">Retirement Buckets</h1>

<p align="center"><strong>Turn retirement withdrawals into an explicit, testable system for liquidity, growth, and adaptive spending.</strong></p>

<p align="center">
  <img alt="Documentation" src="https://img.shields.io/badge/type-research%20%2B%20implementation-1f6feb">
  <img alt="Method" src="https://img.shields.io/badge/method-buckets%20%2B%20guardrails-20a779">
  <img alt="Status" src="https://img.shields.io/badge/status-reference%20playbook-f0b429">
</p>

<p align="center">
  <a href="docs/summary.md">Executive summary</a> ·
  <a href="docs/timestamped-notes.md">Timestamped video notes</a> ·
  <a href="docs/research-sources.md">Primary-source research</a> ·
  <a href="https://www.youtube.com/watch?v=IHm81JJyZX8">Original video</a>
</p>

---

## Implementation guide

This guide translates the video's concept into a deterministic, auditable planning process. It does not recommend a withdrawal rate, security, annuity, or transaction. A fiduciary financial planner and tax professional should review a real household implementation.

## 1. Define the planning contract

Before allocating money, write a one-page withdrawal policy containing:

- household members, ages, beneficiaries, and planning horizon;
- essential and discretionary annual spending, separately;
- known large expenses and emergency reserves;
- reliable income by start date, inflation treatment, and survivor treatment;
- investable accounts, ownership, tax type, current balance, and cost basis where relevant;
- target asset allocation, permitted ranges, fees, and liquidity constraints;
- the annual review date and portfolio valuation convention;
- the spending floor, spending ceiling, and maximum tolerable annual cut;
- guardrail thresholds, action size, recovery rule, and override authority.

The policy should distinguish three quantities:

```text
gross household cash need
− reliable after-tax income
= portfolio-funded after-tax gap

portfolio-funded after-tax gap
+ estimated tax caused by distributions
= gross portfolio withdrawal
```

Use the **gross portfolio withdrawal** in the withdrawal-rate calculation. Recalculate it when Social Security, pension income, survivor status, taxes, or recurring expenses change.

## 2. Establish the starting rate

Let:

```text
P0 = investable assets available to support the plan
W0 = first-year gross portfolio withdrawal
r0 = W0 / P0
```

The video uses `P0 = $1,000,000`, `W0 = $47,000`, and `r0 = 4.7%`. Treat these as demonstration inputs. A household's sustainable rate depends on horizon, allocation, flexibility, income, taxes, costs, and goals.

## 3. Size the buckets

### Bucket 1 — operating cash

Purpose: fund monthly transfers without a forced sale at the time each bill is due.

Suggested design range for testing, not a recommendation:

```text
B1 target = 12–24 months × current gross portfolio withdrawal
            + known near-term one-time expenses
```

Possible instruments include appropriately insured bank deposits, Treasury bills, or a suitable government money-market vehicle. Check actual insurance coverage, settlement rules, liquidity, yield, and tax treatment.

### Bucket 2 — stable bridge

Purpose: cover several additional years if growth assets are depressed.

The video uses about five years of its initial need. A practical implementation might test three to five years, using a maturity ladder of high-quality bonds or Treasuries and, where suitable, TIPS. Define duration, credit quality, reinvestment rules, and maturity dates. “Stable” does not mean price-guaranteed before maturity.

```text
B2 target = bridge years × projected annual gross withdrawals
```

For better precision, use year-specific projected gaps rather than multiplying today's gap, especially when Social Security, pensions, or RMDs begin during the bridge period.

### Bucket 3 — long-term growth

Purpose: provide long-horizon real growth and eventually replenish shorter buckets.

```text
B3 = P0 − B1 − B2
```

Choose its diversification and risk level from the household's horizon and ability to tolerate both market loss and spending cuts. Check the **whole portfolio** allocation after assigning buckets; labels should not conceal an accidental equity, duration, sector, or cash concentration.

## 4. Adopt mechanical cash-flow rules

Use a written order of operations:

1. Transfer the monthly retirement paycheck from B1.
2. Direct portfolio income and scheduled B2 maturities to B1 unless tax planning dictates otherwise.
3. At the annual review, calculate the next year's spending before moving assets.
4. Restore B1 to its target from B2 maturities and portfolio cash flows.
5. Refill B2 from B3 only under an objective rebalance rule—for example, when equities exceed their target allocation or upper band after market gains.
6. If growth assets are below their permitted range, use the prefunded bridge instead of selling merely to make every bucket look full.
7. Select the actual account source tax-aware: taxable, tax-deferred, and Roth withdrawals can have different consequences.

A bucket is not a guarantee against loss. The policy buys time; it does not make the total portfolio immune to inflation, interest-rate changes, credit events, or prolonged equity declines.

## 5. Implement the guardrail engine

### Video-faithful simplified thresholds

```text
upper trigger = 1.20 × r0
lower trigger = 0.80 × r0
cut action   = −10% of the defined spending base
raise action = +10% of the defined spending base
review       = once per year
```

For `r0 = 4.7%`:

```text
upper trigger = 5.64%
lower trigger = 3.76%
```

### Recommended calculation order

On the same date every year:

1. Record current investable assets `P` before the coming year's planned distribution.
2. Update income, expenses, taxes, fees, and known one-time items.
3. Calculate `W_base`, the provisional gross withdrawal after the normal inflation update but before the guardrail action.
4. Calculate `r_current = W_base / P`.
5. Apply exactly one decision:

```text
if r_current > 1.20 × r0:
    W_next = max(essential_floor, 0.90 × W_base)
    action = "capital-preservation cut"
elif r_current < 0.80 × r0:
    W_next = min(spending_ceiling, 1.10 × W_base)
    action = "prosperity increase"
else:
    W_next = W_base
    action = "no guardrail change"
```

Prefer applying cuts to discretionary spending. A blind 10% cut to total spending may violate housing, health, insurance, or food needs.

### Resolve the video's unspecified choices

The production policy must explicitly answer:

- Are thresholds anchored forever to the original `r0`, or reset only after a formal plan revision?
- Is the 10% action applied to the total withdrawal, discretionary spending, or the prior year's lifestyle budget?
- Does the inflation update happen before or after a guardrail action?
- Can actions occur in consecutive years?
- How is a temporary cut restored?
- Is an increase permanent, and can it later be reversed?
- What happens if the calculated cut breaches the essential floor?

A transparent default is to keep thresholds anchored to the original `r0`, apply inflation before testing, allow at most one action per annual review, and require an explicit restoration rule. Do not automatically restore a cut merely because one positive year occurs. Any different behavior is acceptable only if documented and tested.

The percentages resemble the Guyton–Klinger capital-preservation and prosperity rules, but their published method includes additional rules and assumptions. A two-trigger implementation should be labeled a simplified adaptation, not the complete Guyton–Klinger method.

## 6. Keep RMDs and spending separate

An RMD is an account distribution requirement, not a requirement to consume the money.

For each account:

1. calculate the applicable RMD under current IRS rules;
2. identify how much of the planned household withdrawal it satisfies;
3. apply withholding and tax estimates;
4. if the RMD exceeds spending, record the excess as a taxable distribution that may be saved or reinvested in a taxable account;
5. never add the same dollars once as an RMD and again as spending.

Current law and account rules can change. Confirm age, account type, aggregation, beneficiary, and first-year timing with official IRS guidance or a tax professional.

## 7. Use an annual operating checklist

### Before the review

- Reconcile every account balance and prior-year withdrawal.
- Update Social Security, pension, annuity, and other income.
- Update essential and discretionary budgets.
- Estimate taxes, RMDs, health premiums, fees, and large planned costs.
- Record inflation using the policy's chosen index and period.

### During the review

- Compute `W_base`, `P`, and `r_current`.
- Apply and document the guardrail result.
- Check the essential floor and spending ceiling.
- Recalculate B1/B2 targets from the new cash-flow path.
- Rebalance only under the written allocation rule.
- Build a tax-aware distribution and withholding schedule.

### After the review

- Save the dated inputs, formulas, balances, action, and rationale.
- Schedule monthly transfers and maturity dates.
- Record any human override and why it was necessary.
- Trigger an off-cycle review after death, disability, long-term-care need, major tax-law change, large one-time expense, or material change in guaranteed income.

## 8. Store an auditable decision log

One row per planning year is sufficient:

```text
year,valuation_date,start_assets,inflation,
essential_spending,discretionary_spending,
social_security,pension,other_income,estimated_tax,fees,
W_base,r0,r_current,upper_trigger,lower_trigger,
guardrail_action,W_next,RMD,actual_spending,
B1_end,B2_end,B3_end,end_assets,override_reason
```

Version the policy separately. Identical inputs and policy versions should reproduce the same output.

## 9. Stress-test before using real money

Compare at least:

- fixed inflation-adjusted withdrawals;
- guardrails without buckets;
- buckets without guardrails;
- the combined policy;
- several cash and bridge durations;
- 30-, 35-, and 40-year horizons;
- adverse early returns, persistent inflation, low bond returns, and long flat markets;
- actual fees, tax estimates, RMD timing, and major health or home costs.

Track more than “success rate”:

- portfolio depletion and date;
- years below the essential floor;
- minimum income delivered as a percentage of target;
- number, size, and duration of cuts;
- number of increases;
- real lifetime spending;
- real ending assets;
- cumulative taxes and fees;
- liquidity shortfalls and forced sales.

Rolling historical periods overlap, while Monte Carlo results depend heavily on distribution and correlation assumptions. Use both as scenario tools, not predictions.

## 10. Stage the rollout

1. **Shadow phase:** run the calculation without moving assets for three to six months; reconcile it against actual cash flow.
2. **Policy phase:** finalize the written withdrawal, guardrail, refill, rebalancing, and exception rules.
3. **Account map:** assign each planned cash flow to a specific account and tax treatment.
4. **Transition:** move toward the target allocation carefully, accounting for gains, surrender terms, trading costs, and insurance limits.
5. **Operate:** use monthly cash transfers and one scheduled annual decision review.
6. **Validate:** compare actual spending, taxes, returns, and balances to the plan; update assumptions without rewriting history.

## Worked example using the video's numbers

This example demonstrates the formulas only.

```text
P0 = $1,000,000
W0 = $47,000
r0 = 4.70%
upper = 5.64%
lower = 3.76%

B1 = $94,000
B2 = $235,000
B3 = $671,000
```

Suppose the next review produces `W_base = $49,000` and current assets of `$850,000`:

```text
r_current = $49,000 / $850,000 = 5.76%
```

Because 5.76% exceeds 5.64%, the simplified rule proposes:

```text
W_next = $49,000 × 90% = $44,100
```

The plan must then test `$44,100` against the household's essential floor and recompute its bucket targets. It must not assume that the cut alone makes the plan safe.

If instead assets were `$1,350,000`:

```text
r_current = $49,000 / $1,350,000 = 3.63%
W_next = $49,000 × 110% = $53,900
```

The spending ceiling and tax consequences still apply.

## Source trail

- [Original video](https://www.youtube.com/watch?v=IHm81JJyZX8)
- [Detailed primary-source research notes](docs/research-sources.md)
- [Guyton & Klinger decision-rules paper](https://www.financialplanningassociation.org/sites/default/files/2021-11/2006%20-%20Guyton%20and%20Klinger%20-%20Decision%20Rules%20and%20SWR%20%281%29.PDF)
- [Vanguard's Principles for Retirement Income](https://corporate.vanguard.com/content/dam/corp/research/pdf/vanguard_principles_retirement_income.pdf)
- [IRS required minimum distribution FAQs](https://www.irs.gov/retirement-plans/retirement-plan-and-ira-required-minimum-distributions-faqs)
- [Investor.gov asset allocation guidance](https://www.investor.gov/introduction-investing/getting-started/asset-allocation)
- [TreasuryDirect TIPS](https://www.treasurydirect.gov/marketable-securities/tips/)
- [Investor.gov annuity guidance](https://www.investor.gov/introduction-investing/investing-basics/investment-products/annuities)
