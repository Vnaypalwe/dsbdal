# dsbdal
https://github.com/AaryaAgrawal6105/DSBDA_practical


Here's everything in one go: the problem statement, your code recap, and a complete list of viva questions with model answers.

## 📋 Problem Statement (Q9)

> Perform the following operations using Python on the Facebook metrics data sets:
> a. Create data subsets for type of post
> b. Merge two subsets
> c. Sort Data on Page total likes
> d. Transposing Data
> e. Melting Data to long format
> f. Casting data to wide format

**Dataset:** `dataset_Facebook.csv` — 500 rows, 19 columns, semicolon-separated. Columns include `Page total likes`, `Type` (Photo/Status/Link/Video), `Category`, `Post Month`, `Paid`, engagement metrics (`comment`, `like`, `share`), and lifetime reach/impression metrics.

---

## 💻 Viva Questions & Model Answers

### Section A — Basics about the dataset and pandas

**Q1. What dataset are you using and where does it come from?**
> The Facebook Metrics dataset from the UCI Machine Learning Repository. It contains 500 Facebook posts from a cosmetics brand, with 19 columns recording the post type, category, engagement (likes, comments, shares), and lifetime reach metrics.

**Q2. Why did you use `sep=';'` in `read_csv`?**
> Because this particular CSV file uses semicolons as separators between columns instead of commas. If I leave the default `sep=','`, pandas reads the whole row as one column. The `sep` parameter tells pandas which character to split on.

**Q3. What is pandas? Why pandas and not just plain Python?**
> Pandas is a Python library built on top of NumPy for handling structured/tabular data. Plain Python uses lists and dicts which are slow and have no built-in operations like merge, melt, or pivot. Pandas gives a DataFrame structure with optimized vectorized operations.

**Q4. What is a DataFrame?**
> A 2D labelled data structure with rows and columns, similar to a spreadsheet or an SQL table. Each column can have a different data type.

---

### Section B — Filtering / Subsets

**Q5. Explain `data[data['Type'] == 'Photo']`.**
> The inner part `data['Type'] == 'Photo'` returns a Boolean Series — True for every row where Type is "Photo", False otherwise. The outer `data[ ... ]` uses this Boolean mask to keep only the True rows. This is called Boolean indexing.

**Q6. Why filter at all?**
> The problem statement asks to create subsets by type of post. Filtering lets us separate the dataset into post-type groups (Photo, Status, Link, Video) for separate analysis.

**Q7. How many post types exist in this dataset?**
> Four: Photo, Status, Link, and Video. Photo is the most common.

**Q8. Can you also do this using `.query()` or `.loc`?**
> Yes. `data.query("Type == 'Photo'")` or `data.loc[data['Type'] == 'Photo']` give the same result. I used Boolean indexing because it's the simplest and most common syntax.

---

### Section C — Merging

**Q9. What does `pd.concat([photo_posts, status_posts])` do?**
> Stacks the two DataFrames vertically into one bigger DataFrame. The result has the rows of `photo_posts` followed by the rows of `status_posts`.

**Q10. What is the difference between `concat` and `merge`?**
> `concat` stacks rows or columns together based on position. `merge` is SQL-style joining on a common key column. For combining subsets of the same table, `concat` is the right choice.

**Q11. What is the default `axis` of concat?**
> `axis=0`, which means row-wise concatenation. Setting `axis=1` would concatenate column-wise.

---

### Section D — Sorting

**Q12. Why did you sort by `Page total likes`?**
> Because the problem statement specifies sort by Page total likes. It helps identify which posts come from pages with the largest audience.

**Q13. What does `ascending=False` mean?**
> Largest values first (descending order). `ascending=True` would put smallest first.

**Q14. What if there are ties (same Page total likes)?**
> Pandas preserves the original order of tied rows by default. You can pass `kind='stable'` to guarantee this behaviour.

**Q15. Can you sort by multiple columns?**
> Yes — pass a list: `sort_values(by=['Page total likes', 'like'], ascending=[False, False])`.

---

### Section E — Transposing

**Q16. What does `.T` do?**
> It transposes the DataFrame — rows become columns and columns become rows. So a 500×19 DataFrame becomes 19×500.

**Q17. Why transpose? Is it useful?**
> In real analysis, rarely. Transpose is useful when you want each variable (column) shown as a row — for example, in summary tables or correlation matrices. Here it's done because the problem statement requires it.

**Q18. Is `.T` the same as `transpose()`?**
> Yes, `.T` is just shorthand for the method `.transpose()`.

---

### Section F — Melting

**Q19. What is wide format vs long format?**
> Wide format = each variable in its own column (e.g., separate `like`, `share`, `comment` columns). Long format = one column for the variable name, one column for the value, multiple rows per record.

**Q20. Explain `pd.melt(data, id_vars=['Type','Category'], value_vars=['like','share','comment'])`.**
> It takes the DataFrame and reshapes it: keeps `Type` and `Category` as ID columns (unchanged), then takes the three engagement columns and "unpivots" them into two new columns — `variable` (which holds the name like, share, or comment) and `value` (which holds the number).

**Q21. Why melt? When is long format useful?**
> Long format is needed by tools like seaborn for plotting and SQL-style group-by operations. It also makes the data tidy: each observation becomes one row.

**Q22. What are `id_vars` and `value_vars`?**
> `id_vars` are the identifier columns to keep as-is. `value_vars` are the columns to unpivot into the new variable/value pair.

---

### Section G — Pivot / Cast

**Q23. What does `pivot_table` do?**
> It reshapes long-format data back into wide format. For each unique combination of `index` values, it places `columns` values across the top and fills cells with the corresponding `values`.

**Q24. What is `aggfunc='sum'` doing?**
> When multiple rows share the same `(Type, Category, variable)` combination, pandas needs to know how to combine them. `'sum'` adds the values. Other choices: `'mean'`, `'count'`, `'max'`, `'min'`, `'median'`.

**Q25. Difference between `pivot` and `pivot_table`?**
> `pivot` requires unique combinations and errors if there are duplicates. `pivot_table` handles duplicates by aggregating (sum/mean/etc.), so it's more flexible.

**Q26. Is melt + pivot reversible?**
> Yes — melt converts wide to long, pivot converts long back to wide. If you don't lose data and use the right `aggfunc`, you get back the original DataFrame.

---

### Section H — Printing / Output

**Q27. What does `.head()` do?**
> Returns the first 5 rows of the DataFrame by default. You can pass `.head(10)` for 10 rows, or `.tail()` for the bottom rows.

**Q28. Why do you call `.head()` on every result?**
> To verify the output without printing thousands of rows. It's a quick sanity check.

---

### Section I — Conceptual / "Why" questions

**Q29. What are the 4 main types of post in this dataset?**
> Photo, Status, Link, Video.

**Q30. Why isn't there a "header=None" in your read_csv?**
> Because this file already has a header row (the first row contains column names like `Page total likes`, `Type`, etc.). Files without headers need `header=None`.

**Q31. What would you do if the dataset had missing values?**
> Use `data.dropna()` to remove rows with NaN, or `data.fillna(value)` to fill them with a default. This file does have some blank cells (a few comment/share values are missing) but they don't affect our 6 operations.

**Q32. What does "tidy data" mean?**
> A concept by Hadley Wickham — each variable is a column, each observation is a row, each type of observational unit is a table. Long format produced by melt is "tidy."

**Q33. What is the difference between a Series and a DataFrame?**
> A Series is 1-dimensional (single column). A DataFrame is 2-dimensional (multiple columns). `data['Type']` returns a Series; `data[['Type']]` returns a DataFrame.

**Q34. What's the shape of your original DataFrame?**
> 500 rows × 19 columns. You can check using `data.shape`.

**Q35. Could you have done this without pandas?**
> Yes, using plain Python loops or CSV module — but it would be 10× the code and much slower. Pandas exists precisely to make this easy.

---

### Section J — Likely "trap" questions

**Q36. What happens if you write `data['Type'] = 'Photo'` instead of `data['Type'] == 'Photo'`?**
> Single `=` is assignment — it would overwrite the entire `Type` column with the value "Photo". Double `==` is comparison. This is a common bug.

**Q37. What if you remove `sep=';'`?**
> Pandas would read the whole row as one giant column because it expects commas by default. The dataset would be unusable.

**Q38. What happens if you concat DataFrames with different columns?**
> Pandas fills the missing columns with NaN. Since `photo_posts` and `status_posts` have identical columns, this isn't an issue here.

**Q39. Why does `ascending=False` give big-to-small?**
> "Ascending" means going up. False means we're NOT going up — we're going down. Descending.

**Q40. Is your code memory-efficient for a 1 million row file?**
> For 500 rows, no problem. For millions, you'd use `chunksize` in `read_csv` or switch to a library like Dask or Polars. Pandas loads the full file into RAM.

---

## 🎯 Top 10 most likely viva questions (memorize these first)

1. Why `sep=';'`?
2. Explain `data[data['Type'] == 'Photo']`.
3. Difference between `concat` and `merge`.
4. What does `ascending=False` do?
5. What does `.T` do?
6. Wide vs long format — define both.
7. Explain `id_vars` and `value_vars` in melt.
8. What is `aggfunc` and why is `sum` used?
9. Difference between `pivot` and `pivot_table`.
10. What is the shape of your DataFrame and how do you check it?

If you can answer these 10 cleanly, you're solid. The other 30 are bonus depth.

import matplotlib.pyplot as plt

# Plot pie chart of post types
data['Type'].value_counts().plot(
    kind='pie',
    autopct='%1.1f%%',
    title='Distribution of Post Types'
)
plt.ylabel('')   # removes the default ugly y-label
plt.show()

