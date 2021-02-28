# MLB Player Projection Analysis

## Introduction
For this project, I observed statistics from 3173 hitters and 1298 pitchers from 2008-2020. The data was obtained from Baseball-Reference.com. The main goal is to predict player value, encompassed in a statistic called Wins Above Replacement (WAR).

During the analysis, I seeked to answer the following questions:
1. Which types of players tend to last longer in the league?

2. Which factors are most influential in WAR calculation?

3. Which statistics get too little/too much attention among today’s front offices?


## Pre-model EDA
The scale for WAR is approximately as follows for one full season: 2=average, 5=all-star, 8=MVP.

![Screen Shot 2021-02-28 at 6 16 40 AM](https://user-images.githubusercontent.com/29186496/109416543-e20fca00-798c-11eb-99b6-c3011f6bde20.png)


Within the baseball industry, bulkier players, or players with a relatively high BMI, tend to get less valuable contracts when they hit free agency. One goal was to determine if these reservations about how these players would age are valid. So, I observed introductory metrics that provide information regarding the relationships between BMI and longevity (as expressed by the max_age, or the oldest a player was when they were in the league). I did the same for the relationship between BMI and WAR after the age of 30. While there were slightly inverse relationships between BMI and longevity/production after age 30, there was no evidence to suggest that BMI should be a key predictor when trying to determine future value. Therefore, it appears that there should be less emphasis on this in the industry.

## Linear Regression
To get a sense of which variables are most important when calculating WAR, I ran linear regressions for the batter and pitcher dataframes. The results were limited, as after addressing multicollinearity issues and statistically insignificant variables, R-Squared for both datasets were .30. So, 30% of the difference in WAR after age 30 between players could be explained by the retained variables. While the linear regression models had their limitations, they offered some conclusions.

For batters, the ability to make contact proved to be a large indicator of success, as higher strikeout rates lead to lower value. The coefficient for strikeout rate was -17.2, so holding other factors constant, a player who strikes out 10% of the time will provide about 3.4 more WAR than a player who strikes out 30% of the time (17.2*(.3-.1)). The linear regression model also highlighted the limits of stolen bases (SB). CS (caught stealing, or failed stolen base attempts) are nearly four times as costly as stolen bases are valuable. As a result, players need to be successful around 79% of the time on stolen bases for them to be worth attempting, which is very difficult, especially in the latter portion of players' careers.

For pitchers, the strikeout-to-walk ratio proved to be an important predictor of success. For the hitter dataframe, we saw how costly high strikeout rates are for hitters, so it is no surprise to see that strikeouts are valuable for pitchers. In addition, to have a good strikeout-to-walk ratio, low walk-rates are required, so the importance of the ratio indicates that good control pays off.

## Final Model (Random Forest)
In an attempt to improve on the linear regressions, I created a classification model. Players with at least 10 career WAR with at least 25% of their total career WAR coming after the age of 30 were categorized as a 1, while others were categorized as a 0. I decided on those parameters because if a player has fewer than 7.5 career WAR before age 30, then the player is likely not a viable solution to fill a gap on a team. As for the 25% requirement, if a player accumulated a ton of value before age 30, but they could not add much after age 30, then they most likely did not live up to the size of the contract they were offered.

The final models were random forest models, which rely on decision trees. Given the same data, the random forest algorithm will make a decision that optimizes information gain at every step. Trees trained on the same dataset will always come out the same way, so variance is important.

I am most interested in the precision metrics for the random forests. This metric indicates the percentage of players the model recommends to pursue who will prove to be worth their contract and age well in the latter portion of their career. The batter precision was .58, while the pitcher precision ws .86.

## Takeaways
Regarding hitters, it is easier to increase their value through their work at the plate as opposed to in the field. In addition, speed-reliant players have relatively limited longevity in the league, so that reservation within the industry, unlike BMI, seems to hold up.

For pitchers, while ERA does a decent job, simply looking at runs allowed (instead of limiting it to earned runs) does a better job. This is due to the noise of the error statistic, which allows pitchers to get off the hook for runs they allow that get excluded by ERA. 

![Screen Shot 2021-02-28 at 6 18 35 AM](https://user-images.githubusercontent.com/29186496/109416553-f5229a00-798c-11eb-9a26-eee91b0d17bc.png)

Also, control is very valuable for pitchers, as evidenced by the importance of strikeout-to-walk ratios. For both hitters and pitchers, BMI proved to be virtually a non-factor.

## Future Work
In the future, I am interested in incorporating Statcast into the analysis. For pitchers, I would like to know how much higher spin rates on breaking balls help pitchers. Also, it would be good to know if players who rely more on different types of pitches are more valuable and/or durable than others. Pitchers are relatively difficult to project due to their higher injury risk, so durability is a key factor there. For hitters, exit velocity and launch angle on batted balls have recently gained much attention within the industry. I would like to know how much fast exit velocities and optimal launch angles increase player value and likelihood of a hit.
