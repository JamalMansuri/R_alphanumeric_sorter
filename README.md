# Alphanumeric Sorting in R 
# JM {06/04/2024}

Alphanumeric sorting without the use of fancy tools


```r
library(dplyr)
library(tidyr)


df <- data.frame(
  Sample_ID = c("A1", "A2", "B10", "A10", "B1", "A20", "C1"),
  Value = c(10, 20, 30, 40, 50, 60, 70)
)

# Splits the Sample_ID alphanumeric into alphabets and numerics
df <- df %>%
  separate(Sample_ID, into = c("alpha", "numeric"), sep = "(?<=\\D)(?=\\d)", remove = FALSE, convert = TRUE)

# Split the data frame into a list of data frames based on the first character of 'Sample_ID'
df_list <- split(df, substr(df$Sample_ID, 1, 1))

# Sorting the alphanumeric sample ID column
df_list <- lapply(df_list, function(df) df[order(df$numeric), ])

# Recombining df_list into df_sorted
df_sorted <- do.call(rbind, df_list)

# Remove alpha and numeric columns
df_sorted <- df_sorted[, -which(names(df_sorted) %in% c("alpha", "numeric"))]

print(df_sorted)
```
