---
title: Lyft Data Challenge Report
author:
- Terry Luo
- Nilai Vemula
date: 9-15-2019
---

Due to the fact that the provided data spanned only three months, calculating a driver's lifetime value proved to be a challenge. Some drivers clearly had a lifetime longer than three months for which we do not have data for. If we do not know when a driver stopped driving, it would be very difficult to calculate anything relating to a "lifetime". What we can do, however, is first try to determine which drivers quit within 90 days and which drivers were still active.

We first noted the last timestamp given among all drivers: `2016-06-27 00:50:50 UTC`. [UPDATE TIMESTAMP]Then we found the difference between this last timestamp and the last ride timestamp for each driver - this is essentially the last break that each driver took. If the last break is some duration such as 50 days, it is easy to conclude that the driver quit, as he or she has no recorded rides in the last 50 days of the []. What if the last break was 4 days, or 4 hours?

To answer this question for every driver, we must know more about their driving habits. Do they consistently drive every day or do they tend to take longer breaks? After calculating the difference in time for every ride a driver made, it is clear that there are two types of breaks. Here is a density plot to show the distribution of breaks for a very active driver: 

![img](data:image/png;base64,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)

[this table should have x axis be in days]

This data suggests that drivers often work in "sessions" - periods of hours at a time when they are continuously looking for rides, which makes a lot of sense. The large spike on the left corresponds to all the breaks a driver makes when searching for a new ride, which is usually on the order of minutes. The smaller hills to the right represent the breaks between sessions. If a driver actually quit, then their last break should be a break between sessions. Furthermore, the last break should be significantly greater than the other session breaks a driver has taken. 

After looking at the break distributions for all the drivers, we determined the cutoff between the types of breaks to be 5-6 hours. This is corroborated by Lyft's time limit policy, which prevents drivers from driving for longer than 14 hours at a time without taking an uninterrupted 6 hour break. 

Now that we know exactly which breaks can be considered breaks between sessions, it's easy to determine whether the last break was significant or not. After filtering out all the drivers whose last break was significant, we determined that out of 837 drivers that gave rides, 259 of them quit within the 90 day period.

## Recommend a Driver's Lifetime Value (i.e., the value of a driver to Lyft over the entire projected lifetime of a driver).

- the wording of this question implies that we don't know what the lifetime of a driver is
- we know the exact value a driver brought to lyft if they quit in 3 months
-
- we can split between quitters and non-quitters and find a formula?
- Drivers who we deemed quit within three months made $1807.02 on average.


### What are the main factors that affect a driver's lifetime value?
- ideas
  - total revenue brought in
  - total number of rides

- It's clear that the more rides a driver does, the more total value they bring



- Does Lyft care about a driver's driving pattern?
- If Lyft had total control, how many Lyft drivers would they employ and at what time?
- The surge pricing is a way to combat having more riders than drivers. It filters the riders to those willing to pay the highest price.
-

### What is the average projected lifetime of a driver? That is, once a driver is onboarded, how long do they typically continue driving with Lyft?
- Not able to answer this question in full yet
- What we do know:
  - Out of 837 drivers that we have complete ride data for, 259 (30.9%) quit within 90 days
  - Among the drivers that quit within 3 months, the average career lifetime was 34 days

### Do all drivers act alike? Are there specific segments of drivers that generate more value for Lyft than the average driver?
- What does "more value for Lyft than the average driver mean?"
  - does this mean more total value or value per unit time or ride
- We should analyze this problem disregarding the total amount a driver has made. Instead, we want to take a look at amount made per ride or amount made per time

Statistics for the Average Driver:

- Number of rides: 220
- Mean earnings per ride: $13.64
- Mean ride distance: 4.47 miles
- Mean ride duration: 14.33 minutes
- Mean scaled eagerness: 0.67
- Mean scaled response time: 0.67
- Mean prime time weighted by durations: 16.57

### What actionable recommendations are there for the business?
- take more money from driver's with higher insurance costs


Ymd HMS

- Started off with:
  - 937 drivers
  - 193,502 rides

- Analyzing
  - 837 drivers
  - 184,209 rides
