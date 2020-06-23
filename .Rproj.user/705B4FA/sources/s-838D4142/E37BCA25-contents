library(tidyverse)
library(lubridate)

#Read in the data.

netflix <- read_csv("../raw_data/netflix_titles.csv")


# Splitind data into two categories netflix_movie and netflix_tv

netflix_movie <- netflix %>%
  select(-show_id) %>%
  filter(type == "Movie") %>%
  select(-type)

netflix_tv <- netflix %>%
  select(-show_id) %>%
  filter(type == "TV Show") %>%
  select(-type)

#replacing NA with Unknown

netflix_tv <-  netflix_tv %>%
  replace(is.na(.), "Unknown")

netflix_movie <-  netflix_movie %>%
  replace(is.na(.), "Unknown")

#Tidy duration columns in both datasets.

netflix_tv <- netflix_tv %>%
  rename(number_of_seasons = duration) %>%
  mutate(number_of_seasons = str_extract(number_of_seasons, "[:digit:]+")) %>%
  mutate(number_of_seasons = as.numeric(number_of_seasons))


netflix_movie <-  netflix_movie %>%
  mutate(duration = str_extract(duration, "[:digit:]+")) %>%
  rename(duratiom_min = duration)


#Fixing country column for many countries.

netflix_tv <- netflix_tv %>%
  mutate(country = if_else(sapply(strsplit(country, ","), length) > 1, "International", country))

netflix_movie <-  netflix_movie %>%
  mutate(country = ifelse(sapply(strsplit(country, ","), length) > 1, "International", country))


netflix_movie <-  netflix_movie %>% 
  mutate(country = str_replace(country,","," ")) 


netflix_tv <- netflix_tv %>% 
  mutate(country = str_replace(country,","," "))

#Fixing  columns types.

netflix_tv <- netflix_tv %>%
  mutate(date_added = parse_date_time(date_added, orders = c("mdy")))


netflix_movie <- netflix_movie %>%
  mutate(date_added = parse_date_time(date_added, orders = c("mdy"))) %>%
  mutate(duratiom_min = as.numeric(duratiom_min))

#Saving tables to csv files. 

netflix_movie %>% write_csv("../clean_data/netflix_movie.csv")

netflix_tv %>% write_csv("../clean_data/netflix_tv.csv")

rm(netflix,netflix_movie,netflix_tv)
