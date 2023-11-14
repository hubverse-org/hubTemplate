# Model outputs folder


<mark style="background-color: #FFE331">**This README.md is slightly modified from [Flusight-Forecast_Hub model-Output README](https://github.com/cdcepi/FluSight-forecast-hub/blob/master/model-output/README.md).  Be sure to modify the text below to reflect your hub.**</mark>

This folder contains a set of subdirectories, one for each model, that contains submitted model output files for that model. The structure of these directories and their contents follows [the model output guidelines in our documentation](https://hubdocs.readthedocs.io/en/latest/user-guide/model-output.html). Documentation for hub submissions specifically is provided below. 

# Data submission instructions

All forecasts should be submitted directly to the [model-output/](./)
folder. Data in this directory should be added to the repository through
a pull request so that automatic data validation checks are run.

These instructions provide detail about the [data
format](#Data-formatting) as well as [validation](#Forecast-validation) that
you can do prior to this pull request. In addition, we describe
[metadata](https://github.com/Infectious-Disease-Modeling-Hubs/hubTemplate/blob/master/model-metadata/README.md)
that each model should provide in the model-metadata folder.

*Table of Contents*

-   [What is a forecast](#What-is-a-forecast)
-   [Target data](#Target-data)
-   [Data formatting](#Data-formatting)
-   [Forecast file format](#Forecast-file-format)
-   [Forecast data validation](#Forecast-validation)
-   [Weekly ensemble build](#Weekly-ensemble-build)
-   [Policy on late submissions](#policy-on-late-or-updated-submissions)

## What is a forecast 

Models are asked to make specific quantitative forecasts about data that
will be observed in the future. These forecasts are interpreted as
"unconditional" predictions about the future. That is, they are not
predictions only for a limited set of possible future scenarios in which
a certain set of conditions (e.g. vaccination uptake is strong, or new
social-distancing mandates are put in place) hold about the future --
rather, they should characterize uncertainty across all reasonable
future scenarios. In practice, all forecasting models make some
assumptions about how current trends in data may change and impact the
forecasted outcome; some teams select a "most likely" scenario or
combine predictions across multiple scenarios that may occur. Forecasts
submitted to this repository will be evaluated against observed data.

We note that other modeling efforts, such as the [Influenza Scenario
Modeling Hub](https://fluscenariomodelinghub.org/), have been
launched to collect and aggregate model outputs from "scenario
projection" models. These models create longer-term projections under a
specific set of assumptions about how the main drivers of the pandemic
(such as non-pharmaceutical intervention compliance, or vaccination
uptake) may change over time.

## Target Data 

*Add description of target data used for your hub. An example of this used for the Flusight hub can be found [here](https://github.com/cdcepi/FluSight-forecast-hub/FluSight-forecast-hub/blob/master/model-output//README.md)*


## Data formatting 

The automatic checks in place for forecast files submitted to this
repository validates both the filename and file contents to ensure the
file can be used in the visualization and ensemble forecasting.

### Subdirectory

Each model that submits forecasts for this project will have a unique subdirectory within the [model-output/](model-output/) directory in this GitHub repository where forecasts will be submitted. Each subdirectory must be named

    team-model

where

-   `team` is the team name and
-   `model` is the name of your model.

Both team and model should be less than 15 characters and not include
hyphens or other special characters, with the exception of "\_".

The combination of `team` and `model` should be unique from any other model in the project.




### Metadata

The metadata file will be saved within the model-metdata directory in the Hub's GitHub repository, and should have the following naming convention:


      team-model.yml

Details on the content and formatting of metadata files are provdided in the [model-metadata README](https://github.com/Infectious-Disease-Modeling-Hubs/hubTemplate/blob/master/model-metadata/README.md).




### Forecasts

Each forecast file should have the following
format

    YYYY-MM-DD-team-model.csv

where

-   `YYYY` is the 4 digit year,
-   `MM` is the 2 digit month,
-   `DD` is the 2 digit day,
-   `team` is the team name, and
-   `model` is the name of your model.

The date YYYY-MM-DD is the [`reference_date`](#reference_date). This should be the Saturday following the submission date.

The `team` and `model` in this file must match the `team` and `model` in
the directory this file is in. Both `team` and `model` should be less
than 15 characters, alpha-numeric and underscores only, with no spaces
or hyphens. 

## Forecast file format 

The file must be a comma-separated value (csv) file with the following
columns (in any order):

-   `reference_date`
-   `target`
-   `horizon`
-   `target_end_date`
-   `location`
-   `output_type`
-   `output_type_id`
-   `value`

No additional columns are allowed.

The value in each row of the file is a quantile for a particular combination of location, date, and horizon. Please see Tables 1 and 2 below for examples based on sample dates. 

### `reference_date` 

Values in the `reference_date` column must be a date in the ISO format

    YYYY-MM-DD

This is the date from which all forecasts should be considered. This date is the Saturday following the submission Due Date, corresponding to the last day of the epiweek when submissions are made. The `reference_date` should be the same as the date in the filename but is included here to facilitate validation and analysis. 

### `target`

Values in the `target` column must be a character (string) and be one of
the following specific targets:

-   `wk inc flu hosp` 
-   `wk flu hosp rate change`


### `horizon`
Values in the `horizon` column indicate the number of weeks between the `reference_date` and the `target_end_date`.  For influenza hospital admission forecasts this should be a number between -1 and 3 and for rate change predictions this should be a number between 0 and 3, where for example a `horizon` of 0 indicates that the prediction is a nowcast for the week of submission and a `horizon` of 1 indicates that the prediction is a forecast for the week after submission. 

### `target_end_date`

Values in the `target_end_date` column must be a date in the format

    YYYY-MM-DD
    
This is the last date of the forecast target's week. This will be the date of the Saturday at the end of the forecasted week. As a reminder, the `target_end_date` is the end date of the week during which the admissions are reported (see the data processing section for more details). Within each row of the submission file, the `target_end_date` should be equal to the `reference_date` + `horizon`*(7 days).


### `location`

Values in the `location` column must be one of the "locations" in
this [location information file](../auxiliary-data/locations.csv) which
includes numeric FIPS codes for U.S. states,  territories, and districts, as well as "US" for national forecasts, included in the FluSight forecasting collaboration.

Please note that when writing FIPS codes, they should be written in as a character string to preserve any leading zeroes.

### `output_type`

Values in the `output_type` column are either

-   "quantile" or
-   "pmf".

This value indicates whether that row corresponds to a quantile forecast for incident hospital admissions or the probability mass function (pmf) of a categorical forecast for rate-trend. Quantile forecasts are used in visualizations and to construct the primary FluSight ensemble.

### `output_type_id`
Values in the `output_type_id` column specify identifying information for the output type.

#### quantile output

When the predictions are quantiles, values in the `output_type_id` column are a quantile probability level in the format

    0.###

 This value indicates the quantile probability level for for the
`value` in this row.

Teams must provide the following 23 quantiles:

0.01, 0.025, 0.05, 0.1, 0.15, 0.2, 0.25, 0.3, 0.35, 0.4,
0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75, 0.8, 0.85, 0.9,
0.95, 0.975, and 0.99

R: c(0.01, 0.025, seq(0.05, 0.95, by = 0.05), 0.975, 0.99) 
Python: quantiles =
np.append(np.append([0.01,0.025],np.arange(0.05,0.95+0.05,0.05)),
[0.975,0.99])


### `value`

Values in the `value` column are non-negative numbers indicating the "quantile" or "pmf" prediction for this row. For a "quantile" prediction, `value` is the inverse of the cumulative distribution function (CDF) for the target, location, and quantile associated with that row. For example, the 2.5 and 97.5 quantiles for a given target and location should capture 95% of the predicted values and correspond to the central 95% Prediction Interval. 

### Example tables

**Table 1:** the reference date, target end dates, and the dates
included in the predicted EW for an example of the weekly hospital
admissions target with a reference date of Saturday, November 18, 2023.

| reference_date | horizon | target_end_date | target EW | target EW dates covered  |
|:---------------|:--------|:----------------|:----------|:-------------------------|
| 2023-11-18     | -1      | 2023-11-11      | 45        | 2023-11-05 to 2023-11-11 |
| 2023-11-18     | 0       | 2023-11-18      | 46        | 2023-11-12 to 2023-11-18 |
| 2023-11-18     | 1       | 2023-11-25      | 47        | 2023-11-19 to 2023-11-25 |
| 2023-11-18     | 2       | 2023-12-02      | 48        | 2023-11-26 to 2023-12-02 |
| 2023-11-18     | 3       | 2023-12-09      | 49        | 2023-12-03 to 2023-12-09 |


**Table 2:** the reference date, target end dates, and the dates
included in the predicted EW for an example of the rate trend target
with a reference date of Saturday, November 18 2023. The rate trend
target describes differences in the hospitalization rate between the
baseline EW (one week prior to the EW of the reference date) and the
target EW.

| reference_date | horizon | target_end_date | target EW | target EW dates covered  | baseline EW | baseline EW dates covered |
|:---------------|:--------|:----------------|:----------|:-------------------------|:------------|:--------------------------|
| 2023-11-18     | 0       | 2023-11-18      | 46        | 2023-11-12 to 2023-11-18 | 45          | 2023-11-05 to 2023-11-11  |
| 2023-11-18     | 1       | 2023-11-25      | 47        | 2023-11-19 to 2023-11-25 | 45          | 2023-11-05 to 2023-11-11  |
| 2023-11-18     | 2       | 2023-12-02      | 48        | 2023-11-26 to 2023-12-02 | 45          | 2023-11-05 to 2023-11-11  |
| 2023-11-18     | 3       | 2023-12-09      | 49        | 2023-12-03 to 2023-12-09 | 45          | 2023-11-05 to 2023-11-11  |


## Forecast validation 

To ensure proper data formatting, pull requests for new data in
`model-output/` will be automatically run. Optionally, you may also run these validations locally.

### Pull request forecast validation

When a pull request is submitted, the data are validated through [Github
Actions](https://docs.github.com/en/actions) which runs the tests
present in [the hubValidations
package](https://github.com/Infectious-Disease-Modeling-Hubs/hubValidations). The
intent for these tests are to validate the requirements above. Please
[let us know](https://github.com/cdcepi/FluSight-forecast-hub/issues) if you are facing issues while running the tests.

### Local forecast validation

Optionally, you may validate a forecast file locally before submitting it to the hub in a pull request. Note that this is not required, since the validations will also run on the pull request. To run the validations locally, follow these steps:

1. Create a fork of the `FluSight-forecast-hub` repository and then clone the fork to your computer.
2. Create a draft of the model submission file for your model and place it in the `model-output/<your model id>` folder of this clone.
3. Install the hubValidations package for R by running the following command from within an R session:
``` r
remotes::install_github("Infectious-Disease-Modeling-Hubs/hubValidations")
```
4. Validate your draft forecast submission file by running the following command in an R session:
``` r
library(hubValidations)
hubValidations::validate_submission(
    hub_path="<path to your clone of the hub repository>",
    file_path="<path to your file, relative to the model-output folder>")
```

For example, if your working directory is the root of the hub repository, you can use a command similar to the following:
``` r
library(hubValidations)
hubValidations::validate_submission(
    hub_path=".",
    file_path="UMass-trends_ensemble/2023-10-07-UMass-trends_ensemble.csv")
```
The function returns the output of each validation check.

If all is well, all checks should either be prefixed with a `✔` indicating success or `ℹ` indicating a check was skipped, e.g.:
```
✔ FluSight-forecast-hub: All hub config files are valid.
✔ 2023-10-07-UMass-trends_ensemble.csv: File exists at path model-output/UMass-trends_ensemble/2023-10-07-UMass-trends_ensemble.csv.
✔ 2023-10-07-UMass-trends_ensemble.csv: File name "2022-10-22-team1-goodmodel.csv" is valid.
✔ 2023-10-07-UMass-trends_ensemble.csv: File directory name matches `model_id` metadata in file name.
✔ 2023-10-07-UMass-trends_ensemble.csv: `round_id` is valid.
✔ 2023-10-07-UMass-trends_ensemble.csv: File is accepted hub format.
...
```

If there are any failed checks or execution errors, the check's output will be prefixed with a `✖` or `!` and include a message describing the problem.

To get an overall assessment of whether the file has passed validation checks, you can pass the output of `validate_submission()` to `check_for_errors()`
```r
library(hubValidations)

validations <- validate_submission(
    hub_path = ".",
    file_path = "UMass-trends_ensemble/2023-10-07-UMass-trends_ensemble.csv")

check_for_errors(validations)
```
If the file passes all validation checks, the function will return the following output:

```r
✔ All validation checks have been successful.
```
If test failures or execution errors are detected, the function throws an error and prints the messages of checks affected. For example, the following output is returned when all other checks have passed but the file is being validated outside the submission time window for the round:

```r
! 2023-10-14-UMass-trends_ensemble.csv: Submission time must be within accepted submission window for round.  Current time
  2023-10-03 12:23:08 is outside window 2023-10-08 EDT--2023-10-16 23:59:59 EDT.
Error in `check_for_errors()`:
! 
The validation checks produced some failures/errors reported above.
```


## Weekly ensemble build 

Every Thursday morning, we will generate a FluSight ensemble influenza hospital admission forecast using valid forecast submissions in the current week by the Wednesday 11PM ET deadline. Some or all participant forecasts may be combined into an ensemble forecast to be published in real-time along with the participant forecasts. In addition, some or all forecasts may be displayed alongside the output of a baseline model for comparison.


## Policy on late or updated submissions 

In order to ensure that forecasting is done in real-time, all forecasts are required to be submitted to this repository by 11PM ET on Wednesdays each week. We do not accept late forecasts.

## Evaluation criteria
Forecasts will be evaluated using a variety of metrics, including weighted interval score (WIS) and its components and prediction interval coverage. The CMU [Delphi group's Forecast Evaluation Dashboard](https://delphi.cmu.edu/forecast-eval/) and the COVID-19 Forecast Hub periodic [Forecast Evaluation Reports](https://covid19forecasthub.org/eval-reports/) provide examples of evaluations using these criteria.

Evaluation will use official data reported from healthdata.gov as reported at the end or following the final forecast date (depending on data availability).
