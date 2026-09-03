# Bellabeat Smart Device Usage Analysis

An R-Based Data Analysis and Marketing Insights Report

## Portfolio Project



Keman Xiang

R * RStudio * tidyverse * ggplot2


Based on the Google Data Analytics Professional Certificate Capstone Case Study 2

---

## Table of Contents
Executive Summary	2
1. Ask: Business Task and Research Questions	3
2. Prepare: Data Sources and Scope	3
Data limitations	3
3. Process: Data Preparation and Quality Assessment	4 <br>
3.1 Initial inspection	4<br>
3.2 Cleaning and validation	4<br>
3.3 A critical join correction	4
4. Analyze: Descriptive Analysis	4
4.1 Activity profile	4<br>
4.2 Sleep profile	4
5. Relationships and Visual Analysis	4<br>
5.1 Daily steps and Sedentary time	4<br>
5.2 Very active minutes and Calories	5<br>
5.3 Sleep duration and Time in bed	6<br>
5.4 Sleep duration and Daily steps	7<br>
5.5. Additional Behavioral Pattern: Day of Week	7<br>
5.6. Sleep-duration distribution	8
6. Key Findings	9
7. Business Insights and Marketing Implications	10<br>
7.1 Product positioning: from tracking to personalized wellness guidance	10<br>
7.2 Behavioral segmentation opportunity	10<br>
7.3 Content strategy	10<br>
7.4 Timing of engagement	10
8. Limitations	10
9. Conclusion	11<br>
Appendix: Analytical Audit	11

## Executive Summary
This project analyzes public Fitbit fitness-tracker data to identify patterns in activity and sleep behavior and translate those patterns into high-level marketing implications for Bellabeat. The analysis uses two daily-level datasets: <b>dailyActivity_merged.csv</b> and <b>sleepDay_merged.csv</b>.

The portfolio version of the analysis follows the Google Data Analytics case-study framework of <b>Ask, Prepare, Process, Analyze, Share, and Act</b>, while expanding the original course notebook with explicit data-quality checks, reproducible transformations, relationship analysis, and interpretation.

<b>Key conclusion</b>. The data indicate that sedentary behavior, activity intensity, sleep duration, and weekly activity patterns are useful complementary signals of wellness behavior. The strongest business opportunity is therefore not to optimize one metric in isolation, but to combine behavioral signals into personalized and context-aware guidance.

The evidence is hypothesis-generating rather than causal or population-representative because the sample is small, the observation period is short, the data are from 2016, and demographic information is unavailable.

(For related .rmd file and plots, please refer to my [GitHub directory](https://github.com/KemanXiang/R_Projects/tree/7e5bb6f636d20e2dbbf80acc572ac98b63ec4abb/Portfolio_Project/01Bellabeat_Smart_Device_Usage_Analysis)

You can also view my R outputs on [RPub](http://rpubs.com/KemanXiang/1456053).)

---

## 1. Ask: Business Task and Research Questions

Bellabeat is a wellness technology company whose products connect users with information about activity, sleep, stress, hydration, and other aspects of daily wellness. The business task is to use non-Bellabeat smart-device data to understand user behavior and identify insights that can inform Bellabeat’s marketing strategy.

The analysis addresses the original case-study questions:
1.What are some trends in smart-device usage?
2.How could these trends apply to Bellabeat customers?
3.How could these trends help influence Bellabeat marketing strategy?

The analysis focuses on the <b>Bellabeat app</b> as the product context because the app can connect multiple wellness behaviors and translate tracker data into personalized guidance.

---

## 2. Prepare: Data Sources and Scope
The source case study identifies the FitBit Fitness Tracker Data, made available through Mobius, as the principal public dataset. The full source package contains multiple files at daily, hourly, and minute-level resolutions.

For this project, two files were selected:
- dailyActivity_merged.csv: daily activity measures including steps, distance, activity intensity, sedentary minutes, and calories.
- sleepDay_merged.csv: daily sleep measures including sleep records, minutes asleep, and time in bed.

|Dataset|	Rows|	Users|	Primary Information|
|-------|-----|------|---------------------|
|dailyActivity_merged.csv|	940|	33|	steps, distance, activity intensity, sedentary minutes, calories|
|sleepDay_merged.csv|	413|	24|	sleep records, minutes asleep, time in bed|

The two files are complementary rather than redundant: the activity table provides a broad daily activity profile, while the sleep table adds a second dimension of wellness behavior.

### Data limitations
The dataset contains observations from a small group of Fitbit users over approximately one month. The activity file contains 33 distinct users, while the sleep file contains 24 distinct users. The dataset therefore does not represent a broad population sample, and the absence of demographic variables means that findings cannot be assumed to describe Bellabeat’s female customer base specifically.

The data are also from 2016 and therefore should be interpreted as behavioral evidence from an older smart-device ecosystem rather than as a current estimate of consumer behavior.

---

## 3. Process: Data Preparation and Quality Assessment

### 3.1 Initial inspection
The R workflow begins by importing both CSV files and inspecting their dimensions, column names, data types, distinct user counts, and observation counts. This establishes the analytical grain before any transformation.

## 3.2 Cleaning and validation
Column names are standardized with <b>janitor::clean_names()</b>, and date fields are explicitly parsed before analysis. The supplied files contain no missing values. The activity table contains no duplicate rows. The sleep table contains three exact duplicate records, which are removed before the combined analysis.

The activity data also contain zero-step days and days with 1,440 sedentary minutes. These observations are retained because they may reflect genuine non-wear or incomplete logging rather than demonstrable data errors. They are treated as a limitation during interpretation.

## 3.3 A critical join correction

The first version of my notebook merged the tables using Id only. That operation is technically problematic for daily observations: every sleep record for a user can be paired with every activity day for that same user. The resulting table has 12,441 rows and would artificially duplicate daily observations.

For the portfolio version, the tables are instead joined on <b>user ID + date</b>. After duplicate removal and date alignment, the matched dataset contains <b>410 daily user-date observations from 24 users</b>. This produces a valid one-day-to-one-day comparison for sleep and activity.

This correction is important because it improves reproducibility and prevents an inflated sample from driving the sleep-versus-steps relationship.

---

## 4. Analyze: Descriptive Analysis

### 4.1 Activity profile
Across 940 activity observations, mean daily steps are <b>7,638</b>, mean total distance is <b>5.49</b>, and mean calories burned are <b>2,304</b>. Mean sedentary time is <b>991 minutes</b> per day.

The mean activity profile contains approximately <b>21 very active minutes, 14 fairly active minutes</b>, and <b>193 lightly active minutes</b> per day. This highlights the importance of sedentary behavior as a substantial component of the observed day.

### 4.2 Sleep profile
Mean recorded sleep duration is <b>419.2 minutes (6.99 hours)</b>, while mean time in bed is <b>458.5 minutes (7.64 hours)</b>.

Only <b>28.3%</b> of sleep observations reach eight hours, while <b>24.4%</b> are below six hours. The mean difference between time in bed and recorded sleep is approximately <b>39 minutes</b>.

---

## 5. Relationships and Visual Analysis

### 5.1 Daily steps and Sedentary time

Figure: 5.1 Steps and sedentary time
<img width="1538" height="962" alt="Figure_1_Daily_Steps_and_Sedentary_Time" src="https://github.com/user-attachments/assets/8397d91f-ccb9-4787-8e5a-4029b5d4fce2" />

The fitted line summarizes the overall direction of association; it does not imply causation.

The relationship is negative: Pearson correlation is <b>r = -0.33</b> (p < 0.001). The association is modest, with <b>R2 ≈ 0.11</b>. Days with more steps tend to have fewer sedentary minutes, but the relationship is far from deterministic.

For Bellabeat, this suggests a behavioral-segmentation opportunity: some users may benefit less from additional exercise targets and more from interventions that encourage movement throughout otherwise sedentary days.

### 5.2 Very active minutes and Calories

Figure: 5.2 Very active minutes and calories
<img width="1538" height="962" alt="Figure_2_Very_Active_Time_and_Calories_Burned" src="https://github.com/user-attachments/assets/095c7f5b-a8f6-42b0-ad9d-fba0ce096c5b" />

Very active minutes show a moderate positive association with calories burned (r = 0.62, p < 0.001; R2 ≈ 0.38). Days with more vigorous activity tend to coincide with higher recorded energy expenditure. Activity intensity therefore provides a useful behavioral signal beyond simple step counts.

### 5.3 Sleep duration and Time in bed

Figure: 5.3 Sleep duration and time in bed
<img width="1538" height="962" alt="Figure_3_Sleep_Duration_and_Time_in_Bed" src="https://github.com/user-attachments/assets/9fa18fc8-733b-4a88-9c70-5e026cb0568b" />

Sleep duration and time in bed are very strongly associated (r = 0.93, p < 0.001). This is expected because sleep duration is a major component of time in bed. The remaining gap—about 39 minutes on average—can nevertheless provide a useful behavioral indicator of time spent in bed without recorded sleep.

### 5.4 Sleep duration and Daily steps

Figure: 5.4 Sleep duration and daily steps
<img width="1538" height="962" alt="Figure_4_Sleep_Duration_and_Daily_Steps" src="https://github.com/user-attachments/assets/964290de-96f8-4d15-ac33-ac801ec0de78" />

Before combining the datasets, dates are standardized and the tables are joined by <b>user ID and date</b>. This is important because joining only on Id would match each user’s sleep observations to every activity day for that user and would artificially duplicate observations.

The corrected matched dataset shows a weak negative association between sleep duration and steps (r = -0.19, p < 0.001; R2 ≈ 0.04). The effect size is small, so the evidence does not support a strong claim that longer sleep causes lower activity—or vice versa.

The more defensible interpretation is that sleep and activity should be monitored together as complementary wellness dimensions. The relationship is useful for personalization, but not strong enough to justify a simple universal rule.

### 5.5. Additional Behavioral Pattern: Day of Week

Figure:5.5. Additional Behavioral Pattern: Day of Week
<img width="1538" height="962" alt="Figure_5_Average_Daily_Steps_by_Day_of_Week" src="https://github.com/user-attachments/assets/975ef55c-99cc-46b0-a53c-ece72a5b07c1" />

Average recorded steps are highest on Saturday (8,153) and Tuesday in this sample and lowest on Sunday6,933) . The weekday pattern suggests that activity is not uniform across the week.

Because the differences are observational and the sample is small, these results should be used as a hypothesis for marketing experimentation rather than as a universal behavioral rule.

### 5.6. Sleep-duration distribution

Figure: 5.6.Distribution of recorded daily sleep duration. 
<img width="1538" height="962" alt="Figure_6_Sleep_Duration_Distribution" src="https://github.com/user-attachments/assets/93c96c9e-f26f-456c-8ccb-519084d33297" />

The dashed reference line marks eight hours.

The distribution shows substantial variation in sleep duration rather than a single uniform user pattern. This supports using sleep behavior as a segmentation or personalization signal instead of assuming that all users have the same wellness needs.

---

## 6. Key Findings

### Finding 1 — Sedentary behavior is a meaningful component of daily activity.
The average day contains approximately 991 sedentary minutes, and higher sedentary time is associated with fewer steps. This suggests an opportunity for interventions focused not only on exercise but also on breaking up sedentary periods.

### Finding 2 — Vigorous activity is strongly connected with recorded calorie expenditure.
Very active minutes show a moderate positive association with calories burned(r = 0.62). Activity intensity can therefore provide a useful behavioral signal for personalized feedback.

### Finding 3 — Sleep duration is generally below eight hours in the observed records.
The mean sleep duration is approximately 6.99 hours. Only about 28.3% of recorded sleep observations reach eight hours or more, while about 24.4% are below six hours. These descriptive results indicate substantial variation in sleep behavior.

### Finding 4 — Sleep and activity should be considered together.
The matched data show only a weak negative relationship between sleep duration and daily steps (r = -0.19). Rather than supporting a simple “more sleep means fewer steps” conclusion, the data suggest that sleep and physical activity should be treated as complementary dimensions of wellness.

### Finding 5 — Weekly behavior varies.
Average steps differ by day of week, with Saturday showing the highest average in this sample and Sunday the lowest. This creates a potential basis for time-sensitive engagement strategies.

---

## 7. Business Insights and Marketing Implications

### 7.1 Product positioning: from tracking to personalized wellness guidance
The data suggest that users generate multiple behavioral signals—activity, sedentary time, and sleep—that can be interpreted together. For Bellabeat, this supports positioning the app not merely as a tracker, but as a tool that helps users understand relationships among everyday wellness behaviors.

### 7.2 Behavioral segmentation opportunity
A practical segmentation framework could distinguish:
- <b>Active users</b>: higher activity and lower sedentary time.
- <b>Sedentary users</b>: relatively low steps and high sedentary time.
- <b>Sleep-focused users</b>: users with limited sleep duration or large gaps between time in bed and time asleep.
- <b>Balanced users</b>: relatively consistent activity and sleep patterns.

The available data do not contain demographic information, so these should be treated as behavioral segments, not demographic segments.

### 7.3 Content strategy
Marketing content could emphasize small, measurable behavior changes:
- movement reminders for sedentary users;
- activity goals for users seeking greater daily movement;
- sleep-awareness content for users with short sleep duration;
- weekly summaries that connect activity and sleep rather than reporting isolated metrics.

### 7.4 Timing of engagement
The observed day-of-week differences suggest an opportunity to test whether engagement messages perform differently across the week. For example, Bellabeat could experiment with weekend movement prompts or end-of-week wellness summaries.

These recommendations are hypotheses derived from the observed sample and should be validated through current Bellabeat product data and controlled marketing experiments.

---

## 8. Limitations

Several limitations constrain the conclusions:

1. <b>Small and non-representative sample</b>. The activity data contain 33 users and the sleep data contain 24 users.
2. <b>Limited observation period</b>. The records cover approximately one month in 2016.
3. <b>No demographic variables</b>. The dataset does not establish that participants represent Bellabeat’s female target market.
4. <b>Third-party device data</b>. The data describe Fitbit users rather than Bellabeat users.
5. <b>Missing sleep coverage</b>. Not every activity record has a corresponding sleep record.
6. <b>Observational relationships</b>. Correlations describe associations and do not establish causation.
7. <b>Potential tracker non-wea</b>r. Zero-step or extremely sedentary days may reflect non-wear or incomplete logging rather than true behavior.

For future analysis, current Bellabeat data, demographic information, longer observation windows, and more complete sleep/activity coverage would materially strengthen the evidence.

## 9. Conclusion

This analysis demonstrates an end-to-end R workflow for transforming public smart-device data into business insights. The evidence indicates that sedentary behavior, activity intensity, sleep duration, and weekly activity patterns are useful dimensions for understanding wellness behavior.

The strongest practical opportunity is not to optimize one metric in isolation, but to use multiple behavioral signals to deliver personalized and context-aware guidance. For Bellabeat, this supports a marketing strategy centered on <b>personalized wellness coaching, behavioral segmentation, and timely engagement</b>.

The findings should be regarded as hypothesis-generating rather than causal or population-representative. Their primary value is demonstrating how a reproducible analytical workflow can move from raw device data to interpretable business questions, evidence-based insights, and testable marketing recommendations.

## 10. Methodology and Reproducibility
The analysis was implemented in R/RStudio using tidyverse, janitor, lubridate, skimr, here, and readr. The portfolio version explicitly records data-cleaning decisions, validates missing and duplicate records, standardizes dates before joining, and uses a user-date key for the combined sleep/activity analysis.

The original course workflow is preserved as the conceptual backbone: <b>Ask → Prepare → Process → Analyze → Share → Act</b>. The accompanying R Markdown file provides the reproducible analytical workflow.

## References and Source Materials
Google Data Analytics Professional Certificate. Case Study 2: How Can a Wellness Technology Company Play It Smart? The original instructions define the business scenario, guiding questions, data source, analytical process, visualization requirements, and recommendation deliverables.

FitBit Fitness Tracker Data. Public-domain fitness-tracker dataset made available through Mobius and distributed through Kaggle. The dataset contains daily activity, sleep, and other device measurements collected from Fitbit users.

Portfolio structure was informed by publicly available examples of Bellabeat analyses on Kaggle and portfolio sites. These examples were used only to benchmark presentation structure; all numerical findings in this report are calculated from the two supplied CSV files.


## Appendix: Analytical Audit

|Item|Result|
|----|------|
|Activity observations|	940|
|Activity users|	33|
|Sleep raw records|	413|
|Sleep users|	24|
|Exact duplicate sleep records|	3|
|Missing values in supplied files|	0|
|Matched user-date observations after cleaning|	410|
|Matched users|	24|
|Mean steps/day|	7,638|
|Mean sedentary minutes/day|	991|
|Mean sleep duration|	6.99 hours|
|Sleep observations ≥ 8 hours|	28.3%|
|Sleep observations < 6 hours|	24.4%|
|Steps vs sedentary Pearson r|	-0.33|
|Very active minutes vs calories Pearson r|	0.62|
|Sleep vs time in bed Pearson r|	0.93|
|Matched sleep vs steps Pearson r|	-0.19|





