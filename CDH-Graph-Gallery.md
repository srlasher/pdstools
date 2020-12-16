Welcome to the CDH graph gallery, a collection of charts made from Pega data to give you insights into CDH.

The charts are made using just a few lines of R or Python and use data from the ADM Datamart, from IH and Decision Results. A few also use customer data exports. The gallery intends to show how easy it is to get additional and deeper insights into the what and how of CDH and the AI models mostly using stock data exports from the platform.

It is by no means exhaustive but mainly serves to give inspiration and some guidance how to use the open source GitHub repository with CDH tools.

All of the visualisation examples in this article can be forked from this Github repo. Detailed steps and code examples can be found by clicking on the images.

## Offer Analysis

How successful are the offers? How does that vary across channels, and how well can we find the preferences of individual customers?

| Performance vs Success Rate by Channel and Issue | Average Model performance by Channel and Group |
| :---: | :---: |
| [<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/bubblechart_on_channel_issue.png" width="100%">](CDH-Model-analysis-from-ADM-Datamart) | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/average_model_performance_by_channel_group.png" width="100%"> |
| **Proposition Base Success Rates** | **Animation of Model Evolution** |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/overall_proposition_success_rates.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/adm_animation.gif" width="100%"> |

## Response Analysis

How do the accept rates look like in different channels? And how do they change over time? What are the effects in accept rates after a change to the application has been made? How does the propensity distribution look like?

| Hierarchical breakdown of response counts | Weekly accept rates with outliers | Delta in accept rates |
| :---: | :---: | :---: |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_responses_hierarchical_breakdown.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_weekly_accept_rate_with_outliers.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_share_delta.png" width="100%"> |

## Model Analysis

What is the expected uptake when I target the top decile of my customers for a certain offer? At which propensity threshold do I get 300% lift?

| Lift | Cumulative Gains | TODO |
| :---: | :---: | :---: |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/lift_offline_modelreport.png" width="100%"> |  <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/cum_gains_offline_modelreport.png" width="100%"> |   |


## Predictor Analysis

Which predictors to well overall? Are there channels in which the external scores feeding into ADM models do not work well?

| Predictor performance by Type | TODO | TODO |
| :---: | :---: | :---: |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/aggregate_predictor_performance.png" width="100%"> |   |   |


## Off-line model reports
***

(pics)


