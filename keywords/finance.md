# Finances and Accounting

See also: [[management]]

#finance #bib

Subtopics:
*  [[pricing]]
*  ERPs: [[sap]]


# Accounting

There appears to be several ways to build a hierarchy of business indicators. One of them (a rather simple one) starts like that:

* Gross Sales = Gross Revenue = what your customers nominally paid you, for this time period
* Net Sales = Net Revenue = Gross Sales – VAT – (discounts + refunds)     
    * What's the deal with time allocation? Is it typicaly Generated vs billed vs payed? 
* Gross profit = Net Sales – Cost of goods sold
* Operating profit = Gross profit – OPEX (operating expenses) – Depreciation and amortization
* EBIT (Earnings before interest and taxes) = Operating profit – Administrative, general and marketing expenses + Non-operating sources of income (but not interest on money, even if it was positive)
* EBT (Earnings before taxes) = EBIT + Interest (this interest may be positive if you lent money to someone and they are paying you, or you have money in the bank, but more often it's negative as you took some loans)
* Net income = Net profit (?) = EBT – Taxes

There are also some dead-end definitions that don't follow this path:
* EBITDA (Earnings before interest, taxes, depreciation and amortization) = Gross Profit – OPEX –Marketing, general, and administrative expenses = EBIT + Depreciaton and Amortization. Essentially (it seems that) "Operating profit" and EBITDA form a fork that later coverges again. With Operating Profit you include relevant equipment costs, but don't include overhead yet (similar to CM2; see below). With EBITDA, on the other hand, you include all running costs at first (even those that are really not operations-specific), and also then come to equipment.

A fancier approach replaces the sequence from Revenue to EBIT with a battery of contribution margins:
* CM1 (Contribution Margin 1) = Net Revenue – variable costs (costs associated with the process of delivering the service)
* CM2 = CM1 – Product fixed costs (the costs you bear regardless of whether you sell anything or not, such as the costs of holding the equipment that you use to deliver the services)
* CM3 = CM2 – Group fixed cost. This last one is only important for huge organizations with a portfolio of very different products, where they want to estimate the financials of each group of products separately.

So the CM1 sequnce kinda removes expenses in a different order. From an accounting point of view it makes sense to remove operation expenses (including all overhead) first, as this is live money (cashflow), and then remove amortizaton and depreciation (as these are paid at some random moment, and then spread across the entire period). But from the per product or per market profability analysis, it makes sense remove the investments in this product or market first, to estimate the marginal effect from potentially getting rid of this market, and only then apply fixed overheads (the organization / techstack).

There are also non-GAAP measures that are sometimes used in performance reporting to investors etc:
* Adjusted EBITDA: something like a reconciliation that tries to model the performance of a company as it "would have been", in the absence of financial effects that supposedly don't characterize the performance of the company, but are kinda extraneous. You calculate it in the opposite direction, not from the cashflow down, but from the official Net income up, only including those taxes, intersts, amortizations and changes in the capital that are relevant for the performance of the company.

# References

https://en.wikipedia.org/wiki/Earnings_before_interest_and_taxes
https://en.wikipedia.org/wiki/Earnings_before_interest,_taxes,_depreciation_and_amortization

Contribution margins:
https://www.intomarkets.com/en/wiki/contribution-margin/

GAAP: basic principles of accounting
https://www.accounting.com/resources/gaap/


