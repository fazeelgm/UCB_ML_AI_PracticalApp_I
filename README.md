# Will a Customer Accept the Coupon?
<p align='right'>
Practical Application I<br>
UC Berkeley ML/AI Professional Certification coursework<br>
Fazeel Mufti
</p>
  
**Resources**

* ```data/coupons.csv```: Yiou can download this file to play with the data yourself
* ```propmt.ipynb```: More details and supporting code is in this notebook

## Context
This was the first Practical Application Project as part of my UC Berkeley ML/AI Professional Certification coursework. I took this to deep-dive into the ML/AI stack with a grad-level survey course in these domains from March - Oct 2024 as I see this as foundational grounding for any Techie going forward. Onward and upward!

The _**objective** is to see if a person would accept a coupon offered to them on their cell phone as they were driving around town_.  Here's the assignment prompt:

> Imagine driving through town and a coupon is delivered to your cell phone for a restaraunt near where you are driving. Would you accept that coupon and take a short detour to the restaraunt? Would you accept the coupon but use it on a sunbsequent trip? Would you ignore the coupon entirely? What if the coupon was for a bar instead of a restaraunt? What about a coffee house? Would you accept a bar coupon with a minor passenger in the car? What about if it was just you and your partner in the car? Would weather impact the rate of acceptance? What about the time of day?
>
>Obviously, proximity to the business is a factor on whether the coupon is delivered to the driver or not, but what are the factors that determine whether a driver accepts the coupon once it is delivered to them? How would you determine whether a driver is likely to accept a coupon?
> 
>**Overview**
>
>The goal of this project is to use what you know about visualizations and probability distributions to distinguish between customers who accepted a driving coupon versus those that did not.
>
>**Data**
>
>This data comes to us from the UCI Machine Learning repository and was collected via a survey on Amazon Mechanical Turk. The survey describes different driving scenarios including the destination, current time, weather, passenger, etc., and then ask the person whether he will accept the coupon if he is the driver. Answers that the user will drive there ‘right away’ or ‘later before the coupon expires’ are labeled as ‘Y = 1’ and answers ‘no, I do not want the coupon’ are labeled as ‘Y = 0’.  There are five different types of coupons -- less expensive restaurants (under $20), coffee houses, carry out & take away, bar, and more expensive restaurants ($20 - $50).
>
>The attributes of this data set include:
>1. User attributes
>    -  Gender: male, female
>    -  Age: below 21, 21 to 25, 26 to 30, etc.
>    -  Marital Status: single, married partner, unmarried partner, or widowed
>    -  Number of children: 0, 1, or more than 1
>    -  Education: high school, bachelors degree, associates degree, or graduate degree
>    -  Occupation: architecture & engineering, business & financial, etc.
>    -  Annual income: less than $12500, $12500 - $24999, $25000 - $37499, etc.
>    -  Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
>    -  Number of times that he/she buys takeaway food: 0, less than 1, 1 to 3, 4 to 8 or greater
>    than 8
>    -  Number of times that he/she goes to a coffee house: 0, less than 1, 1 to 3, 4 to 8 or
>    greater than 8
>    -  Number of times that he/she eats at a restaurant with average expense less than \\$20 per
>    person: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
>    -  Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
>    
>2. Contextual attributes
>    - Driving destination: home, work, or no urgent destination
>    - Location of user, coupon and destination: we provide a map to show the geographical
>    location of the user, destination, and the venue, and we mark the distance between each
>    two places with time of driving. The user can see whether the venue is in the same
>    direction as the destination.
>    - Weather: sunny, rainy, or snowy
>    - Temperature: 30F, 55F, or 80F
>    - Time: 10AM, 2PM, or 6PM
>    - Passenger: alone, partner, kid(s), or friend(s)
>
>3. Coupon attributes
>    - time before it expires: 2 hours or one day

## Investigation: Overall coupon acceptance rates

Overall redemption rates and acceptance by ```Coupon Type``` were found to be:

<table style="width:100%">
<tr>
<td width="33%">
  <img src="https://github.com/fazeelgm/PracticalApplication-5-1/blob/main/images/pie-acceptance-overall-bar.png" border="10"/>
</td>
<td width="67%">
  <img src="https://github.com/fazeelgm/PracticalApplication-5-1/blob/main/images/bar-coupons.png" border="10"/>
</td>
</tr>
</table>

## Investigation: Acceptance of Bar coupons

We then dove into the single coupon type, Bar, and investigated the redemption behavior along the user, contextual and coupon attributes to build a hypothesis about the users that accepted the these Bar coupons.

<table style="width:100%">
<tr>
<td width="33%">
  <img src="https://github.com/fazeelgm/PracticalApplication-5-1/blob/main/images/pie-acceptance-overall-bar.png" border="10"/>
</td>
<td width="67%">
  <img src="https://github.com/fazeelgm/PracticalApplication-5-1/blob/main/images/bar-acceptance-overall.png" border="10"/>
</td>
</tr>
</table>

We compared the acceptance rates between those who took the coupon offer and rejected them based on the following criteria:

1. Frequency of visits to bars
2. Age of the coupon acceptor
3. Whether there were child passengers in the vehicle
4. Whether their profession required them to get up early in the morning
5. Marital status widowed
6. Frequency of visits to cheap restaurants

Comparing the results, we can draw the following conclusions about those who accepted the bar coupons:

* For _regular_+ bar-goers (visits > 3) they would accept the coupon, but we were likely not changing their behavior
* For _occasional_ bar-goers (visits > 1), they would accept the coupon at higher rates if there was no impediment to their ability to go to the bar (kids, early  wake up, etc.), so we were likely influencing their normal bar-going behavior

_**This shows that bar-goers are a good target audience for our program.**_

## Independent Investigations

We were then asked to use the Bar coupon example to further explore one of the other coupon groups and try to determine the characteristics of passengers who accept the coupons. I used a feature correlation matrix to see if there were any easy "pickings" but nothing obvious jumped out. 

My overall strategy now was to:

* Find a segment where likelihood of coupon acceptance is high
* Then work with Sales, Marketing and Partnership Teams to build revenue-share streams

### Investigating Destination / Direction of Travel

Based on the findings from the last investigation, it seems that the most likelihood of the user accepting the coupon is based on their ability to redeem it immediately. 

**Hypothesis: _Could it be that redemption rate is impacted by the destination and direction of travel for different coupon types?_**

<img src="https://github.com/fazeelgm/PracticalApplication-5-1/blob/main/images/directionality.png" border="10"/>

**Observations when comparing home versus work as a destination**

* Carry out & Take away - redemption very high (62-82%), other coupon types not so much
* Picking up dinner/lunch on way to home? Possibly change user behavior?
* Same for cheaper restaurants? Expensive?

**Observations when comparing the directionality towards or away from the coupon redemption site**

* Coffee House - redemption higher when location is on the way to work (57% vs 33%)
* Carry out & Take away - overall redemption high as noted above but higher when location is on the way home (82% vs 75)
* Restaurant(<20) - redemption is higher when location is on the way home (61% vs 36%)

**Observations when the destination is 'No Urgent Place'**

* Interestingly, 'No Urgent Place' contributes to about half the destinations, so we definitely to look at it
* _Direction of travel is always opposite to the destination for this category, so I'm guessing that directionality data was not captured for this category_
* Overall, the redemption rates across coupon types are higher than what we observe in the Home and Work destinations, with the exception of the Restaurant(<20) and Carry Out categories which are higher. This stands to reason because there is no _urgency_ or time-constraint and allows the user to readily accept them

See the [Learnings & Recommendations](https://github.com/fazeelgm/PracticalApplication-5-1#actionable-learnings--recommendations) section below for conclusions based on these observations.

### Investigating Time of Day

Based on the redemption sensitivity to direction and destination on coupons, I decided to look at the time of day the coupon is accepted. 

**Hypothesis: _Does the Time of Day have an effect on redeeming the coupons?_**

<img src="https://github.com/fazeelgm/PracticalApplication-5-1/blob/main/images/time_of_day.png" border="10"/>

## Actionable Learnings & Recommendations

**Patterns to exploit**
* Partnerships: Carry out and Restaurants<20) had the highest acceptance rates with notable increase when the venue is along the way to the destination. We should look for additonal rev-share partners in these two high converting categories
* Revenue: Coffee House: When driving to work, redemption rates ~35% higher if venue is along the way
* No Urgent Place conversion is generally higher in all categories and we should drill-down on this usage pattern to learn more about coupon placement
* Partnerships / Revenue: Meal-time (breakfast/lunch/dinner) redemption is relatively high, so these types of partnerships should be prioirittized since the customer is already on the road and likely to convert, so we have a stronger negotiating hand in partnership terms

**Missed opportunities**
*  Bar coupons accepted about 50% when heading home and offered in direction of travel, but other rates are lower. This type of coupon shows senstivity to destination and time of day based on the effects of alchohol, and should support more conversion scenarios if these factors are taken into account. We also note the unexpected redemption of Bar coupons (62%) in the 10am time slot could be an indication that we are changing typical usage patterns and there may be other time-sensitive coupons offering scenarios (e.g. shift-end visit to Bars in the morning) that can result in higher acceptance!

**Overal Product Recommendations**
* Coupons along the direction of travel should be surfaced at a higher priority/frequency than those in the opposite direction

Checkout the ```prompt.ipynb notebook``` for all the details!!!

## Resources

* ```data/coupons.csv```: Yiou can download this file to play with the data yourself
* ```propmt.ipynb```: More details and supporting code is in this notebook
