[January 2019]: https://www.info.gov.hk/gia/general/201902/28/P2019022700606.htm
[November 2023]: https://www.scmp.com/news/hong-kong/hong-kong-economy/article/3246714/hong-kong-deficit-balloons-hk1641-billion-first-8-months-financial-year
[September 1998]: https://www.info.gov.hk/gia/general/199810/31/1031041.htm
[DownThemAll]: https://chromewebstore.google.com/detail/downthemall/nljkibfhlpcnanjgbnlnbjecgicbjkge
[DATA.GOV.HK]: https://data.gov.hk/en-data/dataset/hk-try-trymthfinr-press-release-financial-results
[archive]: https://www.try.gov.hk/internet/eharch_acco_monfinancial.html
---
# Scraping Hong Kong's Monthly Financial Results
## Overview
Amid Hong Kong's slower-than-expected post-pandemic economic recovery, its deficit has become a major concern for society. Its fiscal reserves peaked at HK$1,211.0 billion in [January 2019]. However, due to consistent deficits every financial year since 2019 (except in 2021-22), its fiscal reserves have declined to HK$670.7 billion as of [November 2023].

How bad is this decline? How should the deficit be addressed? Answering these questions would require us to put recent five years' deficit into perspective. Unfortunately, it seems that there is now no public and easy-to-access datasets or visualisations about Hong Kong's financial results in the long term.

To inform the public debate on Hong Kong's fiscal policy, this project creates and visualises a dataset including Hong Kong's fiscal reserves, revenue, and expenditure each month since [September 1998], when the government started to release monthly summaries of its financial results to meet IMF standards. The dataset, spanning from 1998 to the present, can be found in the file "Monthly Financial Results 1998-present.xlsx". All numbers are in HK$ million.

## Sources
The Hong Kong government regularly publishes the previous month's financial results at the end of each month. The latest two financial years' press releases can be found [here](https://www.try.gov.hk/internet/ehpubl_acco_monfinancial.html), and the rest can be found [here](https://www.try.gov.hk/internet/eharch_acco_monfinancial.html).

While [DATA.GOV.HK] allows the public to download historical datasets related to the monthly financial results, the earliest financial year available is 2019-20. You can find all five fiancial years available for download in the folder "data_gov_hk_csv".

Data from September 1998 to March 2019 are only available in the monthly press releases. They typically include a "Consolidated Account" table, providing that month's revenue, expenditure, and surplus or deficit, and a "Fiscal Reserves" table, providing the size of fiscal reserves at the start and end of the month. Their html files are downloaded from the [archive] using [DownThemAll] and stored in the folder "financial_results_htmls". After that, the major work of this project is to scrape relevant data from the html files.

## Scraping method
"extraction.ipynb" contains the code for extracting relevant data from the html files. Although all press releases include two tables, the presentation of financial data has varied over time. Earliers years used tags such as 'br', 'pre', and 'p' for line breaks and inserted multiple spaces between words and numbers for alignment. It's only in recent years that HTML tables have been used.  To address these variations, the extraction functions are somewhat complex, and some manual insertions are necessary for unique formats.

## Results
Running "extraction.ipynb" produces "extraction_result.csv", which contains data from September 1998 to March 2019:
* file_name
* month_ended
* revenue
* expenditure
* surplus_deficit
* fiscal_reserve_start
* fiscal_reserve_end
* proceed_repayment

"Monthly Financial Results 1998-present.xlsx" is compiled by merging "extraction_result.csv" with the csv files in "data_gov_hk_csvs" using Excel. Columns J, K, and L perform the following validations:
* Column J checks if surplus_deficit matches the differece between revenue and expenditure.
* Column K checks if the fiscal reserves at end of period equals the fiscal reserves at start of period plus the changes that month.
* Column L checks if the fiscal reserves at start of period equals the fiscal reserves at end of the previous period.

There is no problem with column J and K. Column L identified some minor discrepancies, which are explained in the footnotes of the press releases. These validations confirm the accuracy of the extraction code.

## Free use
All the data and code in this repository are available for public use. Feel free to utilise these resources in your work.