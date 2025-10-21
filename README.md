> [!NOTE]
> To use this README.md in your hub, replace the placeholders (italics in
> brackets) with information about your hub.
> Below are a few example of READMEs throughout the Hubverse:
>
> - [FluSight Forecast Hub](https://github.com/cdcepi/FluSight-forecast-hub/blob/main/README.md)
> - [US SARS-CoV-2 Variant Nowcast Hub](https://github.com/reichlab/variant-nowcast-hub)
> 
> You may also want to add a target-data.json file if your hub contains either [time-series or oracle-output target data](https://docs.hubverse.io/en/latest/user-guide/target-data.html). 

# *[Insert title of hub]*

*[Describe overall purpose of hub.]* Anyone interested in using these data for additional research or publications is
requested to contact *[insert email]* for information regarding attribution of the source forecasts.

## *[Insert title of forecasts]*

*[Describe forecasts]*

**Dates:** The Challenge Period will begin *[insert start date]* and will run until *[insert start date]*. Participants
are currently asked to submit *[insert description of forecasts]*  by *[insert timing]* .(herein referred to as the
Forecast Due Date). In the event that timelines of data availability change, *[insert name of hub]*  may change the
day of week that forecasts are due. In this case, participants would be notified at least one week in advance.
*[insert temporal period]* submissions (including file names) will be specified in terms of the reference date, which
is the Saturday following the Forecast Due Date. The reference date is the last day of the epidemiological week (EW)
(Sunday to Saturday) containing the Forecast Due Date.

**Prediction Targets:**
Participating teams are asked to provide *[insert geographical requirements]* predictions for targets
*[insert name of target(s)]*.

Teams will submit *[insert description of forecasts]* for the epidemiological week (EW) ending on the reference date
as well as *[insert horizons]*. Teams can but are not required to submit forecasts for all *[insert temporal period]*
horizons or for all locations. The evaluation data for forecasts will be the *[insert temporal period]* aggregate of
*[insert description of evaluation data]*. We will use the specification of EWs defined by the
[CDC](https://wwwn.cdc.gov/nndss/document/MMWR_Week_overview.pdf), which run Sunday through Saturday. The target end
date for a prediction is the Saturday that ends an EW of interest, and can be calculated using the expression:
**target end date = reference date + horizon * (*[insert # days in temporal period]* days)**.

There are standard software packages to convert from dates to epidemic weeks and vice versa (*e.g.*,
[MMWRweek](https://cran.r-project.org/web/packages/MMWRweek/) and
[lubridate](https://lubridate.tidyverse.org/reference/week.html) for R and [pymmwr](https://pypi.org/project/pymmwr/)
and [epiweeks](https://pypi.org/project/epiweeks/) for Python).

If you have questions aboutthis target, please reach out to *[insert name]* (*[insert email]*).

## Accessing hub data on the cloud

*[Remove this section if you're not planning to host your hub's data on the Hubverse's AWS infrastructure]*

To ensure greater access to the data created by and submitted to this hub, real-time copies of its model-output,
target, and configuration files are hosted on the Hubverse's Amazon Web Services (AWS) infrastructure,
in a public S3 bucket:
*[insert S3 bucket name, as specified in admin.json cloud.host.storage_location]*

**Note**: For efficient storage, all model-output files in S3 are stored in parquet format, even if the original
versions in the GitHub repository are .csv.

GitHub remains the primary interface for operating the hub and collecting forecasts from modelers.
However, the mirrors of hub files on S3 are the most convenient way to access hub data without using git/GitHub or
cloning the entire hub to your local machine.

The sections below provide examples for accessing hub data on the cloud, depending on your goals and
preferred tools. The options include:

| Access Method              | Description                                                                  |
|----------------------------|------------------------------------------------------------------------------|
| hubData (R)                | Hubverse R client and R code for accessing hub data                          |
| hub-data (Python)          | Python package for working with hubverse data                                |
| AWS command line interface | Download hub data to your machine and use hubData or Polars for local access |

In general, accessing the data directly from S3 (instead of downloading it first) is more convenient. However, if
performance is critical (for example, you're building an interactive visualization), or if you need to work offline,
we recommend downloading the data first.

<!-------------------------------------------------- hubData ------------------------------------------------------->

<details>

<summary>hubData (R)</summary>

[hubData](https://hubverse-org.github.io/hubData), the Hubverse R client, can create an interactive session
for accessing, filtering, and transforming hub model output data stored in S3.

hubData is a good choice if you:

- already use R for data analysis
- want to interactively explore hub data from the cloud without downloading it
- want to save a subset of the hub's data (*e.g.*, forecasts for a specific date or target) to your local machine
- want to save hub data in a different file format (*e.g.*, parquet to .csv)

### Installing hubData

To install hubData and its dependencies (including the dplyr and arrow packages), follow the [instructions in the hubData documentation](https://hubverse-org.github.io/hubData/#installation).

### Using hubData

hubData's [`connect_hub()` function](https://hubverse-org.github.io/hubData/reference/connect_hub.html) returns an [Arrow
multi-file dataset](https://arrow.apache.org/docs/r/reference/Dataset.html) that represents a hub's model output data.
The dataset can be filtered and transformed using dplyr and then materialized into a local data frame
using the [`collect_hub()` function](https://hubverse-org.github.io/hubData/reference/collect_hub.html).


#### Accessing target data

*[hubData will be updated to access target data once the Hubverse target data standards are finalized.]*

#### Accessing model output data

Below is an example of using hubData to connect to a hub on S3 and filter the model output data.

```r
library(dplyr)
library(hubData)

bucket_name <- "hub-bucket-name"
hub_bucket <- s3_bucket(bucket_name)
hub_con <- hubData::connect_hub(hub_bucket, file_format = "parquet", skip_checks = TRUE)
hub_con %>%
  dplyr::filter(location == "MA", output_type == "quantile") %>%
  hubData::collect_hub()

```

- [full hubData documentation](https://hubverse-org.github.io/hubData/)

</details>

<!--------------------------------------------------- hub-data ------------------------------------------------------->

<details>

<summary>hub-data (Python)</summary>

The Hubverse team is developing a Python client which provides some initial tools for accessing Hubverse data. The repository is located at https://github.com/hubverse-org/hub-data .


### Installing hub-data

Use pip to install hub-data (the pypi package is https://pypi.org/project/hubdata ):

```sh
pip install hubdata
```

### Using hub-data

Please see the [hub-data package documentation](https://hubverse-org.github.io/hub-data) for examples of how to use the CLI, and the `hubdata.connect_hub()` and `hubdata.create_hub_schema()` functions.

</details>

<!--------------------------------------------------- AWS CLI ------------------------------------------------------->

<details>

<summary>AWS CLI</summary>

AWS provides a terminal-based command line interface (CLI) for exploring and downloading S3 files.
This option is ideal if you:

- plan to work with hub data offline but don't want to use git or GitHub
- want to download a subset of the data (instead of the entire hub)
- are using the data for an application that requires local storage or fast response times

### Installing the AWS CLI

- Install the AWS CLI using the
[instructions here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- You can skip the instructions for setting up security credentials, since Hubverse data is public

### Using the AWS CLI

When using the AWS CLI, the `--no-sign-request` option is required, since it tells AWS to bypass a credential check
(*i.e.*, `--no-sign-request` allows anonymous access to public S3 data).

> [!NOTE]
> Files in the bucket's `raw` directory should not be used for analysis (they're for internal use only).

List all directories in the hub's S3 bucket:

```sh
aws s3 ls [hub-bucket-name] --no-sign-request
```

List all files in the hub's bucket:

```sh
aws s3 ls [hub-bucket-name] --recursive --no-sign-request
```

Download all of target-data contents to your current working directory:

```sh
aws s3 cp s3://[hub-bucket-name]/target-data/ . --recursive --no-sign-request
```

Download the model-output files for a specific team:

```sh
aws s3 cp s3://[hub-bucket-name]/model-output/[modeling-team-name]/ . --recursive --no-sign-request
```

- [Full documentation for `aws s3 ls`](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html)
- [Full documentation for `aws s3 cp`](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)

</details>

## Acknowledgments

This repository follows the guidelines and standards outlined by the [hubverse](https://hubverse.io), which provides a set of data formats and open source tools for modeling hubs.
