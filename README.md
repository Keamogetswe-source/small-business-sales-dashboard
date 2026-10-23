# Small Business Sales Dashboard

A data cleaning and analysis project on a fictional South African small-business dataset: 500 orders, 5 branches and 15 products (January to September 2026). I cleaned a deliberately messy dataset, answered three questions an owner would ask, and built a dashboard in Looker Studio.

> The data is fictional. Data-quality problems were deliberately introduced so I could practise finding and fixing them.

## The business questions

1. Which product sold the most?
2. Which product made the most profit?
3. Which products are slow movers?

## Key findings

- **Totals:** R163,968.80 in sales, R57,756.80 in profit, 500 orders.
- **Best seller:** Rice 5kg is first on sales value (R21,776.40) and on profit (R5,728.40). It is also first on units (236) once two unusually large orders are excluded.
- **Bulk orders change the story.** Two Individual-customer orders (`ORD0291`: 300 Laundry Soap, `ORD0390`: 200 Bath Soap) make those products lead on units. I kept them in the data, flagged them, and gave the dashboard a switch to include or exclude them.
- **Branches:** Soweto is first on sales (R38,459) only because of the 300-unit Laundry Soap order. Without it, Pretoria is first. Tembisa is last either way.
- **Slow movers:** Washing Powder sold the fewest units (105) and Toothpaste has the lowest sales value. Toilet Paper 18s is a strong seller that has not sold since 25 August 2026, which is worth asking the owner about. The dataset has no stock levels, so "dead stock" could not be measured directly.

## Dashboard

Looker Studio: _add the shared link here_

_Add screenshots from the `screenshots/` folder here._

## What I did

1. **Inspected** the messy data before changing anything.
2. **Cleaned** 12 kinds of problem in Google Sheets (duplicates, spelling, blanks, negative quantities, impossible dates, text stored as numbers, wrong profit). Each fix was checked with a formula afterwards. Full details: [docs/data_cleaning_log.md](docs/data_cleaning_log.md).
3. **Analysed** the clean data with pivot tables.
4. **Visualised** the results in Looker Studio.

## Repository structure

```
data/raw/sales_data_messy.csv      messy input (508 rows)
data/clean/sales_data_clean.csv    cleaned output (500 rows)
docs/data_cleaning_log.md          what was wrong, how I found it, how I fixed it
screenshots/                       dashboard images
```

## Tools

Google Sheets (cleaning, pivot tables), Looker Studio (dashboard), Git and GitHub.
