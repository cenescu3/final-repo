# final-repo
Working on R studio final exam

library(tidyverse)
library(janitor)
library(stringr)

# Load the dataset
data <- read_csv("Site_To_ReportingArea_022324.csv")

# Clean column names for consistency
data <- data %>% clean_names()

# Remove duplicate rows
data <- data %>% distinct()

# Remove sites with missing coordinates
data <- data %>% filter(!is.na(site_lat), !is.na(site_long))
