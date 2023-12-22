---
layout: page
title: The Wizardry of Good Movies
subtitle: Analysis of magic spells that lead to high box office revenue
cover-img: "https://www.destructoid.com/wp-content/uploads/2023/09/Wizardry-PGMO_keyart_nologo.jpg"
thumbnail-img: /assets/img/movie.png
share-img: /assets/img/movie.png
use-site-title: true
---

## Abstract

The most expensive film ever produced is Star Wars: The Force Awakens, costing an estimated 447 million $ , but it ended up being a huge financial success grossing over 2 billion $. What explains this success? One could think that obvious features such as the budget of the film are the main contributors to its success, but this is not always the case. In fact, many other factors influence the success of the film. Our aim is to explore the connection between the films' success (which we measure by its revenue when we have enough data) and various less obvious factors such as the gender of the cast, the sentiment expressed in the plot etc. Our analysis will aim to find out which factors are the most important and play an important role in contributing to a movies' success.


Along this analysis, we wish to look at movies from different angles, using a whole range of data analysis tools. On our journey we will be using regression, NLP and sentiment analysis, graphs, to make sure to discover all little secrets hiding behind movies success. Let us now delve into the magic behind the success of movies to understand the composition of the magic potion that leads a film to a high box office revenue. But first, we must process a little bit the data before exploiting them. Indeed, certain values of our dataset need a small update.

-----------------
## Accounting for Inflation: Revenue and Budget

In any analysis concerning financial figures over time, such as movie box office revenues and budgets, it's crucial to consider the impact of inflation. Inflation erodes the value of money over time, meaning that a movie's box office revenue or budget from decades ago doesn't hold the same value as today. To compare the financial performance and investment across different eras fairly, we adjust these figures for inflation, ensuring a like-for-like comparison.

### Why Adjust for Inflation?

Without accounting for inflation, we risk misinterpreting the financial success of older films or underestimating the scale of investment in past productions. A blockbuster from the 1980s, for instance, might have had a tremendous box office success in its day, but when compared to modern revenues without adjustment, it may seem less impressive. Similarly, the budget of an old classic, when adjusted for today's value, might reveal it as a more significant investment than initially perceived. Adjusting for inflation allows us to appreciate the real financial scale and success of movies across different periods.

### Adjusting Box Office Revenue and Budget

To normalize the box office revenue and budget figures, we use the Consumer Price Index (CPI), which provides a consistent measure to adjust values over time. The adjusted revenue and budget provide a more accurate reflection of a movie's financial success and the investment put into it, respectively. The formula for adjustment is as follows:

![Equation](https://latex.codecogs.com/svg.image?\text{Adjusted&space;Value}=\text{Nominal&space;Value}\times\frac{\text{CPI&space;in&space;Target&space;Year}}{\text{CPI&space;in&space;Release&space;Year}})

Where:

- **Adjusted Value**: The revenue or budget value adjusted for inflation.
- **Nominal Value**: The reported revenue or budget at the time of the movie's release.
- **CPI in Target Year**: The CPI of the year to which we're adjusting the value (commonly the current year).
- **CPI in Release Year**: The CPI of the year in which the movie was released.

By applying this formula to both the box office revenue and the production budget, we can analyze the real financial performance and investment in movies on a comparable basis, regardless of their release year.

-----------------

## 1. Explore the relationship between movie plot sentiment and movie success

The last stage of our analysis will take us to a point that is crucial for humans and which therefore probably influences the success of movies. We have carried out a sentiment analysis of movie plots to explore the relationship between the emotions expressed in the plots and the success of the films in terms of box office revenue. We know that human beings are fond of feelings. This is one of the reasons why so many films in the drama genre are produced, because they express human interaction. Let's start by explaining in a few words how our NLP pipeline works. 

For each film we tokenised its plot and labelled the sentences in the plot as being either positive, negative or neutral on the basis of an emotional score. Here's a small example with this wise quote from Frank the wizard:

*"<span style="color:green">I got up in a good mood. I had a good night. I had an excellent breakfast.</span> <span style="color:red">Unfortunately, I realised that I'd have to do the washing-up afterwards.</span><span style="color:blue"> I would have liked to use a dishwasher"</span>*

The first three sentences are labelled as positive because of their positive score assigned by our sentiment analyser. Therefore they are coloured in green. On the contrary, the forth sentence in red is labelled as negative by the analyser and the last one is considered as neutral. This is why they appear in red and blue, respectively.

Then we classified plots as follows. All plots containing more than 50% sentences associated with a particular sentiment are labelled as belonging to that sentiment. For example, if more than 50% of the sentences of a plot are negative, the plot will be considered negative. In the case of our previous example, the text would be labelled as positive because it contains three of positive sentences over five. Movies with a proportion of sentiment of less than 50% in all categories are left out because they cannot be correctly labelled. 
Let's look at the result of this processing by visualising the number of films with plots appearing in each of the emotional categories for different genres:

<iframe id="image" src="assets/math_plot_count.html" width="750px" height="520px" frameborder="0" position="relative">Display plot counts</iframe>
Now that the data is available, we can ask our research questions to address them:

- **1)** Are films with emotional plots (positive or negative) more successful than films with neutral plots in terms of box office revenue?
- **2)** Focusing on movies with emotional plot, what emotion (between positive and negative) in the plot makes the movie more successful?

### Naive analysis

A naive approach to answer our questions on the impact of sentiment in movie plots on box office revenues would be the following: we could simply compare the box office revenue averages for each plot category: emotional vs neutral and positive vs negative. This can be done using a t-test. Let's look at what this would give us:

<iframe id="image" src="assets/Math_naive_exp.html" width="750px" height="520px" frameborder="0" position="relative">Display naive exp</iframe>
Looking at the plot above we observe that in both cases, the confidence intervals of box office revenues are not overlapping whether looking to the left bar plots for question **1)** or to the right bar plots for question **2)**. In addition, the p-values of t-test are the following: 

- p-value = 0.0202 for question **1)** comparing mean box office revenues between emotional versus non-emotional plot
- p-value = 9.18e-08 for question **2)** comparing mean box office revenues between positive and negative emotional plot

So, in both cases the difference of mean box office revenues is statistically significant (at a significance level of 0.05) as both p-values are smaller than the significance level. For **1)**, if we look at the value of the t-statistic or simply observe the plot above, we can see that films with emotional plots have a higher average box office revenue than neutral plots! Thanks to the low p-value we can reject the fact that the mean box office revenues are the same for both categories. This is the same for question **2)**. As the p-value is below 0.05 we can conclude that the difference of mean box office revenues is statistically significant between movies with negative emotional plot and positive ones. Films with negative emotional plots have a higher average box office revenue than neutral plot.

### Matched experience

But are those conclusions trustworthy? **No!** In the case of observational analyses, we have to take account of important factors in our data which can influence box office revenues by category in different ways! These are the terrible confounders... In our analyses  we consider that the genre of a movie might have an effect on whether the plot is sentimental or not. For example, we would expect to have a higher number of emotional plots for drama and maybe only a few emotional plots for action movies. This may impact our analysis on box office revenues as the two genres may not have similar distributions of revenues. 
 
The same applies to budgets: it is obvious that if we consider the money generated as output, we must care about the money at the input i.e. the budget as the later has probably a big impact on the revenues. A movie with a higher budget will generally reach a bigger audience (higher budget means it will probably lead to a greater marketing, be translated into more languages, have better special effect...) and consequently generate a higher box office revenue.

To get rid of this merciless enemy, we did some matching. In our analysis, we select for each genre the same number of movies with an emotional plot as with a non-emotional plot for question **1)**. For question **2)** we also select for each genre the same number of movies with positive emotional plot as with negative plot. To match the dataset on budgets, it is very unlikely that two movies have the exact same budget. So, we match movies between the treatment and the control group whose diffence in budget is not more than 20% of the smallest budget between the two i.e.:
<img id="image" src="assets/equation.png" width="750px" height="40px" frameborder="0" position="relative">
To compare the box office revenues distributions, we performed t-test as before.

But what about the results of our analysis? Here they are:

- For question **1)** on the link between box office income and plot emotionality (whether they are neutral or emotional) it turns out that, against all expectations, there is no statistically significant difference (significance level of 0.05) between the box office averages of the two categories! In fact, by performing a t-test between the two averages, we can see that the p-value (0.402) is above the significance threshold. We also observe in the figure below that the 95% confidence intervals overlap considerably. We might have expected the emotion ingredient in movie plots to contribute to the potion of success. But because of the low p-value, we cannot refute the hypothesis that the two averages are similar unlike the naive analysis told us.

<iframe id="image" src="assets/Math_matched_exp1.html" width="750px" height="520px" frameborder="0" position="relative">Plot matched exp1</iframe>
- Regarding question **2)**, if the plot is considered to be emotional, it turns out that this time we can reject the null hypothesis stating that the two sets of movies box office revenues have identical means (at a significance level of .05) as the p-value (0.0324) is below the significance level. Based on the t-statistic or on the plot below, we can say that the mean of box office revenues for movies with a negative emotional plot is higher. This is interesting to observe that films with negative emotional plot generally convey a greater success looking at the box office revenues. One would probably have thought that humans would be more sensitive to movies with positive emotional plot rather than negative emotional ones. *"The heart has its reasons which reason knows not."* to quote the famous french mathematician Blaise Pascal.
<iframe id="image" src="assets/Math_matched_exp2_office.html" width="750px" height="520px" frameborder="0" position="relative">Plot matched exp2</iframe>

-----------------

## 2. Does the Period of Release of a Movie Have an Impact on Its Box Office Revenue?

In the spellbinding realm of cinema, where stories unfold and captivate audiences, the success of a film is often attributed to myriad factors, ranging from its narrative depth to the star power it wields. Yet, beyond these obvious charms lies a more subtle, often overlooked element that can significantly sway a film's journey to prosperity: its release period. The date a movie graces the silver screen might seem inconsequential at first glance, but our analysis delves deep into the temporal mysteries to uncover its potential impact on box office revenue.

Our initial foray into this enigmatic topic revealed a mild, though statistically significant, negative correlation between a movie's release year and its inflation-adjusted box office takings. This intriguing finding hints that the year of release, while not overwhelmingly dominant, does play a role in a film's financial outcome. However, to truly understand the nuances of this relationship, we must look beyond mere years and delve into the specifics of months and days.

Imagine the calendar as a grand stage, with each month and day holding its unique potential for cinematic success. We seek to decode this complex schedule, asking questions of utmost importance to filmmakers and producers alike: **"What is the most favorable month to launch a film to maximize box office returns? Are certain days of the week more auspicious for premieres?"** This section of our story embarks on a data-driven quest to unravel the temporal keys to box office triumph, endeavoring to discover if a golden slot exists within the calendar's pages that beckons larger audiences and swells the coffers of the box office.

Join us as we weave through the data, analyzing patterns and peculiarities, and perhaps, just perhaps, we'll uncover the hidden temporal magic that influences the financial success of movies. Whether it's the charm of holiday seasons, the allure of summer blockbusters, or the quiet appeal of off-peak releases, our story seeks to illuminate the subtle yet significant impact of release timing on a film's box office destiny. As we turn each page of the calendar, let's unveil the secrets held within the dates and determine if, indeed, the period of release holds the power to enchant audiences and fill the treasure chests of those who dare to dream in the world of cinema.

### Temporal Alchemy: The Relationship Between Release Months and Box Office Gold

As we delve into the enigmatic world of cinema, it's not just the storyline or star cast that carves the path to prosperity but also the strategic choice of the release date. To unveil this hidden influence, we embarked on a meticulous journey through data, beginning with an ANOVA test to dissect the year into months, each holding its own promise for cinematic success.

With our analytical wands, we aggregated box office revenues by month and invoked the powers of the `f_oneway` function from `scipy.stats`. The incantation revealed a significant F-statistic of 25.655 and a p-value whispering the tale of a strong statistical difference in average box office revenues across various months. This was our first clue, suggesting that the timing of a film's release is more than just a date on the calendar; it's a strategic decision that can sway financial outcomes significantly.

#### The Quest for the Optimal Release Month
Armed with this knowledge, we embarked on a detailed examination of each month's individual impact on movie revenues. Employing pairwise T-Tests, we contrasted each month against the collective performance of others, unveiling a granular view of monthly trends. This wasn't just a comparison; it was a quest to identify the most auspicious times for filmmakers to release their creations.

Our script transformed each month into a name, from the frosty whispers of January to the sun-kissed days of June, and everything in between. With each month mapped and data prepared, our T-Tests danced through the calendar, revealing the highs and lows of box office performance.

#### Visualizing Monthly Revenue Trends: Highlighting the Optimal Release Window

Following our statistical analysis, we now present a visual representation of the data that further illuminates our findings. The interactive graph below captures the essence of our results, emphasizing the temporal dimension of box office success.

<iframe id="image" src="assets/monthly_analysis.html" width="800px" height="400px" frameborder="0" position="relative">Display plot counts</iframe>

#### June and December: The Crowned Months of Cinema
The analysis crowned June as the prime month for movie releases, its golden hues reflecting the sun-drenched days of summer vacations when audiences flock to theaters seeking escape and entertainment. December, with its festive allure, beckoned families and friends to share moments of joy and togetherness in the cozy embrace of cinema halls.

These insights are more than mere observations; they are strategic tools for filmmakers and producers. Timing releases to coincide with major vacation periods can significantly enhance box office performance, turning a well-timed premiere into a financial triumph.

### Daily Dynamics: The Impact of Weekdays on Box Office Revenues

In the complex world of movie-making, the release date of a movie can be crucial to its success. Moving on from examining the impact of release months, we now narrow our focus to the days of the week. In this part of our analysis, we explore how the specific day a film is released may influence its performance at the box office for different genres.

#### Understanding Box Office Performance: Weekday and Genre Analysis

To understand how weekdays influence box office revenues, we first transformed the days of the week and movie genres into a numerical format. This transformation, known as one-hot encoding, allowed us to prepare the data for our analysis effectively. Next, we created a matrix showing the average box office revenue for each genre on every day of the week. This step was essential to identify potential patterns and determine which days might be more successful for different movie genres.

Moving further, we aimed to find the best day for releasing films in each genre. For this, we used two methods: Random Forest regression models and analysis of the mean revenue. When the results from these two methods differed, we relied on the mean revenue analysis, as it reflected the historical box office performance more accurately. This comprehensive approach enabled us to suggest the most strategic days for movie releases based on genre-specific audience behaviors.

#### Visualizing Box Office Trends: A Weekday and Genre Heatmap
We created a heatmap to visually present our findings, turning complex data into an easily understandable color spectrum. This heatmap illustrates the average box office earnings for each genre on different weekdays. With just a glance, we could identify which days typically bring in higher revenues for each genre.

We also added annotations to highlight the best days for releases. Where our Random Forest model and mean revenue analysis didn't agree, we marked the 'Best' day in red to indicate the discrepancy. This heatmap isn't just a simple chart; it's a comprehensive guide that helps us understand the patterns in box office earnings based on the day of the week and genre.

<iframe id="heatmap" src="assets/daily_analysis.html" width="800px" height="500px" frameborder="0" position="relative">Display plot counts</iframe>

#### The Daily Rhythm: Selecting the Best Release Days
Our analysis showed that the success of a movie at the box office can vary significantly depending on the day it's released. Different genres tend to perform better on specific weekdays, suggesting an underlying pattern in audience behavior. By combining rigorous data analysis with industry knowledge, we've identified which days might offer the best chance for a film's successful release, providing a valuable guide for planning premiere dates.

### Conclusion: Deciphering the Temporal Impact on Box Office Success
Our extensive exploration into the release timing of movies has revealed a complex yet discernible relationship between when a film is shown and its box office success. Initially, we observed subtle shifts in revenue patterns over time, suggesting changes in market dynamics and consumer behavior. Our further investigation into monthly trends identified June and December as standout periods for box office returns, likely due to the seasonal holidays that draw larger audiences to theaters.

We didn't stop at broader trends; our study also delved into the daily dynamics influencing box office outcomes. Through numerical transformation of our data and the application of both Random Forest models and mean revenue analysis, we uncovered the weekdays that favor different movie genres. The heatmap created from our findings not only visualized these intricate patterns but also pinpointed the optimal release days, providing a strategic guide for film releases.

In concluding our analysis, it's clear that the timing of a movie's release—spanning from the year to the specific day—significantly influences its commercial success. Our insights provide a valuable roadmap for industry professionals, indicating the best times to release films to maximize audience engagement and revenue. This comprehensive study not only illuminates the patterns behind cinematic success but also empowers filmmakers and distributors with actionable knowledge to strategically plan their premieres, ensuring that each film has the best opportunity to thrive in the competitive box office landscape.

-----------------

## 3. What is the relationship between a movies' genre and its success?


-----------------

## 4. Opting for a Single Oscar-Nominated Actor? No. Choosing Two or More? Yes.
As a movie producer, you might wonder: "Is it really worth it to hire big-name actors? and if so, how Manny should I hire ?" for your next block buster. Sure, they bring star power, but they also bump up your movie's budget. It's a classic dilemma — balancing the sparkle of famous faces with the realities of filmmaking costs.

First, we had to decide what a big-name actor is. There are several ways to do so, for the purpose of our study, we decided to use Oscar nominations as the benchmark. Essentially, if an actor has received an Oscar nomination at any point in their career, we categorize them as a big-name actor.

### The Sweet Spot
We grouped our movies into four categories based on the number of Oscar nominated actors that participated in the casting to find if there is any relationship between the number of nominated actors and the movie’s box office revenue. As you can see the four bar plots, we can observe that the more Oscar nominated actors you have in your casting, the better your movie will perform in the box office revenue 🤑. The non-overlapping confidence intervals between the no Oscar actor group and the one Oscar actor group clearly says that having one Oscar actor increases your mean box office revenue than having no Oscar actor at all.
<iframe src="assets/4-oscars-categories.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
But we are not dumb because we took the ADA class ! Our analysis so far has been naïve, not considering that many confounders may affect the box office revenue. To try to isolate the Oscar actor effect on revenue, we have performed a paired matching. By completing the paired matching, we try to compare movies with similar probability of receiving the treatment, which in our case is having at least one Oscar nominated actor. We used logistic regression to compute the propensity score based on the budget of the movie and the number of actors of the movie. We then perform the T-test for the mean box office revenues of two independent groups. This is a test for the null hypothesis that 2 independent samples have identical average (expected) values. Unfortunately we obtain a p-value of 0.42, which is greater than the rejection threshold of 0.05, which basically means that we cannot reject the null hypothesis that the two groups come from different distributions. Also, the confidence intervals between the two groups are clearly overlapping. We have to dig more deeply into our data !
<iframe src="assets/box_office_revenue_comparison_after_first_matching.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
Maybe let us modify a little bit the treatment assignment definition and re define the treatment by “having two or more Oscar nominated actors in your casting” and let us redo the exact same study to see if it changes anything. When re-doing the study, we are again separating the movies between a treatment and a control group where the treatment group consists of movies who have more than a single Oscar nominated actor in their cast. And guess what ? this time we got a 0 p-value and non-overlapping confidence intervals suggesting that having two or more Oscar nominated actors in your cast will really increase your chances of performing well in the box office !
<iframe src="assets/box_office_revenue_comparison_after_sec_matching.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>

## Who were the most important actors during the past five decades ?

Do you feel like you're always seeing the same actors, such as Brad Pitt, Emma Watson, and Robert De Niro, when you go watch blockbusters to the movies? Are certain actors more prominently featured in such films? This is what we are gonna explore, so let’s get started. 

We are going to run a little graph study to explore the links between actors. To do so, we are going to consider the set of actors as a social network where each node is an actor and where two actors are connected to each other if and only if they played together in a movie, then the strenght of the link between the two actors will be the mean of the box office revenues of the movies they played together.

Since we only wanted to consider blockbuster movies, we had to define what a blockbuster is. For this purpose, we only selected movies with box office revenues greater or equal to 90% of all other movies of our dataset. 

We then conduct a weighted degree centrality analysis on our graphs for each of the past five decades and sort the actors by their centrality measure, hoping that would give us some insights and some cool stuff to tell you . Let’s see what we get !

<button onclick="changeImage()">Next decade</button>
<iframe id="image" src="assets/actors_1970_1979.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>

<script>
var images = ["assets/actors_1970_1979.html", "assets/actors_1980_1989.html", "assets/actors_1990_1999.html", "assets/actors_2000_2009.html", "assets/actors_2010_2019.html"];
var currentImage = 0;

function changeImage() {
    currentImage++;
    if (currentImage >= images.length) currentImage = 0;
    document.getElementById('image').src = images[currentImage];
}
</script>

### The seventies : a deceny marked by an emblematic movie, The godfather.
Joe Spinell, Talia Shire, James Caan, Marlon Brando, Richard Bright, Diane Keaton, Al Pacino, Robert Duvall, John Cazale and  Peter Donat, these are the 10 most emblematic actors of the seventies. You may ask yourself what do all these actors have in common ? did they perform singularly each in a movie or do they have a common movie that kind of boosted their carreer. Well, just by having a quick look at their wikipedia page, we found out that 8 out of theim : Marlon Brando, Al Pacino, James Caan, Robert Duvall, John Cazale, Diane Keaton, Talia Shire, and Richard Bright all appeared in "The Godfather" (1972) directed by Francis Ford Coppola.

### The Eighties: The Era of "Star Wars" and "Back to the Future"

Again the eighties top actors fall into two groups :

The Star Wars cast where : 
Harrison Ford played Han Solo.
Billy Dee Williams portrayed Lando Calrissian.
Carrie Fisher was famous for her role as Princess Leia.
James Earl Jones provided the voice for Darth Vader.
Frank Oz voiced Yoda.
Mark Hamill starred as Luke Skywalker.

and the Back to the future cast where :
Christopher Lloyd played Dr. Emmett "Doc" Brown.
James Tolkan was known for his role as Mr. Strickland.
Frances Lee McCain played Stella Baines, Lorraine's mother.

### The early twenty first century : The era of fantasy movies
The early 21st century actors are known for their roles in iconic film series, like Harry Potter or the Lord of the rings which are celebrated for bringing the magical worlds of J.K. Rowling's and J.R.R. Tolkien's to life on the big screen.

"Harry Potter" Series Connection:

John Cleese played Nearly Headless Nick.
Robbie Coltrane portrayed Rubeus Hagrid.
Brendan Gleeson appeared as Alastor "Mad-Eye" Moody.
Gary Oldman was Sirius Black.
Alan Rickman played Severus Snape.
Warwick Davis took on multiple roles, including Professor Flitwick and Griphook.
Maggie Smith starred as Professor Minerva McGonagall.

"The Lord of the Rings" / "The Hobbit" Series Connection:

Hugo Weaving played Elrond.
Orlando Bloom portrayed Legolas.
Bruce Spence appeared in "The Lord of the Rings: The Return of the King" as the Mouth of Sauron (extended edition).

Note however that as this analysis might give us good insights on the importance of actors for the past five decades, the way we construct our graph gives the same importance to all kinf of actors and secondary actors might be considered as important as primary ones. We could have had done a better analysis by giving more weights to primary actors to primary actors but we did lack of data in order to do so.
## 6. Are longer movies more likely to be more successful or are they just more boring to watch?

As Benjamin Franklin once said "Remember that time is money", but how does this apply to the world of movies? 
Movie runtime, measured in minutes is not simply the duration of a movie but also an element that can affect the audience's interest and attention. As humans we dislike being under the impression that we are wasting our time, so this plays a big role in choosing how long a movie should be. In this part we will investigate the relationship between movie runtime and it's box office revenue in order to gage the influence of the movies runtime on it's success. So without wasting any time let us continue!

Starting off by looking at the scatter plot of movie runtimes and the movie revenues we might expect to see a clear relationship, but sadly we do not. But all hope is not lost! We will delve deeper into the analysis using statistical testing and linear regressions.

<iframe id="image" src="assets/scatterplot_runtime_revenue.html" width="1200" height="850" frameborder="0" position="relative">Scatter map</iframe>

### Using the magical power of linear regressions we investigate this question further

Analysing the results of our linear regression we find an R-squared value of 3.1%, meaning that movie runtime explains almost no variance in the movie revenue and with a p-value close to 0 it looks like move runtime is a poor predictor of our movies success.

When we are looking at continuous revenues, it appears that runtime does not explain well the variance in box office revenunes. But what about looking at 2 distinct revenue categories, do they have a significant difference in runtime? To accomplish this, we will be looking at block-buster movies (movies that have a revenue of over 400$ million) and non block-buster movies.

By using a student test and doing exact matching on the genres to mitigate an important confounder in our results we find that the average runtime for block-buster movies is significantly higher than for non-blockbuster movies, and our result is statistically significant due to the p-value being close to 0.

<iframe id="image" src="assets/Mean_runtime_blockbuster.html" width="800" height="530" frameborder="0" position="relative">Scatter map</iframe>

Despite having a low value of R-squared between movie runtime and revenue we were able to expose a significant difference in movie runtimes between block-busters and non block-busters. 
This underlines the nuanced role that timing plays on a movies success. As Gandalf wisely said, "A wizard arrives precisely when he means to."

-----------------

## 7. Are movies with higher budgets more successful?
Does investing more money into a film guarantee higher returns? Is this a myth or is this reality? A priori one would expect that investing more would correlate to higher box office revenues.
By doing a linear regression we find that the R-squared values is 0.165 meaning that 16.5% of the variance in box office revenue is explained by the movies budget! And the coefficient associated with the budget is positive indicating that an increase in budget leads to a direct increase in movie revenue. The associated p-value is also close to 0 indicating that our results are statistically relevant and not simply invented like some magical soup in Asterix and Obelix.

In the midst of all of this, one could wonder how profit evolves. Do movies with higher budgets also have higher profits. Is this true? 
It sure looks like it! We have done a student test and found that the average profit for movies with high budgets (meaning a budget above 40$ million) was indeed higher than movies with low budgets and the p-value was close to 0 indicating that the result is again statistically significant.
But wait! Each genre might lead to different amounts of profit being made even if the budget is the same! To avoid this we have employed exact matching in order to mitigate the genres as a potential confounder.

<iframe id="image" src="assets/Mean_net_profit_budget.html" width="800" height="530" frameborder="0" position="relative">Scatter map</iframe>
-----------------


## Wrap Up

Let us now quickly recap the main results of this analysis to be ready to produce good movie generating a looooooot of money.

From our last research question, it seems that there is no significant difference between the success of movies with emotional plot versus movies that have a non-emotional plot looking at the box office revenues. However, when movies plot starts to deal with emotions, it appears that movies with negative emotional plot encounter a greater success in terms of box office revenues than movies with positive emotional plot. So, when writing a plot, if you wish to increase your movie box office don’t forget to put negative emotions if the plot is emotional.
