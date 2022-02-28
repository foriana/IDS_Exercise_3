---
title: 'Weekly Exercises #3'
author: "Felicia Peterson"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday dog breed data
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')

# Tidy Tuesday data for challenge problem
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>%
  mutate(day_of_week = wday(date, label=TRUE)) %>% 
  group_by(vegetable, day_of_week) %>% 
  mutate(weight_lb = weight*0.00220462) %>% 
  summarize(sum_weight = sum(weight_lb)) %>%
 
  pivot_wider(
              names_from = day_of_week,
              values_from = sum_weight) 
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.0661386","4":"0.11023100","5":"0.02645544","6":"0.46737944","7":"NA","8":"NA"},{"1":"beans","2":"4.70906832","3":"6.5080382","4":"4.38719380","5":"3.39291018","6":"1.52559704","7":"1.91361016","8":"4.08295624"},{"1":"beets","2":"0.37919464","3":"0.6724091","4":"0.15873264","5":"11.89172028","6":"0.02425082","7":"0.32187452","8":"0.18298346"},{"1":"broccoli","2":"NA","3":"0.8201186","4":"NA","5":"NA","6":"0.16534650","7":"1.25883802","8":"0.70768302"},{"1":"carrots","2":"2.33028334","3":"0.8708249","4":"0.35273920","5":"2.67420406","6":"2.13848140","7":"2.93655384","8":"5.56225626"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.01763696"},{"1":"cilantro","2":"0.03747854","3":"NA","4":"0.00440924","5":"NA","6":"0.07275246","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"0.7583893","4":"0.72752460","5":"NA","6":"3.44802568","7":"1.45725382","8":"5.30211110"},{"1":"cucumbers","2":"9.64080326","3":"4.7752069","4":"10.04645334","5":"3.30693000","6":"7.42956940","7":"3.10410496","8":"5.30652034"},{"1":"edamame","2":"4.68922674","3":"NA","4":"1.40213832","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"1.2588380","4":"0.14109568","5":"NA","6":"NA","7":"NA","8":"0.06834322"},{"1":"jalapeño","2":"1.50796008","3":"5.5534378","4":"0.54895038","5":"0.22487124","6":"1.29411194","7":"0.26234978","8":"0.48060716"},{"1":"kale","2":"1.49032312","3":"2.0679336","4":"0.28219136","5":"0.27998674","6":"0.38139926","7":"0.82673250","8":"0.61729360"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"0.42108242","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"1.31615814","3":"2.4581513","4":"0.91712192","5":"2.45153744","6":"1.80117454","7":"1.46607230","8":"1.18608556"},{"1":"onions","2":"1.91361016","3":"0.5092672","4":"0.70768302","5":"0.60186126","6":"0.07275246","7":"0.26014516","8":"NA"},{"1":"peas","2":"2.85277828","3":"4.6341112","4":"2.06793356","5":"3.39731942","6":"0.93696350","7":"2.05691046","8":"1.08026380"},{"1":"peppers","2":"1.38229674","3":"2.5264945","4":"1.44402610","5":"0.70988764","6":"0.33510224","7":"0.50265336","8":"2.44271896"},{"1":"potatoes","2":"2.80207202","3":"0.9700328","4":"NA","5":"11.85203712","6":"3.74124014","7":"NA","8":"4.57017726"},{"1":"pumpkins","2":"92.68883866","3":"30.1195184","4":"31.85675900","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.1962112","4":"0.09479866","5":"0.14770954","6":"0.19400656","7":"0.08157094","8":"NA"},{"1":"raspberries","2":"0.53351804","3":"0.1300726","4":"0.33510224","5":"0.28880522","6":"0.57099658","7":"NA","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"NA","4":"NA","5":"NA","6":"3.57809826","7":"19.26396956","8":"NA"},{"1":"spinach","2":"0.26014516","3":"0.1477095","4":"0.49603950","5":"0.23368972","6":"0.19621118","7":"0.48722102","8":"0.21384814"},{"1":"squash","2":"56.22221924","3":"24.3345956","4":"18.46810174","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.4784025","4":"NA","5":"0.08818480","6":"0.48722102","7":"0.08157094","8":"NA"},{"1":"Swiss chard","2":"0.73413846","3":"1.0736499","4":"0.07054784","5":"2.23107544","6":"0.61729360","7":"1.24781492","8":"0.90830344"},{"1":"tomatoes","2":"35.12621046","3":"11.4926841","4":"48.75076206","5":"34.51773534","6":"85.07628580","7":"75.60964752","8":"58.26590198"},{"1":"zucchini","2":"3.41495638","3":"12.1959578","4":"16.46851140","5":"34.63017096","6":"18.72163304","7":"12.23564100","8":"2.04147812"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


__I think the problem is that the summarized garden harvest table means that there is only one representing row for each vegetable variety; the garden_planting data has yet to be summarized and reflects the multiple plantings of each vegetable variety. For future data collections, documenting which plot the vegetable harvest came from would allow us to join by vegetable, variety, AND plot. I would group_by and summarize the garden_planting data so that the amount of rows should be the same and able to join together.__



```r
garden_harvest %>% 
  group_by(vegetable, variety) %>% 
  mutate(weight_lb = weight*0.00220462) %>% 
  summarize(total_weight = sum(weight_lb)) %>% 
  left_join(garden_planting,
            by = c("vegetable", "variety"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["total_weight"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34392072","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04409240","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08026380","4":"potB","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"M","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"D","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"K","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"L","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Classic Slenderette","3":"3.60455370","4":"E","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"Gourmet Golden","3":"7.02171470","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"leaves","3":"0.22266662","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.38678414","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"D","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"I","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Yod Fah","3":"0.82011864","4":"P","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"greens","3":"0.37258078","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"chives","2":"perrenial","3":"0.01763696","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"potD","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"E","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"Dorinny Sweet","3":"11.40670388","4":"A","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"Golden Bantam","3":"1.60275874","4":"B","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"pickling","3":"43.60958822","4":"L","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"edamame","3":"6.09136506","4":"O","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"thai","3":"0.14770954","4":"potB","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potC","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"giant","3":"9.87228836","4":"L","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"P","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"front","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42108242","4":"front","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"C","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"L","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.74875148","4":"G","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"mustard greens","3":"0.05070626","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"reseed","3":"0.09920790","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89466606","4":"P","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"onions","2":"Delicious Duo","3":"0.75398004","4":"P","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31133924","4":"H","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"Magnolia Blossom","3":"7.45822946","4":"B","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"Super Sugar Snap","3":"9.56805080","4":"A","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"K","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"O","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potD","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"purple","3":"3.00930630","4":"D","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"red","3":"4.43349082","4":"I","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"potatoes","2":"Russet","3":"9.09185288","4":"D","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87308882","4":"B","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"New England Sugar","3":"44.85960776","4":"K","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"saved","3":"76.93241952","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"C","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"G","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"H","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"raspberries","2":"perrenial","3":"1.85849466","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74032380","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"H","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"E","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"delicata","3":"10.49840044","4":"K","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"B","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"side","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"K","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"strawberries","2":"perrenial","3":"1.30513504","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88282364","4":"M","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"N","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Big Beef","3":"24.99377694","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Black Krim","3":"15.80712540","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Bonny Best","3":"24.92322910","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Brandywine","3":"15.64618814","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71232674","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"grape","3":"32.39468628","4":"O","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Jet Star","3":"15.02448530","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Old German","3":"26.71778978","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"N","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"J","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"front","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"O","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"Romanesco","3":"99.70834874","4":"D","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
  __To create the dataset, I would first prepare the garden_harvest data to match with the garden_spending data more easily. I would group by vegetable and variety, then summarize their weight, then mutate the weight in grams to weight in pounds. Next, I would begin with garden_spending and use left_join() to join the two datasets based on the vegetable and variety. This should give me a dataset with the vegetables, varieties, summed weight in lbs of harvest, and theother variables in the garden_spending data.Then, I would read in the Whole Foods vegetable data and manipulate it to filter out the produce, leaving the vegetables that match Lisa's harvest. I would left_join() the data again on the basis of vegetable name. To analyze, I would create a datatable to let me compare the monetary savings. Variables needed to mutate: weight in pounds of Lisa's harvest divided by the price of seed, price per pound of Whole Foods, then the difference (Seed/Harvest ratio - Whole Foods price) variable.__

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(variety) %>% 
  summarize(weight=sum(weight)) %>%
  mutate(weight_lb = weight*0.00220462) %>% 
 # fct_infreq(weight_lb) %>% 
  
  ggplot(aes(x = variety,  
             y = reorder(weight_lb, variety))) +
  geom_col(aes(fill = variety)) +
 # geom_text(aes(label = date), vjust = 1.5, color = "white") + my challenge attempt
  
  labs(title = "Tomato Time",
       subtitle = "Total tomato harvest in pounds for each variety",
       y = "",
       x = "") +
  
theme(legend.position = "none",
      axis.text.x = element_text(angle = 90))
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(lower_variety = str_to_lower(variety)) %>% 
  mutate(letter_num = str_length(lower_variety)) %>% 
  group_by(vegetable, lower_variety, letter_num) %>% 
  select(vegetable, lower_variety, letter_num) %>% 
  distinct() %>% 
  arrange(letter_num)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["lower_variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["letter_num"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"potatoes","2":"red","3":"3"},{"1":"hot peppers","2":"thai","3":"4"},{"1":"tomatoes","2":"grape","3":"5"},{"1":"jalapeño","2":"giant","3":"5"},{"1":"peppers","2":"green","3":"5"},{"1":"pumpkins","2":"saved","3":"5"},{"1":"lettuce","2":"reseed","3":"6"},{"1":"beets","2":"leaves","3":"6"},{"1":"lettuce","2":"tatsoi","3":"6"},{"1":"carrots","2":"dragon","3":"6"},{"1":"carrots","2":"bolero","3":"6"},{"1":"potatoes","2":"purple","3":"6"},{"1":"potatoes","2":"yellow","3":"6"},{"1":"carrots","2":"greens","3":"6"},{"1":"potatoes","2":"russet","3":"6"},{"1":"hot peppers","2":"variety","3":"7"},{"1":"peppers","2":"variety","3":"7"},{"1":"broccoli","2":"yod fah","3":"7"},{"1":"edamame","2":"edamame","3":"7"},{"1":"apple","2":"unknown","3":"7"},{"1":"spinach","2":"catalina","3":"8"},{"1":"cilantro","2":"cilantro","3":"8"},{"1":"cucumbers","2":"pickling","3":"8"},{"1":"tomatoes","2":"big beef","3":"8"},{"1":"tomatoes","2":"jet star","3":"8"},{"1":"squash","2":"delicata","3":"8"},{"1":"squash","2":"red kuri","3":"8"},{"1":"chives","2":"perrenial","3":"9"},{"1":"strawberries","2":"perrenial","3":"9"},{"1":"asparagus","2":"asparagus","3":"9"},{"1":"Swiss chard","2":"neon glow","3":"9"},{"1":"raspberries","2":"perrenial","3":"9"},{"1":"zucchini","2":"romanesco","3":"9"},{"1":"tomatoes","2":"bonny best","3":"10"},{"1":"carrots","2":"king midas","3":"10"},{"1":"tomatoes","2":"better boy","3":"10"},{"1":"tomatoes","2":"old german","3":"10"},{"1":"tomatoes","2":"brandywine","3":"10"},{"1":"tomatoes","2":"black krim","3":"10"},{"1":"tomatoes","2":"volunteers","3":"10"},{"1":"tomatoes","2":"amish paste","3":"11"},{"1":"beets","2":"sweet merlin","3":"12"},{"1":"squash","2":"blue (saved)","3":"12"},{"1":"basil","2":"isle of naxos","3":"13"},{"1":"onions","2":"delicious duo","3":"13"},{"1":"corn","2":"dorinny sweet","3":"13"},{"1":"corn","2":"golden bantam","3":"13"},{"1":"lettuce","2":"mustard greens","3":"14"},{"1":"beets","2":"gourmet golden","3":"14"},{"1":"lettuce","2":"lettuce mixture","3":"15"},{"1":"tomatoes","2":"cherokee purple","3":"15"},{"1":"tomatoes","2":"mortgage lifter","3":"15"},{"1":"radish","2":"garden party mix","3":"16"},{"1":"kale","2":"heirloom lacinto","3":"16"},{"1":"peas","2":"magnolia blossom","3":"16"},{"1":"peas","2":"super sugar snap","3":"16"},{"1":"rutabaga","2":"improved helenor","3":"16"},{"1":"beans","2":"bush bush slender","3":"17"},{"1":"broccoli","2":"main crop bravado","3":"17"},{"1":"kohlrabi","2":"crispy colors duo","3":"17"},{"1":"squash","2":"waltham butternut","3":"17"},{"1":"pumpkins","2":"new england sugar","3":"17"},{"1":"beans","2":"chinese red noodle","3":"18"},{"1":"beans","2":"classic slenderette","3":"19"},{"1":"onions","2":"long keeping rainbow","3":"20"},{"1":"lettuce","2":"farmer's market blend","3":"21"},{"1":"pumpkins","2":"cinderella's carraige","3":"21"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  mutate(stuttering_vegetable = str_detect(variety, "er|ar")) %>% 
  distinct()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]},{"label":["stuttering_vegetable"],"name":[6],"type":["lgl"],"align":["right"]}],"data":[{"1":"lettuce","2":"reseed","3":"2020-06-06","4":"20","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams","6":"TRUE"},{"1":"lettuce","2":"reseed","3":"2020-06-08","4":"15","5":"grams","6":"FALSE"},{"1":"lettuce","2":"reseed","3":"2020-06-09","4":"10","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-11","4":"67","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-11","4":"9","5":"grams","6":"FALSE"},{"1":"beets","2":"leaves","3":"2020-06-11","4":"8","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-13","4":"53","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-13","4":"19","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-13","4":"14","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-06-13","4":"10","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-17","4":"48","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-17","4":"58","5":"grams","6":"FALSE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-17","4":"8","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams","6":"TRUE"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-06-18","4":"40","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-18","4":"47","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-18","4":"59","5":"grams","6":"FALSE"},{"1":"beets","2":"leaves","3":"2020-06-18","4":"25","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-06-19","4":"58","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-19","4":"39","5":"grams","6":"TRUE"},{"1":"beets","2":"leaves","3":"2020-06-19","4":"11","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-19","4":"38","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-20","4":"22","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-20","4":"25","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Tatsoi","3":"2020-06-20","4":"18","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-20","4":"16","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-20","4":"71","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-20","4":"148","5":"grams","6":"TRUE"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams","6":"TRUE"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-21","4":"37","5":"grams","6":"TRUE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-21","4":"19","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-06-21","4":"71","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-21","4":"95","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-21","4":"51","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-21","4":"13","5":"grams","6":"FALSE"},{"1":"beets","2":"leaves","3":"2020-06-21","4":"57","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-06-21","4":"60","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-06-22","4":"37","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-22","4":"52","5":"grams","6":"TRUE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-22","4":"40","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-22","4":"19","5":"grams","6":"FALSE"},{"1":"strawberries","2":"perrenial","3":"2020-06-22","4":"19","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-22","4":"18","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-23","4":"40","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-23","4":"165","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-23","4":"41","5":"grams","6":"FALSE"},{"1":"cilantro","2":"cilantro","3":"2020-06-23","4":"2","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-06-23","4":"5","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-24","4":"34","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-24","4":"122","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-25","4":"22","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-25","4":"30","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-06-26","4":"17","5":"grams","6":"TRUE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-26","4":"425","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-27","4":"52","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Tatsoi","3":"2020-06-27","4":"89","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-06-27","4":"60","5":"grams","6":"FALSE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-27","4":"333","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-28","4":"793","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-06-28","4":"99","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-28","4":"111","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-29","4":"58","5":"grams","6":"TRUE"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-29","4":"625","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-29","4":"561","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-06-29","4":"30","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-29","4":"82","5":"grams","6":"TRUE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-30","4":"32","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-06-30","4":"80","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-01","4":"60","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Tatsoi","3":"2020-07-02","4":"144","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-07-02","4":"16","5":"grams","6":"FALSE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-07-02","4":"798","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-02","4":"743","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-03","4":"217","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Tatsoi","3":"2020-07-03","4":"216","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-03","4":"88","5":"grams","6":"TRUE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-03","4":"9","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-04","4":"285","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-07-04","4":"457","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-04","4":"147","5":"grams","6":"TRUE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-06","4":"17","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-06","4":"175","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Tatsoi","3":"2020-07-06","4":"189","5":"grams","6":"FALSE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-07-06","4":"433","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-06","4":"48","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-07","4":"67","5":"grams","6":"TRUE"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-07","4":"62","5":"grams","6":"FALSE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams","6":"TRUE"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-07","4":"43","5":"grams","6":"TRUE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-07","4":"11","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-07","4":"13","5":"grams","6":"TRUE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-08","4":"75","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-07-08","4":"252","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-08","4":"178","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-08","4":"39","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-07-08","4":"181","5":"grams","6":"FALSE"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-08","4":"83","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-07-08","4":"96","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Tatsoi","3":"2020-07-08","4":"75","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-09","4":"61","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-07-09","4":"131","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-09","4":"140","5":"grams","6":"TRUE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-09","4":"69","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-07-09","4":"78","5":"grams","6":"FALSE"},{"1":"raspberries","2":"perrenial","3":"2020-07-10","4":"61","5":"grams","6":"TRUE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-10","4":"150","5":"grams","6":"FALSE"},{"1":"raspberries","2":"perrenial","3":"2020-07-11","4":"60","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-07-11","4":"77","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-07-11","4":"19","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-11","4":"79","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-07-11","4":"105","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-11","4":"701","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-07-11","4":"24","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-12","4":"130","5":"grams","6":"FALSE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-12","4":"89","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-12","4":"492","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-12","4":"83","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-07-13","4":"47","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-13","4":"145","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-13","4":"50","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-07-13","4":"85","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-13","4":"53","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Tatsoi","3":"2020-07-13","4":"137","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-13","4":"40","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-13","4":"443","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-07-14","4":"128","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-14","4":"152","5":"grams","6":"FALSE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-07-14","4":"207","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-14","4":"526","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-07-14","4":"152","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-15","4":"393","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-15","4":"743","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-07-15","4":"1057","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-07-15","4":"39","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-07-16","4":"29","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-07-16","4":"61","5":"grams","6":"TRUE"},{"1":"onions","2":"Delicious Duo","3":"2020-07-16","4":"50","5":"grams","6":"FALSE"},{"1":"strawberries","2":"perrenial","3":"2020-07-17","4":"88","5":"grams","6":"TRUE"},{"1":"cilantro","2":"cilantro","3":"2020-07-17","4":"33","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-17","4":"16","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-07-17","4":"20","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-17","4":"347","5":"grams","6":"FALSE"},{"1":"raspberries","2":"perrenial","3":"2020-07-18","4":"77","5":"grams","6":"TRUE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-18","4":"172","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-07-18","4":"61","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-18","4":"81","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-18","4":"294","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-18","4":"660","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-07-19","4":"113","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-19","4":"531","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-19","4":"344","5":"grams","6":"FALSE"},{"1":"strawberries","2":"perrenial","3":"2020-07-19","4":"37","5":"grams","6":"TRUE"},{"1":"peas","2":"Magnolia Blossom","3":"2020-07-19","4":"140","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-20","4":"134","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-20","4":"179","5":"grams","6":"FALSE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-07-20","4":"336","5":"grams","6":"TRUE"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-20","4":"107","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-07-20","4":"128","5":"grams","6":"FALSE"},{"1":"hot peppers","2":"thai","3":"2020-07-20","4":"12","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-20","4":"519","5":"grams","6":"TRUE"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams","6":"TRUE"},{"1":"jalapeño","2":"giant","3":"2020-07-20","4":"197","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Tatsoi","3":"2020-07-20","4":"123","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-07-20","4":"178","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-20","4":"102","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-21","4":"110","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-07-21","4":"86","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-07-21","4":"137","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-21","4":"339","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-21","4":"21","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-07-21","4":"21","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-21","4":"7","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-22","4":"76","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-22","4":"351","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-07-22","4":"655","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-22","4":"23","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-23","4":"129","5":"grams","6":"TRUE"},{"1":"carrots","2":"King Midas","3":"2020-07-23","4":"56","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-07-23","4":"466","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-23","4":"91","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-23","4":"130","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-24","4":"525","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-07-24","4":"31","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-24","4":"140","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-24","4":"1321","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-24","4":"100","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-07-24","4":"32","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-07-24","4":"93","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-24","4":"16","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-07-24","4":"3","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-07-24","4":"68","5":"grams","6":"TRUE"},{"1":"carrots","2":"King Midas","3":"2020-07-24","4":"178","5":"grams","6":"FALSE"},{"1":"carrots","2":"Dragon","3":"2020-07-24","4":"80","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-07-25","4":"463","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-07-25","4":"106","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-07-25","4":"121","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-25","4":"901","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-26","4":"81","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-26","4":"148","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-27","4":"1542","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-27","4":"728","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-07-27","4":"785","5":"grams","6":"FALSE"},{"1":"strawberries","2":"perrenial","3":"2020-07-27","4":"113","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-07-27","4":"29","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-27","4":"99","5":"grams","6":"FALSE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-27","4":"49","5":"grams","6":"TRUE"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-27","4":"149","5":"grams","6":"FALSE"},{"1":"radish","2":"Garden Party Mix","3":"2020-07-27","4":"39","5":"grams","6":"TRUE"},{"1":"carrots","2":"King Midas","3":"2020-07-27","4":"174","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-27","4":"129","5":"grams","6":"FALSE"},{"1":"broccoli","2":"Yod Fah","3":"2020-07-27","4":"372","5":"grams","6":"FALSE"},{"1":"carrots","2":"King Midas","3":"2020-07-28","4":"160","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-07-28","4":"203","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-28","4":"312","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-07-28","4":"131","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-28","4":"91","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-28","4":"76","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-29","4":"153","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-29","4":"442","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-29","4":"240","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-07-29","4":"209","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-29","4":"73","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-07-29","4":"40","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-29","4":"457","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-29","4":"514","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-29","4":"305","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-07-29","4":"280","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-07-30","4":"91","5":"grams","6":"FALSE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-30","4":"101","5":"grams","6":"TRUE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-30","4":"19","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-30","4":"94","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams","6":"TRUE"},{"1":"carrots","2":"King Midas","3":"2020-07-30","4":"107","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-30","4":"626","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-31","4":"307","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-07-31","4":"197","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-07-31","4":"633","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-31","4":"290","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-07-31","4":"100","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-07-31","4":"1215","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-31","4":"592","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-07-31","4":"23","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-07-31","4":"31","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-31","4":"107","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-07-31","4":"174","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-01","4":"435","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-01","4":"320","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-01","4":"619","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-01","4":"97","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-01","4":"436","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-01","4":"168","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-01","4":"164","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-01","4":"1130","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-01","4":"137","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-08-01","4":"74","5":"grams","6":"FALSE"},{"1":"cilantro","2":"cilantro","3":"2020-08-01","4":"17","5":"grams","6":"FALSE"},{"1":"onions","2":"Delicious Duo","3":"2020-08-01","4":"182","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-02","4":"1175","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-02","4":"509","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-02","4":"857","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-02","4":"336","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-02","4":"156","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-02","4":"211","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-02","4":"102","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-03","4":"308","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-03","4":"252","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-03","4":"1155","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-03","4":"572","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-03","4":"65","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-08-03","4":"383","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-04","4":"387","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-04","4":"231","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-04","4":"339","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-04","4":"118","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-08-04","4":"270","5":"grams","6":"TRUE"},{"1":"jalapeño","2":"giant","3":"2020-08-04","4":"162","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-04","4":"56","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-08-04","4":"192","5":"grams","6":"TRUE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-08-04","4":"195","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-08-04","4":"81","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-08-04","4":"87","5":"grams","6":"FALSE"},{"1":"hot peppers","2":"thai","3":"2020-08-04","4":"24","5":"grams","6":"FALSE"},{"1":"hot peppers","2":"variety","3":"2020-08-04","4":"40","5":"grams","6":"TRUE"},{"1":"spinach","2":"Catalina","3":"2020-08-04","4":"44","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-04","4":"427","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-05","4":"563","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-05","4":"290","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-05","4":"781","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-05","4":"223","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-05","4":"382","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-05","4":"217","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-05","4":"67","5":"grams","6":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-05","4":"234","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-06","4":"393","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-06","4":"307","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-06","4":"175","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-06","4":"303","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-08-06","4":"127","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-06","4":"98","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-08-06","4":"164","5":"grams","6":"TRUE"},{"1":"carrots","2":"Dragon","3":"2020-08-06","4":"442","5":"grams","6":"FALSE"},{"1":"potatoes","2":"purple","3":"2020-08-06","4":"317","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-08-06","4":"439","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-07","4":"359","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-07","4":"356","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-07","4":"233","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-07","4":"364","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-07","4":"1045","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-07","4":"562","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-07","4":"292","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-07","4":"1219","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-07","4":"1327","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-08-07","4":"255","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-07","4":"19","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-08","4":"162","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-08","4":"81","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-08","4":"564","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-08","4":"184","5":"grams","6":"TRUE"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-08","4":"108","5":"grams","6":"FALSE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-08","4":"122","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-08-08","4":"1697","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-08","4":"545","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-08","4":"445","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-08-08","4":"305","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-09","4":"179","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-09","4":"591","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-09","4":"1102","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-09","4":"308","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-09","4":"54","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-09","4":"64","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-09","4":"443","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-08-09","4":"118","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-08-09","4":"302","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-10","4":"13","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-08-10","4":"272","5":"grams","6":"FALSE"},{"1":"potatoes","2":"purple","3":"2020-08-10","4":"168","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-10","4":"216","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-10","4":"241","5":"grams","6":"TRUE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-08-10","4":"309","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-08-10","4":"221","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-11","4":"731","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-11","4":"302","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-11","4":"307","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-11","4":"160","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-11","4":"755","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-08-11","4":"1029","5":"grams","6":"FALSE"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-11","4":"78","5":"grams","6":"FALSE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-11","4":"245","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-11","4":"218","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-11","4":"802","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-11","4":"354","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-11","4":"359","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-11","4":"506","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-11","4":"92","5":"grams","6":"FALSE"},{"1":"edamame","2":"edamame","3":"2020-08-11","4":"109","5":"grams","6":"FALSE"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-11","4":"330","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-12","4":"73","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-13","4":"1774","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-13","4":"468","5":"grams","6":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-13","4":"122","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-13","4":"421","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-13","4":"332","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-13","4":"727","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-13","4":"642","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-13","4":"413","5":"grams","6":"FALSE"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-13","4":"65","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-13","4":"599","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-13","4":"12","5":"grams","6":"FALSE"},{"1":"beets","2":"Sweet Merlin","3":"2020-08-13","4":"198","5":"grams","6":"TRUE"},{"1":"beets","2":"Gourmet Golden","3":"2020-08-13","4":"308","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-08-13","4":"517","5":"grams","6":"FALSE"},{"1":"beets","2":"Sweet Merlin","3":"2020-08-13","4":"2209","5":"grams","6":"TRUE"},{"1":"beets","2":"Gourmet Golden","3":"2020-08-13","4":"2476","5":"grams","6":"FALSE"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-14","4":"1564","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-14","4":"80","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-14","4":"711","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-14","4":"238","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-14","4":"525","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-14","4":"181","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-14","4":"266","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-14","4":"490","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-14","4":"126","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-14","4":"371","5":"grams","6":"FALSE"},{"1":"corn","2":"Golden Bantam","3":"2020-08-15","4":"383","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-15","4":"351","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-15","4":"859","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-15","4":"25","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-08-15","4":"137","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-08-15","4":"71","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-15","4":"56","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-16","4":"477","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-16","4":"328","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-16","4":"45","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-16","4":"543","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-16","4":"599","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-16","4":"560","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-16","4":"291","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-16","4":"238","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-16","4":"397","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-16","4":"660","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-16","4":"693","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-17","4":"364","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-17","4":"305","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-17","4":"588","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-17","4":"764","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-17","4":"436","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-17","4":"306","5":"grams","6":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-17","4":"350","5":"grams","6":"TRUE"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-17","4":"105","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-08-17","4":"30","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-17","4":"67","5":"grams","6":"FALSE"},{"1":"corn","2":"Golden Bantam","3":"2020-08-17","4":"344","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-08-17","4":"173","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-18","4":"27","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-08-18","4":"126","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-08-18","4":"112","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-18","4":"1151","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-18","4":"225","5":"grams","6":"TRUE"},{"1":"cucumbers","2":"pickling","3":"2020-08-18","4":"2888","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-18","4":"608","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-18","4":"136","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-18","4":"148","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-18","4":"317","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-18","4":"105","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-18","4":"271","5":"grams","6":"FALSE"},{"1":"spinach","2":"Catalina","3":"2020-08-18","4":"39","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-18","4":"87","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-18","4":"233","5":"grams","6":"FALSE"},{"1":"edamame","2":"edamame","3":"2020-08-18","4":"527","5":"grams","6":"FALSE"},{"1":"potatoes","2":"purple","3":"2020-08-19","4":"323","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-08-19","4":"278","5":"grams","6":"FALSE"},{"1":"hot peppers","2":"thai","3":"2020-08-19","4":"31","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-19","4":"872","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-19","4":"579","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-19","4":"615","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-19","4":"997","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-19","4":"335","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-19","4":"264","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-19","4":"451","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-19","4":"306","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-20","4":"99","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-20","4":"70","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-20","4":"333","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-20","4":"483","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-20","4":"632","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-20","4":"360","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-20","4":"230","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-20","4":"344","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-20","4":"1010","5":"grams","6":"FALSE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-20","4":"328","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-20","4":"287","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Tatsoi","3":"2020-08-20","4":"322","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-20","4":"493","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-08-20","4":"252","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-08-20","4":"70","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-20","4":"834","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-08-20","4":"113","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-21","4":"1122","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-21","4":"34","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-08-21","4":"509","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-21","4":"1601","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-21","4":"842","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-21","4":"1538","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-21","4":"428","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-21","4":"243","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-21","4":"330","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-21","4":"997","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-21","4":"265","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-21","4":"562","5":"grams","6":"TRUE"},{"1":"carrots","2":"Dragon","3":"2020-08-21","4":"457","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-23","4":"1542","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-23","4":"801","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-23","4":"436","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-23","4":"747","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-23","4":"1573","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-23","4":"704","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-23","4":"446","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-23","4":"269","5":"grams","6":"FALSE"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-23","4":"661","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-23","4":"2436","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-23","4":"111","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-24","4":"134","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-08-24","4":"115","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-24","4":"75","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-08-24","4":"117","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-25","4":"578","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-25","4":"871","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-25","4":"115","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-25","4":"629","5":"grams","6":"FALSE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-25","4":"186","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-08-25","4":"320","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-25","4":"488","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-25","4":"506","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-25","4":"920","5":"grams","6":"FALSE"},{"1":"cucumbers","2":"pickling","3":"2020-08-25","4":"179","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-25","4":"1400","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-25","4":"993","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-25","4":"1026","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-26","4":"1886","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-26","4":"666","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-26","4":"1042","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-26","4":"593","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-26","4":"216","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-26","4":"309","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-26","4":"497","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-26","4":"261","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-26","4":"819","5":"grams","6":"FALSE"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-26","4":"1607","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-27","4":"14","5":"grams","6":"FALSE"},{"1":"raspberries","2":"perrenial","3":"2020-08-28","4":"29","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-08-28","4":"3244","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-08-28","4":"85","5":"grams","6":"FALSE"},{"1":"basil","2":"Isle of Naxos","3":"2020-08-29","4":"24","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-08-29","4":"289","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-08-29","4":"380","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-29","4":"737","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-08-29","4":"1033","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-29","4":"1097","5":"grams","6":"TRUE"},{"1":"edamame","2":"edamame","3":"2020-08-29","4":"483","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-08-29","4":"627","5":"grams","6":"TRUE"},{"1":"jalapeño","2":"giant","3":"2020-08-29","4":"352","5":"grams","6":"FALSE"},{"1":"potatoes","2":"purple","3":"2020-08-29","4":"262","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-08-29","4":"716","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-08-29","4":"888","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-29","4":"566","5":"grams","6":"TRUE"},{"1":"carrots","2":"greens","3":"2020-08-29","4":"169","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-08-30","4":"861","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-30","4":"460","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-08-30","4":"2934","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-08-30","4":"599","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-08-30","4":"155","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-30","4":"822","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-08-30","4":"589","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-08-30","4":"393","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-08-30","4":"752","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-08-30","4":"833","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-09-01","4":"2831","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-01","4":"1953","5":"grams","6":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"2020-09-01","4":"160","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"saved","3":"2020-09-01","4":"4758","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-09-01","4":"2342","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-09-01","4":"3227","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-09-01","4":"5150","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Old German","3":"2020-09-01","4":"805","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-09-01","4":"178","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-09-01","4":"201","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-01","4":"1537","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Jet Star","3":"2020-09-01","4":"773","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-01","4":"1202","5":"grams","6":"TRUE"},{"1":"corn","2":"Dorinny Sweet","3":"2020-09-02","4":"798","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-09-02","4":"370","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-09-02","4":"43","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-09-02","4":"60","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-09-03","4":"1131","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-03","4":"610","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-09-03","4":"1265","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-09-03","4":"102","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-04","4":"2160","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-04","4":"2899","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-09-04","4":"442","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-04","4":"1234","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-09-04","4":"1178","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-04","4":"255","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-09-04","4":"430","5":"grams","6":"FALSE"},{"1":"onions","2":"Delicious Duo","3":"2020-09-04","4":"33","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-09-04","4":"256","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-09-04","4":"58","5":"grams","6":"FALSE"},{"1":"corn","2":"Dorinny Sweet","3":"2020-09-05","4":"214","5":"grams","6":"FALSE"},{"1":"edamame","2":"edamame","3":"2020-09-05","4":"1644","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-06","4":"2377","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-09-06","4":"710","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-06","4":"1317","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-09-06","4":"1649","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-09-06","4":"615","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-09-07","4":"3284","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-09-08","4":"1300","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-09-09","4":"843","5":"grams","6":"FALSE"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-09","4":"102","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-09-09","4":"228","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-10","4":"692","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-09-10","4":"674","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-10","4":"1392","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-10","4":"316","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-09-10","4":"754","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-10","4":"413","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-09-10","4":"509","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-09-12","4":"108","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-09-15","4":"258","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-15","4":"725","5":"grams","6":"TRUE"},{"1":"potatoes","2":"Russet","3":"2020-09-16","4":"629","5":"grams","6":"FALSE"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-16","4":"219","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-09-16","4":"8","5":"grams","6":"FALSE"},{"1":"carrots","2":"King Midas","3":"2020-09-17","4":"160","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-09-17","4":"168","5":"grams","6":"TRUE"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"2020-09-17","4":"191","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-17","4":"212","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Brandywine","3":"2020-09-18","4":"714","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-18","4":"228","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-18","4":"670","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-09-18","4":"1052","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-09-18","4":"1631","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-09-18","4":"137","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-19","4":"2934","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-09-19","4":"304","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-09-19","4":"1058","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"307","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"397","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"537","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"314","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"494","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"484","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"454","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"480","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"252","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"294","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"437","5":"grams","6":"FALSE"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams","6":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1655","5":"grams","6":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1927","5":"grams","6":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1558","5":"grams","6":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1183","5":"grams","6":"TRUE"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1178","5":"grams","6":"FALSE"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"706","5":"grams","6":"FALSE"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1686","5":"grams","6":"FALSE"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1785","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-09-19","4":"1923","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-09-19","4":"2120","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-09-19","4":"2325","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-09-19","4":"1172","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-19","4":"1311","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-19","4":"6250","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"saved","3":"2020-09-19","4":"1154","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-09-19","4":"1208","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-09-19","4":"2882","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-09-19","4":"2689","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-09-19","4":"3441","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-09-19","4":"7050","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1028","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1131","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1302","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1570","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1359","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1608","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"2277","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1743","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"2931","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-09-20","4":"163","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-09-21","4":"714","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-21","4":"95","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-09-25","4":"477","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-25","4":"2738","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Black Krim","3":"2020-09-25","4":"236","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-09-25","4":"1823","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-09-25","4":"819","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-25","4":"2006","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-09-25","4":"659","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-25","4":"1239","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-25","4":"1978","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-09-25","4":"28","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-09-25","4":"24","5":"grams","6":"FALSE"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-25","4":"75","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-09-25","4":"84","5":"grams","6":"TRUE"},{"1":"apple","2":"unknown","3":"2020-09-26","4":"156","5":"grams","6":"FALSE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-09-26","4":"95","5":"grams","6":"FALSE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-09-27","4":"94","5":"grams","6":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"2020-09-27","4":"81","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-09-27","4":"139","5":"grams","6":"FALSE"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-27","4":"134","5":"grams","6":"FALSE"},{"1":"carrots","2":"Dragon","3":"2020-09-27","4":"883","5":"grams","6":"FALSE"},{"1":"carrots","2":"Bolero","3":"2020-09-27","4":"449","5":"grams","6":"TRUE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-09-27","4":"232","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-09-30","4":"88","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-09-30","4":"92","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-09-30","4":"1447","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Better Boy","3":"2020-09-30","4":"494","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"grape","3":"2020-09-30","4":"678","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-09-30","4":"70","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-09-30","4":"327","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-10-01","4":"127","5":"grams","6":"FALSE"},{"1":"potatoes","2":"Russet","3":"2020-10-02","4":"1596","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-10-02","4":"101","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-10-02","4":"145","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-10-03","4":"252","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-03","4":"213","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-10-03","4":"346","5":"grams","6":"TRUE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-10-04","4":"39","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-07","4":"254","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Old German","3":"2020-10-07","4":"363","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-10-07","4":"715","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-10-07","4":"272","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-10-07","4":"64","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-10-07","4":"17","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-10-07","4":"169","5":"grams","6":"FALSE"},{"1":"potatoes","2":"Russet","3":"2020-10-08","4":"372","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-10-08","4":"436","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"grape","3":"2020-10-10","4":"1377","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-10-10","4":"1977","5":"grams","6":"TRUE"},{"1":"jalapeño","2":"giant","3":"2020-10-10","4":"258","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-10-10","4":"23","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-10-11","4":"2478","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-11","4":"200","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Black Krim","3":"2020-10-11","4":"375","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Big Beef","3":"2020-10-11","4":"316","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-10-11","4":"898","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-10-11","4":"526","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Bonny Best","3":"2020-10-11","4":"386","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"volunteers","3":"2020-10-11","4":"230","5":"grams","6":"TRUE"},{"1":"peppers","2":"variety","3":"2020-10-11","4":"84","5":"grams","6":"TRUE"},{"1":"jalapeño","2":"giant","3":"2020-10-11","4":"119","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-10-11","4":"144","5":"grams","6":"FALSE"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-10-11","4":"437","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-10-12","4":"1031","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-10-12","4":"2322","5":"grams","6":"FALSE"},{"1":"squash","2":"Red Kuri","3":"2020-10-12","4":"296","5":"grams","6":"FALSE"},{"1":"squash","2":"delicata","3":"2020-10-12","4":"312","5":"grams","6":"FALSE"},{"1":"squash","2":"Waltham Butternut","3":"2020-10-12","4":"709","5":"grams","6":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"2020-10-12","4":"2143","5":"grams","6":"TRUE"},{"1":"squash","2":"Red Kuri","3":"2020-10-12","4":"1950","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-10-12","4":"1291","5":"grams","6":"FALSE"},{"1":"squash","2":"Blue (saved)","3":"2020-10-12","4":"1627","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-10-12","4":"4372","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"saved","3":"2020-10-12","4":"5000","5":"grams","6":"FALSE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-10-12","4":"2990","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-10-12","4":"1300","5":"grams","6":"TRUE"},{"1":"squash","2":"Red Kuri","3":"2020-10-12","4":"2710","5":"grams","6":"FALSE"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-10-12","4":"137","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-10-14","4":"859","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Big Beef","3":"2020-10-14","4":"791","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Amish Paste","3":"2020-10-14","4":"1175","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Brandywine","3":"2020-10-14","4":"418","5":"grams","6":"FALSE"},{"1":"tomatoes","2":"Old German","3":"2020-10-14","4":"484","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-10-14","4":"219","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-10-14","4":"646","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-10-14","4":"2838","5":"grams","6":"TRUE"},{"1":"carrots","2":"Bolero","3":"2020-10-14","4":"1500","5":"grams","6":"TRUE"},{"1":"carrots","2":"King Midas","3":"2020-10-14","4":"1023","5":"grams","6":"FALSE"},{"1":"peppers","2":"green","3":"2020-10-14","4":"328","5":"grams","6":"FALSE"},{"1":"jalapeño","2":"giant","3":"2020-10-14","4":"175","5":"grams","6":"FALSE"},{"1":"peppers","2":"variety","3":"2020-10-14","4":"89","5":"grams","6":"TRUE"},{"1":"zucchini","2":"Romanesco","3":"2020-10-15","4":"3800","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-10-15","4":"5700","5":"grams","6":"FALSE"},{"1":"zucchini","2":"Romanesco","3":"2020-10-15","4":"3600","5":"grams","6":"FALSE"},{"1":"potatoes","2":"Russet","3":"2020-10-15","4":"1527","5":"grams","6":"FALSE"},{"1":"potatoes","2":"yellow","3":"2020-10-15","4":"272","5":"grams","6":"FALSE"},{"1":"potatoes","2":"red","3":"2020-10-15","4":"1718","5":"grams","6":"FALSE"},{"1":"potatoes","2":"purple","3":"2020-10-15","4":"295","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-16","4":"883","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-16","4":"740","5":"grams","6":"FALSE"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-10-17","4":"310","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-17","4":"932","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-17","4":"1096","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-17","4":"1101","5":"grams","6":"FALSE"},{"1":"potatoes","2":"red","3":"2020-10-17","4":"293","5":"grams","6":"FALSE"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-10-17","4":"183","5":"grams","6":"FALSE"},{"1":"onions","2":"Delicious Duo","3":"2020-10-17","4":"77","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"2001","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"673","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"144","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"366","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"1393","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"903","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"419","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"1026","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"1350","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"297","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"52","5":"grams","6":"FALSE"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-18","4":"114","5":"grams","6":"FALSE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){width="30%"}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){width="30%"}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usual, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

**Event = Each Trip **

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>%
  ggplot(aes(x = sdate)) +
  #geom_histogram() +
  geom_density() + 
  
  labs(title = "Usage of bike-rentals in Washington, DC",
       subtitle = "The count of usage in the last quarter of 2014",
       x = "",
       y = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(time_hour = hour(sdate),
         time_minute = minute(sdate),
         minute_to_hour = time_minute/60,
         time_hour_minute = time_hour + minute_to_hour) %>% 
  ggplot(aes(x = time_hour_minute)) +
  geom_density() + 
  
  labs(title = "Usage of bike-rentals throughout the day",
       subtitle = "Using 24-hour time keeping",
       x = "",
       y = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate, label=TRUE)) %>% 
  ggplot(aes(y = day_of_week)) +
  geom_bar() +
  
  labs(title = "Bike-rental usage throughout the week",
       subtitle = "Based on 2014 bike-rental user data",
       y = "", 
       x = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  
  __There is an increase of usage during the work week spiking from 7am or 8am, which corresponds to the time before the workday, and another spike around 5pm and 6pm, which are typical for end time and rush hours. Sunday and Saturday, the non-work days reflect people sleeping in later and taking bikes after 10am.__
  
  

```r
Trips %>% 
  mutate(time_hour = hour(sdate),
         time_minute = minute(sdate),
         minute_to_hour = time_minute/60,
         time_hour_minute = time_hour + minute_to_hour) %>%
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 

  
  ggplot(aes(x = time_hour_minute)) +
  geom_density() +
  facet_wrap(vars(day_of_week)) +
  
  labs(title = "The differences in bike-rental usage throughout the week", 
       y = "",
       x = "Time of Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(time_hour = hour(sdate),
         time_minute = minute(sdate),
         minute_to_hour = time_minute/60,
         time_hour_minute = time_hour + minute_to_hour) %>%
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 

  
  ggplot(aes(x = time_hour_minute)) +
  geom_density(aes(fill = client), alpha = .5, color = NA) +
  facet_wrap(vars(day_of_week)) + 
  
  labs(title = "The different type of bike-rental client usage patterns ",
       subtitle = "Comparing casual vs registered clients in terms of volume and timing",
       y = "",
       x = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  
  __The stack function confuses the "reader" about the story of clients using bicycles. The non-stacked graph is easier to compare the two different types of riders' patterns. The casual rider appears outside of the start and end periods of time for 9-5 workers. The stacked one uses the registered clients as the "x-axis" to then plot on top the casual client's patterns. If the reader understands that, they can see how similar or dissimilar the casual clients' habits are.__
  

```r
Trips %>% 
  mutate(time_hour = hour(sdate),
         time_minute = minute(sdate),
         minute_to_hour = time_minute/60,
         time_hour_minute = time_hour + minute_to_hour) %>%
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 

  
  ggplot(aes(x = time_hour_minute)) +
  geom_density(aes(fill = client), alpha = .5, color = NA,
               position = position_stack()) +
  facet_wrap(vars(day_of_week)) +
  
  labs(title = "The different type of bike-rental client usage patterns ",
       subtitle = "Comparing casual vs registered clients in terms of volume and timing",
       y = "",
       x = "Registered clients are the x-axis")
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(time_hour = hour(sdate),
         time_minute = minute(sdate),
         minute_to_hour = time_minute/60,
         time_hour_minute = time_hour + minute_to_hour) %>%
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 
  mutate(weekend = ifelse(day_of_week == "Sat"| day_of_week == "Sun", yes = "Weekend", no = "Weekday")) %>% 

  ggplot(aes(x = time_hour_minute)) +
  geom_density() +
  facet_wrap(vars(weekend)) +
  
  labs(title = "Weekend vs Weekday Bike-Rental Usage",
       subtitle = "Comparing the volume of users renting bikes and time of day",
       y = "",
       x = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  
  __This graph highlights both the differences just in terms of overall usage depending on the weekend/weekday as well as disaggregating the information so that the reader can understand quickly the differences between the registered and casual clients. I think this graph is better than the other one because it tells a more complete story.__
  

```r
Trips %>% 
  mutate(time_hour = hour(sdate),
         time_minute = minute(sdate),
         minute_to_hour = time_minute/60,
         time_hour_minute = time_hour + minute_to_hour) %>%
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 
  mutate(weekend = ifelse(day_of_week == "Sat"| day_of_week == "Sun", yes = "Weekend", no = "Weekday")) %>% 

  ggplot(aes(x = time_hour_minute)) +
  geom_density(aes(fill = weekend), alpha = .5, color = NA) +
  facet_wrap(vars(client)) +
#fill with weekday?
  
  labs(title = "Comparing Bike-Rental Client Usage",
       subtitle = "The number of riders as shown throughout the time of day",
       y = "",
       x = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Trips %>% 
  group_by(sstation) %>% 
  summarize(sum_station = n()) %>% 
  left_join(Stations,
            by = c("sstation" = "name")) %>% 
  
  ggplot(aes(x = long,
              y = lat)) +
  geom_point() + 
  
  labs(title = "Map of the DC Bike Rental Stations",
       subtitle = "The points show each stations' coordinates",
       x = "",
       y = "",
       caption = "Better maps to come...")
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  
  __Most clients are registered! Those who are casual users tend to be in the furthest station location.__ **update: once I left the "small" dataset size, I no longer see the casual users at all. Every station is used more by registered clients, it seems.**
  

```r
Trips %>% 
  group_by(sstation, client) %>% 
  summarize(sum_station = n()) %>% 
  left_join(Stations,
            by = c("sstation" = "name")) %>% 
  ggplot(aes(x = long,
              y = lat)) +
  geom_point(aes(color = client)) + 
  
  labs(title = "Station Locations and the Type of Client Who Utilizes Them", 
       subtitle = "Each point represesnts a station's coordinates",
       x= "",
       y = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).

  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.


```r
breed_traits_total <- breed_traits %>% 
  select(-c(`Coat Type`:`Coat Length`)) %>% 
  pivot_longer(cols = !Breed, #!Breed means everything BUT Breed
               names_to = "Evaluation Type",
               values_to = "Rank") %>% 
  group_by(Breed) %>% 
  summarize(total_raiting = sum(Rank))
  

breed_traits_total %>% 
  filter(total_raiting > 0) %>% 
 
  ggplot(aes(y = fct_reorder(Breed, total_raiting),
             x = total_raiting)) + 
  geom_point() +
  
  labs(title = "Dog Breed Ratings",
       subtitle = "Overall ratings of different dog breeds",
       caption = "Who's a good boy?",
       y = "",
       x = "")
```

![](03_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest numer, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  

```r
breed_rank_all %>% 
  pivot_longer(cols = `2013 Rank`:`2020 Rank`,
               names_to = "year",
               values_to = "rank") %>% 
  separate(year,
           into = c("year"),
           extra = "drop",
           convert = TRUE) %>% 
  mutate(squishy_dog = str_squish(Breed)) %>% 
  inner_join(breed_traits_total %>% 
              slice_max(n = 20, order_by = total_raiting) %>% 
              mutate(squishy_dog = str_squish(Breed)),
             by = "squishy_dog") %>% 
 
  
   ggplot(aes(x = year,
             y = fct_rev(fct_reorder(squishy_dog, rank, median)))) +
  geom_line() +
  geom_point(aes(color = rank)) + 
  
  theme(panel.grid = element_blank()) + 
  
  labs(title = "The Top Dog Tango", 
       subtitle = "A visualization of the top 20 dogs and how their ranks compare over the years", 
       y = "",
       x = "",
       caption = "Who will bow-WOW you the most and who is sleeping in the doghouse?")
```

![](03_exercises_files/figure-html/unnamed-chunk-18-1.png)<!-- -->
  
  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.
  

```r
silly_dog <- breed_rank_all %>%
   pivot_longer(cols = starts_with("20"),
               names_to = "year",
               values_to = "rank") %>% 
  separate(year,
           into = c("year"),
           extra = "drop",
           convert = TRUE) %>% 
  mutate(squishy_dog = str_squish(Breed)) %>% 
  inner_join(breed_traits %>%
              mutate(squishy_dog = str_squish(Breed)),
             by = "squishy_dog") %>% 
  mutate(doggo = str_detect(squishy_dog, "dog | Dog | dogs | Dogs")) 

silly_dog %>% 
  filter(`Playfulness Level` == 5) %>% 
  ggplot(aes(y = squishy_dog, 
             x = year, color = doggo)) +
  geom_line() + 
  geom_point() + 
  
  labs(title = "A silly chart for a real dog of a dog",
       subtitle = "Highest-level playful dog over the years",
       y = "",
       x = "",
       caption = "But also wanted to see how the dogs with the name 'dog' in their breed name fare in playfulness.")
```

![](03_exercises_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
