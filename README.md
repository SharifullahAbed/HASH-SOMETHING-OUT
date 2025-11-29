# Hash Something Out – Movie Hash Tables

This project implements a simple separate-chaining hash table for strings and tests
five different hash functions on a dataset of 15,000 movie titles and quotes.

Dataset file used in Colab:
- `MOCK_DATA (1).csv` (15,000 rows, columns: `movie_title`, `quote`)

## Hash Functions Implemented

1. **poly31** – classic polynomial rolling hash with base 31  
2. **djb2** – Dan Bernstein’s hash (h = 5381, h = h * 33 + c)  
3. **fnv1a** – 64-bit FNV-1a hash  
4. **sum_squares** – simple sum of squared character codes plus length mixing  
5. **two_stage** – preprocess (lowercase/strip) then polynomial hash with base 53 and XOR mixing

All hashes are used with:
- separate chaining
- table size = **1009**
- keys = movie titles and movie quotes

## How to Run (Colab)

1. Upload `MOCK_DATA (1).csv` to the Colab Files panel.
2. Run the `Hash.ipynb` notebook.
3. The notebook:
   - loads the CSV
   - builds two hash tables (title table and quote table)
   - prints statistics for each of the five hash functions.

## Results (Summary)

For all five hash functions with table size 1009 and 15,000 records:
- Items inserted: 15,000
- Collisions: 13,991
- Used buckets: 1009
- Wasted buckets (empty slots): 0

The main differences are:
- **Build time**
- **Longest chain length** in the separate-chaining lists

(Details and discussion are below.)

## Discussion

Using a table size of 1009 and 15,000 movie records, all five hash functions produced the same number of items and collisions (13,991) with zero completely empty buckets, so the main differences are in runtime and longest chain length. The `fnv1a` hash was clearly the worst in terms of performance, taking about 4.22 seconds to build the tables and producing the longest chain (up to 40 elements). The fastest function by far was `sum_squares` (≈0.34 seconds) with a longest chain of 33 in the title table, which is similar to `poly31` and `two_stage`. The best distribution of chains appears in `djb2` and `two_stage`: `djb2` has the shortest longest chain in the title table (32), while `two_stage` has the shortest longest chain in the quote table (28). Overall, I would prefer `sum_squares` if I care most about speed, and `djb2` / `two_stage` if I care more about slightly shorter chains and more even distribution.


Optimization Summary (Final Comparison)

Across the five hash functions tested on the 15,000-record dataset, all functions produced the same number of inserted items and used all 1009 buckets. Collisions were also identical because the dataset is much larger than the table size, so heavy chaining was expected.

1. The performance differences showed up mainly in build time and longest-chain length.
2. sum_squares was the fastest overall, finishing in about 0.34 seconds.
3. poly31 and djb2 performed moderately well, around 0.7–0.8 seconds.
4. two_stage was slower due to preprocessing and bit-mixing.
5. fnv1a was the slowest at over 4 seconds, likely because it performs 64-bit multiplication on every character.

In terms of spreading keys across buckets, all functions produced very similar collision counts, but fnv1a created the longest chains, while djb2 and two_stage produced slightly shorter ones. Overall, sum_squares offered the best trade-off between speed and chain length for this dataset.


## Screenshots

Below are the required screenshots from the Colab executions:

1 - poly31 hash results  
2 - djb2 hash results  
3 - fnv1a hash results  
4 - sum_squares hash results  
5 - two_stage hash results  

Screenshots are included in the repository.

