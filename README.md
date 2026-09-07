# CRM Sales Pipeline Analysis — Power BI Dashboard

A 4-page Power BI dashboard analyzing \~8,800 B2B sales opportunities — built to answer the kind of questions a sales leadership team asks before a quarterly business review: how the team is tracking, who's performing, and where deals are falling apart.

## **The Ask**

Leadership needed a live dashboard for a quarterly review that could answer, on the spot:

* How is the team tracking against the pipeline overall?  
* Which reps and regions are leading or lagging?  
* Are certain products or accounts outperforming others?  
* Where are deals getting stuck or falling apart?

  ## **The Data**

Five related tables: a sales pipeline fact table (\~8,800 opportunities), an accounts table (85 client accounts across 15 countries), a products table, a sales team roster (35 agents across 3 regions — Central, East, West), and a data dictionary. *(Source: Maven Analytics CRM Sales Opportunities dataset)*

## **Data Cleaning & Modeling**

* Built a star-schema style model in Power BI with an explicit Date table, using `close_date` as the primary time relationship.  
* **Caught a data-quality bug during QA**: the pipeline data logged one product as `"GTXPro"` while the products table listed it as `"GTX Pro"` — a naming mismatch that silently misattributed ₹35L+ across 729 won deals to a blank/unmatched product category. Fixed at the source with a Power Query `Table.ReplaceValue` step rather than patching around it downstream.  
* Built core KPI measures (`Closed Deals Value`, `Win Opportunities`, `Lost Opportunities`, `Deal Closed Rate`) as **self-contained DAX measures** — each filters to the right deal stage internally via `CALCULATE` \+ `FILTER`, so the numbers stay correct regardless of which page, slicer, or filter state a viewer lands on.  
* Built an **Estimated Loss Value** measure that, for each individual lost deal, looks up the average Won `close_value` for that *specific product*, then sums those per-deal estimates across every lost deal — rather than using one blended average across all products, which would understate high-value products and overstate low-value ones.

  ## **Analysis**

* Overall win rate: **63%** (4,238 Won vs. 2,473 Lost, excluding the 1,589 still-Engaging and 500 Prospecting deals still in motion).  
* West leads by **revenue** (36% of won value) vs. Central (33%) and East (31%) — a modest gap, not dramatic. By **deal count**, though, Central actually wins the most deals (38% vs. West's 34%) despite generating less revenue than West — suggesting Central's average deal size runs smaller than West's.  
* Overall Closed Deals Value: **10.01M** · Estimated Loss Value: **5.94M**  
* Sales Rep with Most Sales: **Darcel Schlecht**  
* Most Valued Product: **GTX Pro**  
* Most Sold Product: **GTX Basic**  
* Most Valued Client: **Kan-code**  
* Highest period of successful sales (Month-wise): **June**  
* Highest period of successful sales (Quarter-wise): **Q2**

  ## **The Dashboard**

Four pages:

1. **Win/Lost Analysis** — win rate, funnel breakdown by deal stage, win/loss trend over time, estimated loss ratio  
2. **Sales Performance Metrics** — pipeline trend (closed value vs. estimated loss), region and rep performance (won vs. lost)  
3. **Product Performance Metrics** — revenue and deal volume by product and account  
4. **Loss Metrics** — estimated loss value and volume by product and account

Show Image Show Image Show Image Show Image

## **Key Insight & Recommendation**

Deals appear to cluster toward quarter-end within the months of data available (only March–December is covered, so this isn't yet confirmed across a full year of complete quarters — worth re-checking once a full 12 months is available). Regionally, West leads on revenue while Central actually wins more deals by count — so no single region is unambiguously "the best," and the more useful read is that Central's average deal size trails West's, which is worth digging into on its own. Separately, one individual stands out: **Darcel Schlecht** accounts for roughly **35% of Central's total won revenue** despite being just 1 of 11 agents in that region. That's a strong enough outlier to be worth studying directly — regardless of how Central's region-wide numbers shake out, understanding what Darcel does differently is a concrete, replicable lever, rather than something to attribute to "the region" as a whole. On product mix: although GTX Basic is the most *sold* product by volume, GTX Pro brings in the most revenue — a classic volume-vs-value split. Kan-code is the most valuable client on both fronts, buying the most products and contributing the most revenue. Hottechi is a close second by number of products purchased but contributes noticeably less revenue — worth investigating whether Hottechi's purchases skew toward lower-value products, and if so, whether there's an opening to introduce them to the premium line (GTX Pro, GTX Plus Pro).

## **Tools**

Power BI Desktop · Power Query · DAX · Tabular Editor (data model / relationship management)

## **Files**

* `CRM_Sales_report.pbix` — the dashboard file  
* `/images` — page exports for anyone viewing without Power BI Desktop
* `/data` — data used for this project
