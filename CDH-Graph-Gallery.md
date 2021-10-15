Welcome to the CDH graph gallery, a collection of charts made from Pega data to give you insights into CDH.

Detailed steps and code examples can be found by clicking on the images.

## Offer Analysis

How successful are the offers? How does that vary across channels, and how well can we find the preferences of individual customers?

[R](https://pegasystems.github.io/cdh-datascientist-tools/articles/adm-datamart.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/python/Example_ADM_Analysis.ipynb)

| Performance vs Success Rate by Channel and Issue | Average Model performance by Channel and Group |
| :---: | :---: |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/bubblechart_on_channel_issue.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/average_model_performance_by_channel_group.png" width="100%"> |
| **Proposition Base Success Rates** | **Animation of Model Evolution** |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/overall_proposition_success_rates.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/adm_animation.gif" width="100%"> |

## Response Analysis

How do the accept rates look like in different channels? And how do they change over time? What are the effects in accept rates after a change to the application has been made? How does the propensity distribution look like?

[R](https://pegasystems.github.io/cdh-datascientist-tools/articles/historical-dataset.html) [Python](https://github.com/pegasystems/cdh-datascientist-tools/blob/master/python/Example_Historical_Dataset_Analysis.ipynb)

| Hierarchical breakdown of response counts | Weekly accept rates with outliers | Delta in accept rates |
| :---: | :---: | :---: |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_responses_hierarchical_breakdown.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_weekly_accept_rate_with_outliers.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/IH_share_delta.png" width="100%"> |
| **Test-Control analysis** | **Z Scores of test vs control** | |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/test-control-analysis.png" width="100%"> | <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/test-control-zscores.png" width="100%"> | |

## Model Analysis

At a very high level, you can look at the overall performance, across all models, all issues and all channels. This very simple graph may already tell you if everything is working as expected, and if new channels or issues are added, see those effects in this timeline.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/overall_model_performance.png" width="50%">

At a more detailed level, you can zoom in into individual models and ask questions like: what is the expected uptake when I target the top decile of my customers for a certain offer? At which propensity threshold do I get 300% lift? Plots like the ones below (which are also part of the off-line model reports) can help answer those questions:

| Lift | Cumulative Gains |
| :---: | :---: | 
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/model_lift.png" width="100%"> |  <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/model_cumgains.png" width="100%"> |


## Predictor Analysis

Which predictors to well overall? Are there channels in which the external scores feeding into ADM models do not work well?

| Predictor performance by Type |
| :---: |
| <img src="/pegasystems/cdh-datascientist-tools/blob/master/images/aggregate_predictor_performance.png" width="100%"> |

