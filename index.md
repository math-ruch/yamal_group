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
## Accounting for inflation
As you can see from the following histogram, our dataset consists of a large variety of movies ranging from the end of the 19th century to nowadays. Since one of the goals of our study is to explore relationships between the box office revenue of a movie and other factors and without adjusting for inflation, the box office revenues of older movies may seem significantly lower than those of recent movies. Therefore, to make a fair comparison, we had to  adjust all box office revenues and all budgets for inflation.
<iframe src="assets/movie_release_year_hist.png" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
The adjust for inflation is done using the consumer price index formula : 
$\text{Adjusted Value} = \text{Original Value} \times \frac{\text{CPI in the Original Year}}{\text{CPI in the Current Year (2021)}}$
<iframe src="assets/inflation-plot.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
Now that we have our budgets and box office revenues adjusted for inflation, let’s dive in our dataset and see what we can extract from it !

-----------------

## 1. Explore the relationship between movie plot sentiment and movie success

The first stage of our analysis will take us to a point that is crucial for humans and which therefore probably influences the success of movies. We have carried out a sentiment analysis of movie plots to explore the relationship between the emotions expressed in the plots and the success of the films in terms of box office revenue. We know that human beings are fond of feelings. This is one of the reasons why so many films in the drama genre are produced, because they express human interaction. Let's start by explaining in a few words how our NLP pipeline works. 

For each film we tokenised its plot and labelled the sentences in the plot as being either positive, negative or neutral on the basis of an emotional score. Here's a small example with this wise quote from Frank the wizard:

*"<span style="color:green">I got up in a good mood. I had an excellent breakfast.</span> <span style="color:red">Unfortunately, I realised that I'd have to do the washing-up afterwards.</span>"*

The first two sentences are labelled as positive and therefore coloured in green because of their positive score assigned by our sentiment analyser and the last sentence in red is considered as negative.
Then all plots with a proportion of sentences associated with a sentiment greater than 50% are labelled as belonging to that sentiment. For example, if more than 50% of the sentences of a plot are negative, the plot would be considered negative. In the case of our previous example, the text would be labelled as positive because it contains two thirds of positive sentences. Movies with a proportion of sentiment of less than 50% in all categories are left out because they cannot be correctly labelled. 
Let's look at the result of this processing by visualising the number of films with dots appearing in each of the emotional categories for different genres:

<iframe id="image" src="assets/math_plot_count.html" width="750px" height="530px" frameborder="0" position="relative">Display plot counts</iframe>
However, in the case of observational analyses, we have to take account of important factors in our data which can influence box office revenues by category in different ways! These are the terrible confounders... In our analyses  we consider that the genre of a movie might have an effect on whether the plot is sentimental or not. For example, we would expect to have a higher number of emotional plots for drama and maybe only a few emotional plots for action movies. This may impact our analysis on box office revenues on the conclusion of our analysis as the two genres may not have similar distributions of box office revenues. 
 
The same applies to budgets: it is obvious that if we consider the money generated as output, we must care about the money at the input i.e. the budget as the later has probably a big impact on the revenues. A movie with a higher budget will generally reach a bigger audience (higher budget means it will probably lead to a greater marketing, be translated into more languages, have better special effect...) and consequently generate a higher box office revenue.

To get rid of this merciless enemy, we did some matching. In our analysis, we select the same number of movies with an emotional plot as with a non-emotional plot for each genre. To match the dataset on budgets, it is very unlikely that two movies have the exact same budget, so we will match movies whose diffence in budget is not more than 20% of the smallest budget between the two i.e. $\text{abs}(\text{budget movie}_1 - \text{budget movie}_2) < 0.2*min(\text{budget movie}_1, \text{budget movie}_2)$


But what about the results of our analysis? Here they are:

- For our first question on the link between box office income and plot emotionality (whether they are neutral or emotional) it turns out that, against all expectations, there is no statistically significant difference (significance level of 0.05) between the box office averages of the two categories! In fact, by performing a t-test between the two averages, we can see that the p-value is below the significance threshold. As can also be seen in the figure below, the 95% confidence intervals overlap considerably. We might have expected the emotion ingredient in the film plot to contribute to the potion of success. But because of the low p-value, we cannot refute the hypothesis that the two averages are similar.

<iframe id="image" src="assets/math_matched_exp1.html" width="750px" height="700px" frameborder="0" position="relative">Plot matched exp1</iframe>
- Regarding our second question, if the plot is considered to be emotional, it turns out that this time there is a statistically significant difference between the box office revenues of the films in the positive plot.

<iframe id="image" src="assets/math_matched_exp2.html" width="750px" height="700px" frameborder="0" position="relative">Plot matched exp2</iframe>

-----------------

## 2. What is the relationship between a movies' genre and its success?

...
-----------------

## 3. Does the period of release of a movie have an impact on its box office revenue?

...
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

## 6. Are longer movies more likely to be more successful?

...
-----------------

## 7. Are movies with higher budgets more successful?

...
-----------------

## 8. what is the relationship between a movies reviews from critics and it’s success?

...
-----------------

## Wrap Up

Let us now quickly recap the main results of this analysis to be ready to produce good movie generating a looooooot of money.
