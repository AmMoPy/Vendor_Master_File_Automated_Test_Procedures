# VMF_ATP

## About

Vendor Master File Automated Test Procedures (VMF_ATP) is a script intended to automate some test procedures and highlight irregularities in the underlying dataset to guide sampling and further analysis of these irregularities.

Among the several tests performed is identification of similarities among different unrelated records using [n_gram](https://en.wikipedia.org/wiki/N-gram) and [tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) statistical measure, you'll be prompted to specify two parameters being n_gram and n_matches to be used in analysing the data and producing related results. It's advisable to use the default parameters being 3 and 10 for n_gram and n_match respectively, as they produce the best match results. However, feel free to tinker based on how your data is structured.

What's an n_gram? For sake of simplicity, it's a sequence of N words. Example: for the word (McDonald's) an n_gram of 1 would be 'm', 'c', 'd'. n_gram of 2 would be 'mc', 'cd', 'do'. n_gram of 3 would be 'mcd', 'cdo', 'don' and so on. Comparing (McDonald's) and (McDnld's) using this script will first generate an n_gram for each word then compute how closely they match.

What about n_match? It's the number of possible matches you desire for a single word/record. You may be looking for only the first highest match or more than one possible matches, so adjust the number accordingly.

The main module used for matching records is [tfidf matcher](https://github.com/LouisTsiattalou/tfidf_matcher), it produced acceptable results within decent execution time that fits the nature of this script.

## How it works

The following are fundamental data sources for script execution:

- Vendor Master File
- Active employee list
- Terminated employee list
- System access rights for vendor record modifications
- Purchase Order detailed Analysis

Data is loaded in a CSV format, code is applied and all results are saved in one excel workbook each in separate sheet.

Loading data in a CSV format is critical for fast execution time. You can either:

- Use your own CSV files, but make sure to keep columns structure as is (don't change any header unless you are going to edit the related code) and that they are encoded as utf-16.
- Copy and paste your data to the macro enabled excel sheet that I've provided (VMF.xlsm) then save it using ctrl + shift + s to activate the macro which will export each sheet to a utf-16 CSV format.

I've also provided Notebook version for convenience. However, working with Python script is more fun and interactive! A typical work flow using the macro enabled excel sheet would be as follows:

- Copy & paste your data to each sheet respectively, don't change headers!
- Press ctrl + shift + s to activate the macro and export all sheets to CSV files.
- Place the script and CSV files in same folder.
- Run the script, select preferred parameters.
- Enjoy the results.

## Output

A set of detailed and summary tables are produced as follows:

- Vendor records exact and fuzzy name matching ---> first identifying possible name matches, then calculating similarities between each name & sorting results in descending order.
- Active employees vs. vendor records exact and fuzzy name matching ---> same procedures above applied to active employee names and vendor names.
- Terminated employees vs. vendor records exact and fuzzy name matching ---> same procedures above applied to terminated employee names and vendor names.
- Filtering out non-English names.
- Identifying all POs issued to employees ---> either active or terminated, both exact and fuzzy name matches are considered.
- Identifying unauthorized record manipulation ---> comparing edit history of vendor records to the approved access rights and employee records.
- Identifying employees editing their own vendor records ---> both exact and fuzzy name matches are considered results are filtered to the nearest match.
- Identifying vendor records manipulation on weekend and/or at abnormal working hours.
- Identifying POs issued to inactive vendors.
- Identifying gaps in vendor ID and PO numbers.
- Identifying similarities across all vendor data ---> similarity across all vendor data (phone, address, tin, etc..) for highest possible name matches only (above 60%).
- Identifying similarities across all active employees vs. vendor data ---> same procedures above applied to active employees and vendor data (phone, address, tin, etc...).
- Identifying similarities across all terminated employees vs. vendor data ---> same procedures above applied to terminated employees and vendor data (phone, address, tin, etc...).
- Identifying POs issued to terminated employees ---> only exact name matches are considered
- Summarizing missing vendor details.
- Summarizing details of POs issued to inactive vendors.
- Summarizing weekend manipulations.
- Summarizing abnormal working hours manipulations.
- Summarizing vendor records manipulations by period.
- Summarizing similarities across all vendor data.
- Summarizing similarities across all active employees vs. vendor data.
- Summarizing similarities across all terminated employees vs. vendor data.

## Challenges

- Duplicate vendor where both full name and abbreviation exist as an independent record may not be highlighted as a possible match, for example 'PwC' and 'PricewaterhouseCoopers', "P&G' and 'Procter and Gamble'. In such case, concatenate the abbreviation to the full name before loading data; this will ensure that a match will be spotted, however, with low similarity score. There is a specific example for such case in this mock data for vendor 'SKK' and 'Strosin, K and H (SKK)'.

- User ID may not be the same as Employee ID, thus access rights test results will be affected. In such case, unify both IDs by mapping each to a unique ID before loading data.  
- You may not have all the data required in each table, in terms of the data itself not just some missing values. In such case, use mock data and just ignore the related results to avoid any errors while running the script.

## References

- Some other great modules for string matching and deduplication [string grouper](https://github.com/Bergvca/string_grouper) and [pandas dedupe](https://github.com/Lyonk71/pandas-dedupe).

- My preferred mock data sources [Cagy](https://www.cagy.org/test-data-generator/?) and [Mockaroo](https://www.mockaroo.com/)
