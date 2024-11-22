---
title: "Japanese Economy"
author: "Ayumu Tanaka"
date: "2024-11-21"
output:
  html_document:
    toc: true
    toc_float: true
    number_sections: yes
    keep_md: yes

#output:
#   pdf_document: 
#     latex_engine: xelatex 
#documentclass: bxjsarticle
#classoption: xelatex,ja=standard
#geometry: no
#bibliography: ref.bib
#link-citations: yes
---



This version: 2024-11-21

#  Load WDI package and search the data


``` r
library(WDI)
library(dplyr)
# Search GDP per capita, PPP
WDIsearch("GDP per capita, PPP")
```

```
##                  indicator                                                 name
## 717     6.0.GDPpc_constant GDP per capita, PPP (constant 2011 international $) 
## 11434    NY.GDP.PCAP.PP.CD        GDP per capita, PPP (current international $)
## 11435    NY.GDP.PCAP.PP.KD  GDP per capita, PPP (constant 2017 international $)
## 11436 NY.GDP.PCAP.PP.KD.87  GDP per capita, PPP (constant 1987 international $)
## 11437 NY.GDP.PCAP.PP.KD.ZG                GDP per capita, PPP annual growth (%)
```

``` r
# Download the WDI data
WDI <- WDI(indicator = c("GDP_PPP" = "NY.GDP.MKTP.PP.KD", 
                         "GDP_per_capita" = "NY.GDP.PCAP.PP.KD",
                         "GDP_growth" = "NY.GDP.MKTP.KD.ZG",
                         "Ease_business" = "IC.BUS.DFRN.XQ"), 
              start = 1989, end = 2023)

# Save the data as CSV
rio::export(WDI, "WDI.csv")
```



# GDP per capita: US versus Japan


``` r
# Filter the data
GDP_per_capita <- WDI %>% 
  filter(iso3c %in% c("USA", "JPN")) %>% 
  select(country, year, GDP_per_capita) 

# Plot the GDP by year and country
library(ggplot2)
ggplot(GDP_per_capita, aes(x = year, y = GDP_per_capita, color = country)) +
  geom_line() +
  labs(title = "GDP per capita: US versus Japan",
       x = "Year",
       y = "GDP per capita (PPP, constant 2017 international $)",
       caption = "Source: World Bank's World Development Indicators") +
  theme_minimal() +
# Add text "Pandemic" in 2020
  geom_text(data = GDP_per_capita %>% filter(year == 2020), 
            aes(label = "Pandemic"), 
            x = 2020, y = 60000, 
            color = "black", size = 3) +
  scale_y_continuous(labels = scales::dollar_format(prefix = "$")) +
# Add text "Global Financial Crisis" in 2008
  geom_text(data = GDP_per_capita %>% filter(year == 2008), 
            aes(label = "Global Financial Crisis"), 
            x = 2008, y = 50000, 
            color = "black", size = 3) 
```

![GDP per capita: US versus Japan](01_Japanese_Economy_files/figure-html/gdp-per-capita-1.png)







