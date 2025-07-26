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

# Trim whitespace in character columns
data <- data %>%
  mutate(across(where(is.character), str_trim))

#Look at cleaned data set
glimpse(data)

#Summary stats ----------------------------------------
sites_per_area <- data %>%
  count(reporting_area_name, sort = TRUE)

sites_per_agency <- data %>%
  count(site_agency_name, sort = TRUE)

sites_per_state <- data %>%
  filter(reporting_area_state %in% state.abb) %>%
  count(reporting_area_state, sort = TRUE)

#Plot
sites_per_area %>%
  slice_max(n, n = 15) %>%
  ggplot(aes(x = reorder(reporting_area_name, n), y = n)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +
  labs(title = "Top 15 Reporting Areas by Number of Sites",
       x = "Reporting Area",
       y = "Number of Sites") +
  theme_minimal()
