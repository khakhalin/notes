# Data Cleaning

aka **Data Cleansing**, including **Data Scrubbing**

#tools #db

Parents: [[01_Tools]]
See also: [[data_wh]], [[database]]

**Data cleansing** involves identifying duplicates, data inconsistencies, and then marking them for verification, or fixing them. A catch-all term that may include things like simply ignoring bad data.

**Data scrubbing**: automated data fixing, that includes developing data standards, validation against these standards, attempts of automated correction of errors (e.g. formatting). The term is also used in the context of RAIDs and data rot (when one of the copies gets corrupted, and needs to be identified, and then recovered from another copy)

Approaches to data validation:
* Data type
* Range constraints (for numbers) and set-constraints (for nominal data with a limited set of options)
* Uniqueness
* Foreign key constraints (a generalization for set membership)
* [[Regex]] patterns

# Refs

https://en.wikipedia.org/wiki/Data_cleansing

https://www.simplilearn.com/what-is-data-scrubbing-article