# R programming tips    

## 💻 Set up  
+ [Positron IDE setup tips](./tutorials/s-positron_setup/s-positron_setup.md)   
 
## 🎨 Data visualisation  
+ [An introduction to `ggplot2` using volcano plots](./tutorials/dv-volcano_plots_with_ggplot/dv-volcano_plots_with_ggplot.md) (Updated)  
+ [Using `DiagrammeR` to draw flow charts](./tutorials/dv-using_diagrammer/dv-using_diagrammer.md) (Updated)  

## 📚 Data cleaning
+ [Data cleaning using `data.table` or `tidyverse` (or Python `Pandas`)](./tutorials/dc-data_table_vs_dplyr/dc-data_table_vs_dplyr.md) (Updated)    
+ [Cleaning strings using regular expressions with base R or `stringr`](./tutorials/dc-cleaning_strings/dc-cleaning_strings.md) (Updated)          

## 🔨 Productionisation  
+ [Creating SQL <> R workflows - Part 1](./tutorials/p-sql_to_r_workflows/p-sql_to_r_workflows_part_1.md) (Updated)  
+ [Creating SQL <> R workflows - Part 2](./tutorials/p-sql_to_r_workflows/p-sql_to_r_workflows_part_2.md) (Updated)  
+ [Automating R Markdown report generation - Part 1](./tutorials/p-automating_rmd_reports/p-automating_rmd_reports_part_1.md) (Updated)  
+ [Automating R Markdown report generation - Part 2](./tutorials/p-automating_rmd_reports/p-automating_rmd_reports_part_2.md) (updated)   

## 🔢 Statistical modelling   
+ TBC  

## 🔮 Machine learning   
+ TBC  

# Other resources 
The resources below also cover a comprehensive range of practical R tutorials.  

+ [Statistical Computing](https://36-750.github.io/) by Alex Reinhart and Christopher Genovese  
+ [Data Science Toolkit](https://benkeser.github.io/info550/lectures/) by David Benkeser  
+ [What They Forgot to Teach You About R](https://rstats.wtf/index.html) by Jennifer Bryan and Jim Hester

# Tutorial style guide  

This repository now contains the following file naming and code style rules.  

+ Folders are no longer ordered with a numerical prefix and names are no longer case sensitive e.g `r_tips\tutorials\...` and `r_tips\figures\...`    
+ Tutorial subtopics share the same prefix e.g. `r_tips\tutorials\dv-...` and   `r_tips\tutorials\st-...`  
+ File names contain `-` to separate file name prefixes and `_` instead of other white space e.g. `r_tips\figures\dv-using_diagrammer-simple_flowchart.svg`  
+ Comments are styled according to the [tidyverse style guide](https://style.tidyverse.org/functions.html?q=comments#comments-1):    
  + The first comment explains the purpose of the code chunk and is styled differently for enhanced readability e.g. `# Code as header --------`     
  + Comments are written in sentence case and only end with a full stop if they contain at least two sentences  
  + Short comments explaining a function argument do not have to be written on a new line  
  + Comments should not be followed by a blank line, unless the comment is a stand-alone paragraph containing in-depth rationale or an alternative solution  
+ R code chunks are styled as follows:  
  + Each R chunk should be named with a short unique description written in the active voice e.g. `create basic plot` and `modify plot labels`    
  + Arguments inside code chunks should not contain white space and boolean argument options should be written in capitals e.g. `{r load libraries, message=FALSE, warning=FALSE}`   
  + To render the github document, results are generally suppressed using `results='hide'` and manually entered in a new line beneath the code.  
  + To render the github document, figures are generally outputed using `fig.show='markdown'` and figure outputs can then be suppressed at the local chunk level using `fig.show='hide'`  
+ Set a margin of 80 characters length in RStudio through `Tools\Global options --> Code --> Display --> Show margin` and use this margin as the cut-off for code and comments length   

# Citations  

Citing packages is a good practice when you are publishing research papers. To do this, use `citations("package")` to print the relevant package publication. A non-exhaustive list of R packages used in this repository is found below.  

+ R Core Team (2021). R: A language and environment for statistical computing. R Foundation for
  Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.
+ Wickham et al., (2019). Welcome to the `tidyverse`. Journal of Open Source Software, 4(43),
  1686, https://doi.org/10.21105/joss.01686
+ H. Wickham. `ggplot2`: Elegant Graphics for Data Analysis. Springer-Verlag New York, 2016.
+ Matt Dowle and Arun Srinivasan (2021). `data.table`: Extension of `data.frame`. R package
  version 1.14.2. https://CRAN.R-project.org/package=data.table  

# Acknowledgements  

Many kudos to [Dr Chuanxin Liu](https://github.com/codetrainee), my former PhD student and code editor, for teaching me how to code in R in my past life as an immunologist.   

<p align="center">  
<img src="https://github.com/erikaduan/r_tips/blob/master/figures/r_milestones.jpg"
width="600"></center>  
</p>  
