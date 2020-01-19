---
title: "CASE STUDY 2"
author: "Kristeena Whittier"
date: "January 18, 2020"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    code_folding: hide
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---





## Background

I learned quite a few things from making these graph. The first thing being how graphs can be split by using the "facet_wrap" function. I can also impose two sets of data on the same graph that can also have different appearances by using the "geom" typ functions.Then by using "ggsave" the graphs can then be saved as seperate images. It is by making use of these functions that graphs and images can be better read and interpreted.

## Images


```r
# Use this R-Chunk to import all your datasets!
gap <- gapminder
gap <- filter(gap, country != "Kuwait")
gap$pop <- gap$pop/100000

#gdp per capita = y
#life expectancy = x
#continent = color
#population = size
#year = graph

ggplot(data = gap, mapping = aes(x=lifeExp, y=gdpPercap, size=pop, col=continent)) +
  geom_point() +
  theme_bw() +
  scale_y_continuous(trans="sqrt") +
  facet_wrap(~year, nrow=1) +
  labs(x="Life Expectancy", y="GDP per capita", size="Population (100k)", col="continent") +
  ggsave("cs02plot1.png", width = 15)
```

![](Case_Study_2_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


```r
#gdp per capita = y
#year = x
#continent = graph
#population = size
#continent = color

gap2 <- gap %>%
  group_by(continent, year) %>%
  summarise(
    count = n(),
    gdpPercap = weighted.mean(gdpPercap, pop),
    pop = sum(pop)
  )

ggplot(data=gap, mapping = aes(x=year, y=gdpPercap, col=continent)) +
  geom_point(mapping = aes(size=pop)) +
  geom_line(mapping =aes(group = country)) +
  geom_point(data = gap2, mapping = aes(size=pop), color = "black") +
  geom_line(data=gap2, color = "black") +
  theme_bw() +
  facet_wrap(~continent, nrow=1) +
  labs(x="Year", y="GDP per capita", size="Population (100k)", col="Continent") +
  ggsave("cs02plot2.png", width = 15)
```

![](Case_Study_2_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

