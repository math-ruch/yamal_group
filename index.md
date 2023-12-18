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
<iframe src="assets/4-oscar-categories.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
But we are not dumb because we took the ADA class ! Our analysis so far has been naïve, not considering that many confounders may affect the box office revenue. To try to isolate the Oscar actor effect on revenue, we have performed a paired matching. By completing the paired matching, we try to compare movies with similar probability of receiving the treatment, which in our case is having at least one Oscar nominated actor. We used logistic regression to compute the propensity score based on the budget of the movie and the number of actors of the movie. We then perform the T-test for the mean box office revenues of two independent groups. This is a test for the null hypothesis that 2 independent samples have identical average (expected) values. Unfortunately we obtain a p-value of 0.42, which is greater than the rejection threshold of 0.05, which basically means that we cannot reject the null hypothesis that the two groups come from different distributions. Also, the confidence intervals between the two groups are clearly overlapping. We have to dig more deeply into our data !
<iframe src="assets/box_office_revenue_comparison_after_first_matching.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>
Maybe let us modify a little bit the treatment assignment definition and re define the treatment by “having two or more Oscar nominated actors in your casting” and let us redo the exact same study to see if it changes anything. When re-doing the study, we are again separating the movies between a treatment and a control group where the treatment group consists of movies who have more than a single Oscar nominated actor in their cast. And guess what ? this time we got a 0 p-value and non-overlapping confidence intervals suggesting that having two or more Oscar nominated actors in your cast will really increase your chances of performing well in the box office !
<iframe src="assets/box_office_revenue_comparison_after_sec_matching.html" width="750px" height="530px" frameborder="0" position="relative">Genre plot</iframe>

## 5. Are longer movies more likely to be more successful?

...
-----------------

## 6. Are movies with higher budgets more successful?

...
-----------------

## 7. what is the relationship between a movies reviews from critics and it’s success?

...
-----------------

## Wrap Up

...
