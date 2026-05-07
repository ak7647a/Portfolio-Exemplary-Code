# Portfolio item 2: Exemplary Codes: 

## 1. Reusable Summary Table Function

This project showcases `summary_table()`, a reusable R function that automates
the generation of clean summary statistics tables. I worked on this table in my midterm project,
then generalized so it could be applied to any dataset, grouping variable, and numeric
variable without rewriting code each time. In many research settings, analysts repeatedly
calculate the same basic statistics across different groups. This function automates that process.
 
The function is built with `tidyverse` syntax and styled using `kableExtra`, which allows it to
produce publication quality tables in both PDF and HTML output. It uses tidy evaluation (`{{ }}`)
so that column names can be passed directly as arguments, handles missing values automatically,
and applies consistent header styling without needing any additional formatting code from the user.
The demonstration applies it three times to the `penguins` dataset across different grouping
and numeric variables, showing how the same function works in different contexts.


## Files
**[View Code](./Portfolio2PS3.qmd)** | **[View PDF](./Portfolio2PS3.pdf)**


## The Function
 
See the full code in [this file](./Portfolio2PS3.qmd). The core function:
 
```r
summary_table <- function(data, group_var, numeric_var,
                          caption = NULL, digits = 1) {
  data |>
    filter(!is.na({{ group_var }}), !is.na({{ numeric_var }})) |>
    group_by({{ group_var }}) |>
    summarise(
      N = n(),
      Mean = round(mean({{ numeric_var }}), digits),
      SD = round(sd({{ numeric_var }}),   digits),
      .groups = "drop"
    ) |>
    rename(Group = {{ group_var }}) |>
    kbl(caption = caption, align = c("l", "c", "c", "c"), booktabs = TRUE) |>
    kable_styling(position = "center", latex_options = "hold_position") |>
    row_spec(0, bold = TRUE, background = "#2E86AB", color = "#F2F0EF") |>
    column_spec(1, bold = TRUE)
}
```

---

## 2. Interactive Trump Approval Rating Graph

This project features an interactive replication of [Silver Bulletin's Trump approval rating tracker](https://www.natesilver.net/p/trump-approval-ratings-nate-silver-bulletin), built in R using `ggplot2` and `plotly`. The visualization displays Donald Trump's approval and disapproval polling averages across his second term, including 90% confidence bands rendered as shaded ribbons around each trend line.

The core of the work involved two layers of construction: first building a static `ggplot` with styled ribbons, dotted confidence bounds, end-of-line labels, and a custom theme, second step was converting it to an interactive `plotly` figure using `ggplotly()` and formatting the title, caption, annotations, and background within plotly's `layout()` system, since ggplot styling does not transfer to `plotly()`. This result in a fully interactive chart rendered inside a Quarto revealjs slide deck.


### Output

**[View Slide Deck](https://ak7647a.github.io/Portfolio-Exemplary-Code/)** | **[View Code](./Portfolio2.qmd)**

### Data

Polling average data sourced from [Silver Bulletin](https://www.natesilver.net/p/trump-approval-ratings-nate-silver-bulletin), accessed February 2026.