# Coupon Analysis

This repo contains the notebook that analyzes the [coupon data](./data/coupons.csv) and attempts to determine the factors that drive a user to accept or deny the coupon. 
- the analysis is manual and not exhaustive given the number of columns
- applying ML techniques would possibly enhance the analysis, but at the moment that is beyond the scope of this assignment

The notebook is present in this [github repo](./prompt.ipynb) as well as on [google drive](https://colab.research.google.com/drive/1Fm75hjiLj5AfgSSbWp8s4Fu6YLSjDC04?usp=sharing) to be referenced using Google Colab

## Problem Statement (as provided in the notebook prompt)
**Context**

Imagine driving through town and a coupon is delivered to your cell phone for a restaurant near where you are driving. Would you accept that coupon and take a short detour to the restaurant? Would you accept the coupon but use it on a subsequent trip? Would you ignore the coupon entirely? What if the coupon was for a bar instead of a restaurant? What about a coffee house? Would you accept a bar coupon with a minor passenger in the car? What about if it was just you and your partner in the car? Would weather impact the rate of acceptance? What about the time of day?

Obviously, proximity to the business is a factor on whether the coupon is delivered to the driver or not, but what are the factors that determine whether a driver accepts the coupon once it is delivered to them? How would you determine whether a driver is likely to accept a coupon?

**Overview**

The goal of this project is to use what you know about visualizations and probability distributions to distinguish between customers who accepted a driving coupon versus those that did not.

**Data**

This data comes to us from the UCI Machine Learning repository and was collected via a survey on Amazon Mechanical Turk. The survey describes different driving scenarios including the destination, current time, weather, passenger, etc., and then ask the person whether he will accept the coupon if he is the driver. Answers that the user will drive there ‘right away’ or ‘later before the coupon expires’ are labeled as ‘Y = 1’ and answers ‘no, I do not want the coupon’ are labeled as ‘Y = 0’.  There are five different types of coupons -- less expensive restaurants (under \$20), coffee houses, carry out & take away, bar, and more expensive restaurants (\$20 - $50).

**Deliverables**

Your final product should be a brief report that highlights the differences between customers who did and did not accept the coupons.  To explore the data you will utilize your knowledge of plotting, statistical summaries, and visualization using Python. You will publish your findings in a public facing github repository as your first portfolio piece.

## Observations

### Overall
The overall distribution **per coupon type** and **visit frequency** shows:

<img src="./images/coupon_acceptance_overall.png" alt="overall acceptance" width="400" height="300">
<img src="./images/visit_freq.png" alt="visit freq" width="400" height="300">


- we have maximum data points for *coffee house* and *cheaper restaurants*
- *carryout* and *cheaper restaurants* have a much higher chance of acceptance
- *bars* have a higher chance of rejection
- *coffee houses* and *costly restaurants* have an equal chance
- people visiting *1-8* times have much higher chance of accepting a coupon for a given type of place

### Temperature
Lets break up the acceptance distribution by temperature:

<img src="./images/coupon_acceptance_temperature.png" alt="temperature based acceptance" width="900" height="300">

- in general we have more data points for warmer weather
- in cold weather coupon denial is much higer (except for carryout)
- in warmer weather *cheap restaurants*, and *carrout & take away* acceptance spike up

### Frequency of Visit
Exploring the acceptance rate for Bar coupons based on user's frequency of visit:

| Bar visit | Acceptance Rate |
|:----------|:----------------|
| > 3 | 0.768844 |
| < 3 | 0.370618 |

- people who visit a bar 3 times or more have a much higher chance of accepting a coupon (70%) compared to those who visit less than 3 times

- multi variate 

| Criteria | Rate |
|:---------|:-----|
| bar > once/month & age > 25 | 0.6952 |
| bar > once/month & age < 30 | 0.7217 |
| bar > once/month & passanger != Kids && occupation != Farming/Fishing/Forestry | 0.7132 |
| bar > once/month & passanger != Kids && maritalStatus != Widowed | 0.7132 |

- observations:
	- for the bar scenario, we see people who go to bar > once/month always have ~ 70% probability of accepting the coupon
	- irrespective of whether they are > 25 years, or have non kid as a passenger or if they are < 30 years
	- just going to the bar > once/month increases the odds of accepting the coupon

### Exploring all coupon types for age ranges and visit frequency
Plotting the coupon types seperately for different age groups and visit frequency:

<img src="./images/coupon_acceptance_Bar.png" alt="Bar" width="1500" height="300">
<img src="./images/coupon_acceptance_Coffee%20House.png" alt="Coffee House" width="1500" height="300">
<img src="./images/coupon_acceptance_Carry%20out%20%26%20Take%20away.png" alt="Carry out & Take away" width="1500" height="300">
<img src="./images/coupon_acceptance_Restaurant(_20).png" alt="RestaurantLessThan20" width="1500" height="300">
<img src="./images/coupon_acceptance_Restaurant(20-50).png" alt="Restaurant20To50" width="1500" height="300">

- *21-30* has the most data points, followed by *31-40*
- *<21* are probably non-adults (non-drivers) and their representation is almost non-existent
- these two groups also have the highest acceptance-to-denial ratio meaning the odds of acceptance are much higher (particulalry in 21-40)
- *CoffeeHouse, CarryOut and Cheap restaurants* have the highest acceptance rate for folks who visit 1-3 and 4-8 times
- People who have never been to a *Bar or Coffee House* have the highest denial ratio for *21-30 and 31-40* age groups

### Exploring all coupon types for destinations and visit frequency
Plotting the coupon types seperately for different destinations and visit frequency:

<img src="./images/coupon_acceptance_Bar_destination.png" alt="Bar" width="900" height="300">
<img src="./images/coupon_acceptance_Coffee%20House_destination.png" alt="Coffee House" width="900" height="300">
<img src="./images/coupon_acceptance_Carry%20out%20%26%20Take%20away_destination.png" alt="Carry out & Take away" width="900" height="300">
<img src="./images/coupon_acceptance_Restaurant(_20)_destination.png" alt="RestaurantLessThan20" width="900" height="300">
<img src="./images/coupon_acceptance_Restaurant(20-50)_destination.png" alt="Restaurant20To50" width="900" height="300">

- people who are traveling to a *non-urgent destination* have higher odds of accepting a coupon
- the probability is highest for *Coffee House*, *Carry Out* and *Cheap Restaurants*

## Exploring all coupon types for income and visit frequency
Not adding the similar plots since they look repetitive. However the analysis states:
- income groups *12500-37499* and *37500-74999* have much higher probability of rejecting a coupon for a Bar
- the same income groups have a higher chance of accepting a *coffee house* coupon if they visit more frequently in general (1-3, 4-8, >8)
 - carry out and cheap restaurants have a higher probabiliy of acceptance irrespective of income group. However the above income groups have the highest odds.
- costly restaurants have a higher rate of rejection unless the income is more than 100K and visiting tendency is more frequent


> We start to realize that with so many parameters its hard to manually determine which columns have a stronger correlation with the probability of coupon acceptance. Atleast to touch upon the other columns we do a plot of each remaining feature with the acceptance vs rejection count

## Plotting some more features to find any patterns

|<img src="./images/coupon_acceptance_expiration_count.png" alt="Expiration" width="300" height="300">|<img src="./images/coupon_acceptance_gender_count.png" alt="Gender" width="300" height="300">|<img src="./images/coupon_acceptance_maritalStatus_count.png" alt="Marital" width="300" height="300">|<img src="./images/coupon_acceptance_has_children_count.png" alt="Children" width="300" height="300">|
|-|-|-|-|
<img src="./images/coupon_acceptance_passenger_count.png" alt="Passenger" width="300" height="300">|<img src="./images/coupon_acceptance_temperature_count.png" alt="Temp" width="300" height="300">|<img src="./images/coupon_acceptance_weather_count.png" alt="Weather" width="300" height="300">|<img src="./images/coupon_acceptance_time_count.png" alt="Time" width="300" height="300">|

- more data points (and probability of acceptance) for people traveling alone and with friends
- sunny weather and also 80 degrees (both are similar) leads to more acceptance
- 2pm and 6pm gives more odds of acceptance
- coupons with 1d validity has mmore odds
- gender doesnt necessarily play an important role
- people who are single, or married (and also have children) have a higher chance of acceptance

## Summary & Conclusion
- There are multiple factors that help determine the odds of accepting a coupon
- Its hard to determine which factor has a stringer influence over others
- Weather, time of the day, income, frequency of visit - almost everyone of these have a correlation with the acceptance rate
- Every type of coupon is unique:
  	- Cheap restaurants and coffee houses are more acceptable to younger and low-mid income people
	- Bars have a strong relationship with the frequency of visit
	- Costly restaurants are more acceptable among higher income groups
	- Warmer weather has a strong relationship with almost all coupon types
	- No urgent destination also increases the odds of acceptance

While we are able to determine many relationships, its hard to determine which ones exert more influence. At the same time, since there are so many parameters, its next to impossible to try out all possible combinations and come to a conclusion manually. The ability to use a ML model to analyze all the parameters would probably yield more useful results without extensive manual effort and in a shorter time span.


