Interaction History stores both the predicted propensities as well as the actual outcomes. Using the `cdhtools` it just takes a few lines to calculate model performance from IH. This could be a useful additional analysis especially when looking across different models.

We start by loading the libraries

```r
library(cdhtools)
library(data.table)
library(lubridate)
library(ggplot2)
```

Then we read an export of IH data. Here we assume the data has been downloaded from Pega using an export from the `pxInteractionHistory` dataset. In practice, IH is usually very large and you would need to apply some filtering and sampling in order to get to a manageable sized set of data.

After the ingestion of the data, we apply some name standardisation and turn the outcome time into a proper time format.

```r
# ih <- readDSExport("Data-pxStrategyResult_pxInteractionHistory", "~/Downloads")
ih <- readDSExport("Data-pxStrategyResult_pxInteractionHistory.zip", "../data")

applyUniformPegaFieldCasing(ih)
ih[, OutcomeTime := fromPRPCDateTime(OutcomeTime)
```

Now that we have the data in the right format, we choose an aggregation period. Here, we settled on "days". Then we apply standard `data.table` summarisation to calculate the AUC per day and per Issue/Group/Name combination. The work-horse function here is `auc_from_probs` from `cdhtools` that calculates AUC given a series of outcomes and propensities. Of course, other libraries (eg `pROC`) could be used to accomplish the same.

```r
ih[, period := day(OutcomeTime)]
performance <- ih[Issue=="Sales", 
                  .(auc = auc_from_probs(Outcome=="Accept"|Outcome=="Accepted", Propensity),
                    n = .N,
                    date = as.Date(first(OutcomeTime))), 
                  by=c("period", "Name", "Group", "Issue")]

```

Which gives a `data.table` with the performance of the Sales actions. In this example data we only had two days.

```
   period             Name       Group Issue       auc      n       date
1:     22    UPlusPersonal CreditCards Sales 0.5298628  62436 2021-01-22
2:     22        UPlusGold CreditCards Sales 0.5806756  53171 2021-01-22
3:     22 UPlusFinPersonal CreditCards Sales 0.5876831  58409 2021-01-22
4:     22     UPlusFinGold CreditCards Sales 0.5741162  58741 2021-01-22
5:     27    UPlusPersonal CreditCards Sales 0.5379591 125091 2021-01-27
6:     27        UPlusGold CreditCards Sales 0.5767399 106987 2021-01-27
7:     27 UPlusFinPersonal CreditCards Sales 0.6011121 117023 2021-01-27
8:     27     UPlusFinGold CreditCards Sales 0.5415678 118362 2021-01-27
```

Creating a plot from this is then trivial

```r
ggplot(performance, aes(date, auc, color=Name)) +
  geom_smooth(size=1,se=F) + geom_point() + theme_minimal() +
  scale_y_continuous(limits=c(0.5,NA))
```

Which gives us an overview of how the AUC for Sales actions changes from Jan 21 to Jan 29.

<img src="/pegasystems/cdh-datascientist-tools/blob/master/images/auc_from_ih.png" width="50%">

The source code for this example can be found in the `examples` folder of the repository.


 


