

final_presentation
The Canadian Employment Rates by Gender for selected years
========================================================
author: Tarig Elamin
date:    March20,2015
transition: rotate

Introduction
========================================================

This presentation attempted to use rCharts to view the Canadian employment rate trends in selected years within 1976-2009.

Relevance

The employment rate shows the percentage of Canadian adults (15 years of age and over) working for pay, and thus in a position to earn income to take care of themselves and their families.

National Employment Picture

Economic cycles and recessions notwithstanding, Canada's employment rate increased over the last three decades and a half. In 2012, the percentage of adult Canadians who were holding a job was 61.8%, up from 57.1% in 1976, an increase of 4.7 percentage points.

The shiny Code for the interactive rCharts is :
========================================================


```r
#The ui R code is :
require(rCharts)
options(RCHART_LIB = 'polycharts')
shinyUI(pageWithSidebar(
  headerPanel("Percentage of Employed By Gender in Canada"),
  
  sidebarPanel(
    selectInput(inputId = "year",
                label = "Select year to compare provinces",
                choices = sort(unique(data_employ$year)),
                selected = 2001),
    selectInput(inputId = "province",
                label = "Select province to compare years",
                choices = sort(unique(data_employ$province)),
                selected = "Ontario")
  ),
  
  mainPanel(
    showOutput("chart1", "polycharts"),
    showOutput("chart2", "polycharts")
  )
))
#The server R code is:
require(rCharts)
options(RCHART_WIDTH = 800)
shinyServer(function(input, output) {
  output$chart1 <- renderChart({
    YEAR = input$year
    men <- subset(data_employ, gender == "male" & year == YEAR)
    women <- subset(data_employ, gender == "female" & year == YEAR)
    p1 <- rPlot(x = list(var = "provincecode", sort = "value"), y = "value", 
                color = 'gender', data = women, type = 'bar')
    p1$layer(x = "provincecode", y = "value", color = 'gender', 
             data = men, type = 'point', size = list(const = 3))
    p1$addParams(height = 300, dom = 'chart1', 
                 title = "Percentage of Employed by gender in Canada")
    p1$guides(x = list(title = "", ticks = unique(men$provincecode)))
    p1$guides(y = list(title = "", max = 80))
    return(p1)
  })
  output$chart2 <- renderChart({
   PROVINCE = input$province
    province = subset(data_employ, province == PROVINCE)
    p2 <- rPlot(value ~ year, color = 'gender', type = 'line', data = province)
    p2$guides(y = list(min = 0, title = ""))
    p2$guides(y = list(title = ""))
    p2$addParams(height = 300, dom = 'chart2')
    return(p2)
  })
})
```

The link to the shiny application is :
========================================================

[the link to the interactive rChart]( http://tarigelamin.shinyapps.io/shinyyy)

Summary :
========================================================

- From the graph,it seemed that there is a clear postive upward trend

  Employment rates for women aged 15 and over, 1976 to 2009

- The employment rate gap between women and men is closing more and more

- Data source :Statistics Canada,at : www.statcan.gc.ca
 

 

  


