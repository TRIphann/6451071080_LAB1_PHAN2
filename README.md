# 6451071080 - LAB 1 - PART 2

Data Mining practical lab, Part 2: association rule mining with the Apriori and FP-Growth algorithms.

Student: Phan Cong Tri - 6451071080

## Contents

| Folder | Dataset | Notebooks |
| --- | --- | --- |
| `6451071080_LAB1_Bai2` | `Online Retail.xlsx` | 6 notebooks |
| `6451071080_LAB1_Bai5a` | `Online Retail.xlsx` | 6 notebooks |
| `6451071080_LAB1_Bai6` | `groceries.csv` | 6 notebooks |

## Exercise 2 - Online Retail, United Kingdom

The notebooks clean the raw workbook first: column names are stripped, `InvoiceNo` is cast to string, and cancelled invoices (identifiers containing `C`) are removed. Data is then pivoted so each row is one invoice and each column is one product, and quantities are converted to a boolean one-hot matrix.

| Question | Task |
| --- | --- |
| Cau 1 | Load `Online Retail.xlsx` and inspect the structure |
| Cau 2 | Strip column names and inspect data types |
| Cau 3 | Cast `InvoiceNo` to string and remove cancelled invoices |
| Cau 4 | Count records per country and plot the distribution |
| Cau 5 | Restrict the data to the United Kingdom and pivot it into a basket matrix |
| Cau 6 | Run Apriori on the United Kingdom basket, generate association rules and plot support against confidence |

## Exercise 5a - Online Retail, Germany and France

Same pipeline as Exercise 2 with a different country and different thresholds.

| Question | Task |
| --- | --- |
| Cau 1 - Cau 4 | Load, clean and filter the workbook |
| Cau 5 | Germany subset, `min_sup = 0.05`, `min_conf = 0.50` |
| Cau 6 | France subset, `min_sup = 0.07`, `min_conf = 0.70` |

## Exercise 6 - Groceries dataset

`groceries.csv` holds 9835 transactions over 169 distinct items, one transaction per line with comma-separated items.

| Question | Task |
| --- | --- |
| Cau 1 | Read the transactions and print the total count with a few examples |
| Cau 2 | Encode the transactions into a one-hot matrix with `TransactionEncoder` and inspect its density |
| Cau 3 | Remove empty transactions and describe the number of items per transaction |
| Cau 4 | Count item frequencies and plot the 20 most frequent items |
| Cau 5 | Apriori over `min_sup` from 0.01% to 0.1%, recording the runtime for each threshold |
| Cau 6 | FP-Growth over the same thresholds, then plot a runtime comparison against Apriori |

### Note on `max_len` in Cau 5 and Cau 6

At `min_sup = 0.01%` a frequent itemset needs only a single transaction to qualify. With 169 items unrestricted itemset growth explodes combinatorially, and both implementations run out of memory on a normal machine.

Both notebooks therefore cap itemset size with `max_len=2`, so the search covers single items and item pairs. This keeps all ten thresholds within reach (about 0.25 s for Apriori and 0.6 s for FP-Growth) and still yields 9805 frequent itemsets at the lowest threshold, which is enough to compare the two algorithms. Item pairs are also the usual target when mining retail baskets.

## Layout

```
6451071080_Lab1_PHAN2/
|-- README.md
|-- requirements.txt
|-- .gitignore
|-- 6451071080_LAB1_Bai2/
|   |-- 6451071080_LAB1_Bai2_Cau1.ipynb ... Cau6.ipynb
|   `-- Online Retail.xlsx
|-- 6451071080_LAB1_Bai5a/
|   |-- 6451071080_LAB1_Bai5a_Cau1.ipynb ... Cau6.ipynb
|   `-- Online Retail.xlsx
`-- 6451071080_LAB1_Bai6/
    |-- 6451071080_LAB1_Bai6_Cau1.ipynb ... Cau6.ipynb
    `-- groceries.csv
```

## How to run

Every notebook reads its dataset by a relative path, so start Jupyter from the folder that contains the notebook.

```bash
pip install -r requirements.txt
jupyter notebook
```

Open the question notebook you want to run, for example `6451071080_LAB1_Bai6/6451071080_LAB1_Bai6_Cau6.ipynb`, and run all cells.

`Online Retail.xlsx` is roughly 23 MB and is stored in both `6451071080_LAB1_Bai2` and `6451071080_LAB1_Bai5a`, because each notebook loads the workbook from its own folder. The two copies are byte-identical.

## Dependencies

Python 3.14 with the packages listed in `requirements.txt`:

- pandas 3.1.0
- numpy 2.5.4
- matplotlib 3.11.2
- seaborn 0.13.2
- mlxtend 0.25.0
- openpyxl 3.1.5 (required to read the `.xlsx` workbook)
