---
title: Lyft Data Challenge Report
author:
- Terry Luo
- Nilai Vemula
date: 9-15-2019
fignos-cleveref: True
fignos-plus-name: Fig.
header-includes:
  - \numberwithin{figure}{section}
  - \numberwithin{equation}{section}
  - \numberwithin{table}{section}

---

Due to the fact that the provided data spanned only three months, calculating a driver's lifetime value proved to be a challenge. Some drivers clearly worked longer than three months for which we do not have data for, making it be very difficult to calculate anything relating to a "lifetime". What we can do, however, is first try to determine which drivers quit within 90 days and which drivers were still active.

We first noted the last timestamp given among all drivers: `2016-06-27 00:50:50 UTC`. [UPDATE TIMESTAMP]Then we found the difference between this last timestamp and the last ride timestamp for each driver - this is essentially the last break that each driver took. If the last break is some duration such as 50 days, it is easy to conclude that the driver quit, as he or she has no recorded rides in the last 50 days of the []. What if the last break was 4 days, or 4 hours?

To answer this question for every driver, we must know more about their driving habits. Do they consistently drive every day or do they tend to take longer breaks? After calculating the difference in time for every ride a driver made, it is clear that there are two types of breaks. +@fig:1 is a density plot displaying the distribution of breaks for a very active driver: 

![Driver Density Plot](C:/Users/luote/Documents/Projects/Lyft_Data_Challenge/plots/first_driver_density.png){#fig:1 width=7in}

[figure legend - driver id = "002be0ffdc997bd5c50703158b7c2491"]

This data suggests that drivers often work in "sessions" - periods of hours at a time when they are continuously looking for rides, which makes a lot of sense. The large spike on the left corresponds to all the breaks a driver makes when searching for a new ride, which is usually on the order of minutes. The smaller hills to the right represent the breaks between sessions. If a driver actually quit, then their last break should be a break between sessions. Furthermore, the last break should be significantly greater than the other session breaks a driver has taken. 

After looking at the break distributions for all the drivers, we determined the cutoff between the types of breaks to be 5-6 hours. This is corroborated by Lyft's time limit policy, which prevents drivers from driving for longer than 14 hours at a time without taking an uninterrupted 6 hour break. 

We can now categorize each break longer than 6 hours as a "break between session." We considered the last break a driver took to be significant if it was 

1. greater than two standard deviations above the mean length of session breaks, and
2. at least 98% of the previous longest break between sessions a driver took

The reason for #2 is that if a driver has previous history of taking an x day break that is considered significant, we can assume that they may take a break of similar length again. After filtering out all the drivers whose last break was significant, we determined that out of 837 drivers that gave rides, 259 of them quit within the 90 day period.