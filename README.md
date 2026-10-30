# Nike Sales Dashboard: Data Summary and Insights

> Review of two Power BI pages for Nike-style sportswear sales. The dashboard reports **3.44M total profit**, **23% loss-making orders**, and an **8% return rate**. The main issue to investigate is net revenue: its reported value is much lower than total profit.

## Contents

- [Dashboard 1: Nike Sales Overview](<img width="890" height="747" alt="Screenshot 2026-10-29 201625" src="https://github.com/user-attachments/assets/052e1222-be37-42df-9e2e-5ea7d176998a" />
)
- [Dashboard 2: Overall Data Preview](<img width="887" height="742" alt="Screenshot 2026-10-29 201608" src="https://github.com/user-attachments/assets/2605a5db-a26b-4309-9346-1c6f9af65110" />
)
- [Data quality issues](#data-quality-issues)


## Dataset snapshot

The cleaned file contains 1,000 orders across 21 columns. The dashboard reports approximately 2,500 orders, so the file appears to be an extract covering `Order_ID` 2000–2999. Several ratios are close between the extract and dashboard: loss-making orders are 23.3% versus 23%, and average discount is 21.2% versus 21%.

| Item | Detail |
|---|---|
| Rows and columns | 1,000 orders, 21 columns |
| Order date range | 4 Aug 2023–6 Dec 2025; 221 orders have no date |
| Products | 20 products across Training, Basketball, Lifestyle, Running, and Soccer |
| Customer segments | Men, Women, Kids |
| Sales channels | Online: 514; Retail: 486 |
| Regions | Bangalore, Delhi, Hyderabad, Kolkata, Mumbai, Pune |
| Profit in extract | 1.34M; 233 loss-making orders (23.3%) |
| Returns in extract | 92 orders (9.2%), identified by negative `Units_Sold` |
| Average discount | 21.2% |
| Added fields | `Profit Flag`, `Discount Flag`, `Revenue Flag`, `Gross Sales`, `Discount Amount`, `Net Revenue Calc`, `Is Return`, `Is Loss` |

Dashboard values below are read from the Power BI pages. Chart readings marked approximate may not match underlying values exactly.

## Dashboard 1: Nike Sales Overview


<img width="890" height="747" alt="Screenshot 2026-10-29 201625" src="https://github.com/user-attachments/assets/19f0c393-a4b3-4ff2-8530-cdc7a9c48802" />


### Summary

The page shows **687.18K net revenue**, **3.44M total profit**, and **1.875K units sold**. It includes revenue by product, profit by gender and region, and the profitable-versus-loss order split. Product and Region slicers filter the charts.

### Insights

- Profit is distributed nearly evenly by gender: Women **1.19M (34.48%)**, Kids **1.13M (32.99%)**, and Men **1.12M (32.53%)**.
- Bangalore leads regional profit at approximately **0.62M**; Hyderabad is lowest at approximately **0.52M**. The six cities perform fairly evenly, with a roughly **0.1M** spread.
- Flex Trainer has the highest revenue at approximately **73K**, followed by LeBron 20 (**59K**) and Zoom Freak (**51K**). Pegasus Turbo is lowest at under **10K**.
- Approximately **1.92K orders (76.88%)** are profitable, while **0.58K (23.12%)** lose money.

### Revenue warning

Net revenue (**687.18K**) is far below total profit (**3.44M**). Profit should not exceed revenue, so the revenue measure likely needs review. In the 1,000-row extract, 932 rows are flagged as having no Revenue. `Net Revenue Calc` may be a better KPI base, but it should be validated against the business definition before replacing the existing measure.

## Dashboard 2: Overall Data Preview
<img width="887" height="742" alt="Screenshot 2026-10-29 201608" src="https://github.com/user-attachments/assets/7b5f38f1-90b0-478f-99b1-2549fbd79d40" />

### Summary

The page tracks **2.5K total orders**, **0.21 average discount**, **0.23 loss order rate**, and **0.08 return rate**. It also shows the five products with the largest losses, the five products with the highest return rates, and a profit trend line.

### Insights

- The dashboard reports a **21% average discount**, **23% loss-making orders**, and **8% returns**.
- The five largest loss-making products total **99,705** in losses across **156 orders**, or about **639 per loss order**.

| Product | Loss | Loss-making orders |
|---|---:|---:|
| Free RN | 21,278 | 28 |
| Air Jordan | 20,835 | 35 |
| Waffle One | 20,519 | 34 |
| Tiempo Legend | 19,274 | 30 |
| Phantom GT | 17,799 | 29 |

- Free RN has the largest total loss despite having the fewest loss-making orders among these five, at approximately **760 loss per order**. Air Jordan has the most loss-making orders.
- Tiempo Legend has the highest return rate at approximately **11.5%**. Kyrie Flytrap and Metcon 7 are around **11%**, Flex Trainer around **10%**, and Waffle One around **9.5%**. These readings are approximate and all exceed the 8% dashboard average.
- Tiempo Legend and Waffle One appear on both the loss and return lists, making them good candidates for review.
- Flex Trainer leads revenue on Dashboard 1 and has an approximately **10% return rate**, so returns may be affecting its realized revenue.
- Profit rises from approximately **0.2M in 2023** to **1.2M in 2024**, then eases to **1.16M in 2025**. Because data starts in August 2023, the first year is partial.

### Trend warning

The three annual points total approximately **2.5M**, below the **3.44M** total profit. The 221 undated orders in the extract may explain some of the gap. Also, the chart title says “month wise,” but its axis displays years; use a Year-Month axis if a monthly trend is intended.

## Data quality issues

Counts in this table refer to the 1,000-row extract.

| Issue | Count in extract | Dashboard impact |
|---|---:|---|
| Revenue is zero | 932 rows; 275 of 333 rows with units sold above zero | Net revenue may be understated |
| MRP is zero | 503 rows | Gross sales cannot be calculated for these orders |
| `Order_Date` is null | 221 rows | Orders are omitted from the dated profit trend |
| Size is `Unavailaible` | 198 rows | Likely typo; size values mix apparel sizes (M, XL) and shoe sizes |
| Repeated `Order_ID` | 53 rows; 947 unique IDs | Order counts may be inflated if duplicates are not handled |
| Discount exceeds 100% | Values reach 125%; 20 orders flagged “in Debt” | Can produce negative net revenue |

## Recommended next steps

1. Validate the revenue calculation and compare the 687.18K KPI with `Net Revenue Calc` and the 3.44M profit figure.
2. Make `Order_ID` unique or explicitly flag and handle duplicate IDs before counting orders.
3. Standardize `Unavailaible` to `Unknown` and review other inconsistent values.
4. Change the trend chart to a Year-Month axis and add an “Unknown date” note or KPI for undated orders.
5. Review Tiempo Legend and Waffle One first because they appear in both the loss and return lists.
