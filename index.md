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

The most expensive film ever produced is Star Wars: The Force Awakens, costing an estimated 447 million $ , but it ended up being a huge financial success grossing over 2 billion $. What explains this success? One could think that obvious features such as the budget of the film are the main contributors to its success, but this is not always the case. In fact, many other factors influence the success of the film. Our aim is to explore the connection between the films' success (which we measure by its user ratings and revenue when we have enough data) and various less obvious factors such as the gender of the cast, where the film was released etc. Our analysis will aim to find out which factors are the most important and play an important role in contributing to a movies' success.

-----------------

## Accounting for inflation
As you can see from the following histogram, our dataset consists of a large variety of movies ranging from the end of the 19th century to nowadays. Since one of the goals of our study is to explore relationships between the box office revenue of a movie and other factors and without adjusting for inflation, the box office revenues of older movies may seem significantly lower than those of recent movies. Therefore, to make a fair comparison, we had to  adjust all box office revenues and all budgets for inflation.
<iframe src="assets/movie_release_year_hist.png" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
The adjust for inflation is done using the consumer price index formula : 
$ \text{Adjusted Value} = \text{Original Value} \times \frac{\text{CPI in the Original Year}}{\text{CPI in the Current Year (2021)}} $
<iframe src="assets/inflation-plot.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
Now that we have our budgets and box office revenues adjusted for inflation, let’s dive in our dataset and see what we can extract from it !
## 1. Is there a relationship between the movies' release country and its ratings?

Example of paragraph here. In this part we wish to understand the links of movies with their produciton countries  with
regard to the box office revenue.
Blablabla

Blablabla
<img src="https%3A%2F%2Fnews.mit.edu%2Fsites%2Fdefault%2Ffiles%2Fstyles%2Fnews_article__image_gallery%2Fpublic%2Fimages%2F201003%2F20100315144150-1_0.jpg%3Fitok%3DxksoTT8q&tbnid=Fb_w8aExDVFILM&vet=12ahUKEwi27rfS-f2CAxXryrsIHWu8Bt4QMygFegQIARB-..i&imgrefurl=https%3A%2F%2Fnews.mit.edu%2F2010%2Fexplained-reg-analysis-0316&docid=tQatzh8dt3mxrM&w=800&h=600&q=regression&ved=2ahUKEwi27rfS-f2CAxXryrsIHWu8Bt4QMygFegQIARB-)" height=600px width=720px class="center"/>

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

...
