---
layout: post
title:  "Merge two data frames with partial matching"
date: 2015-08-06
categories: 
    - R
    - Data science
    - In English
---

With real-world databases, merging is not as easy as we should learn in classes. For my MD thesis, I'm working on heterogeneous databases, and I'm facing this problem. 

## Problem

I have two databases I have to merge: 

### Database 1

The first database is coming from a prospective study, manually filled. I already did some data management and error checking, but there must always be errors. Now the first columns of this database should be as this :

| First name | Last name | Date of Birth | Gender | Date of entry|(other data)
|------------|-----------|---------------|--------|--------------|---
| Johne       | Down      | 1978-09-02    | M      | 2004-01-05 |  ...
| Maria      | Gonzales  | 1988-01-16    | F      | 2004-06-23 | ...

Notice I have altered the first name of John and the date of birth of Maria.

### Database 2

The second database is coming from the [DRG-based](https://en.wikipedia.org/wiki/Diagnosis-related_group) information system. This database is mostly reliable on the variables I will use to merge (first and last name, gender, date of birth and date of hospitalization). An imaginary example of this database should be:

| First name | Last name | Date of Birth | Gender | Date of entry| GHM | (other data)
|------------|-----------|---------------|--------|-------------|---|---
| John       | Down      | 1978-09-02    | M      | 2004-01-05 | 03C19 | ...
| Maria      | Gonzales  | 1988-01-17    | F      | 2004-06-23 | 03M06 | ...
| Maria      | Gonzales  | 1988-01-17    | F      | 2005-02-05 | 03M06 | ...

### Merge

I have to merge these two databases. There is normally a match by hospitalization. This mean a patient may have several lines.

I can't use an exact merge by these variable because I know there is some alteration. And I need to find a match for each observation of the first database (left merging).

I search a little bit, found [one post about partial matching](http://thebiobucket.blogspot.fr/2012/09/merging-dataframes-by-partly-matching.html
) but nothing perfect for me.

## A solution

I plan to do some partial matching using [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) and the creation of a score of matching. All variables used for the merge will be treated as strings (including dates) because I consider the error come from the manual typing.

The lowest this score is, the closest to the perfect match it is. Then a score of 0 = perfect match.

The algorithm I will follow will be:

1. Define the columns use for matching (first name, last name, date of birth, date of entry).
2. Optionally, take a weight to each of these columns
3. For each row of the Database 1 
    1. For each row of Database 2
        1.  For each selected column use for matching 
            1. Calculate a Levenshtein distance from the corresponding column in Database 2. 
            2. Multiply it by the weight (default value = 1). The greater the weight is, the less tolerant will be the score.
            3. Add it to the pair match score.
        2. Write the pair match score in a score table (see below)
    2. If there is a pair match score = 0 (perfect match), then this is the match. Add the label "perfect match".
    3. Else if find the lowest match score. 
        1. If the score is < to limit, then add to match table with the label "partial match" and the score.
        2. Else, if there is the first match is > to the limit write  no match (
    4. Write the result in a match table (see below)


Example of match scores table:

row_id_db1 | row_id_db2 | col1_score | col2_score | ... | Total score
----------|---------|---------|----|----|----
1 | 1 | 7 | 3 | ... | 20
1 | 2 | 0 | 0 | .... | 0
1 | 3 | 4 | 9 | ... | 18
2 | 1 | 2 | 0 | ... | 2
2 | 1 | 1 | 0 | ... | 1
2 | 1 | 4 | 6 | ... | 12
3 | 1 | 8 | 3 | ... | 20
3 | 2 | 6 | 6 | .... | 16
3 | 3 | 5 | 9 | ... | 15

Example of id merge table


row_id_db1 | row_id_db2 | match_status | match_score
----|----|----|---
1   | 2  | "Perfect match" | 0
2   | 1  | "Partial match" | 1
3   | 3  | "Suspicious match" | 16

# Conclusion

I will implement this solution in _R_ try this solution the next days. I'm sure there is someone else who already think about this problem (because it's very common) then I will also search for already existing solution.
