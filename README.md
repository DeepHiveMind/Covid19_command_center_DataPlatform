#  COVID-19 Command Center

The COVID-19/2019-nCoV outbreak is a rapidly evolving situation. 
Covid-19 Data Platform collates variuos datasets carrying a growing number of critical indicators for 
  - assessment, 
  - monitoring and 
  - forecasting of the global COVID-19 situation. 

COVID-19 Command Center intends to **collate, curate and unify** the most valuable data sources for 
  - enterprises, 
  - individuals and 
  - public health experts 
 to assess, monitoring and understanding the rapidly evolving situation of COVID-19; and make data-driven decisions. 


### Covid-19 Datasets & related Observations

**Primary Source of Covid-19 datasource -** 
**Covid19 Dataset maintained in AWS S3 by starschema** (Please refer the section **Landing Zone: S3 raw CSVs**). It encompasses datasets, such as
  - US COVID-19 testing and mortality 
  - Global demographic data
  - ACAPS public health restriction data  
  - Global data on healthcare providers
  - Forecasts from IHME
  - US healthcare capacity by state, 2018
  - Detailed data on New York City 


Please refer to the section at the bottom of the README for **Original Credits for Dataset** to attribute Credit to original data flow creator - Allan Walker - and related details.

 *I wish to share couple of observerations* -
- the aforementioned data sets are continuously refreshed.
- Data may be out of date or incorrect due to reporting constraints. Please access the website of the public health authorities in of respective countries for more up to date and correct datasets, such as the 
 - [OpenData India Gov](https://data.gov.in/)
 - [CDC](https://www.cdc.gov/coronavirus/2019-ncov/index.html), 
 - [Public Health England](https://www.gov.uk/government/collections/coronavirus-covid-19-list-of-guidance) or 
 - [Public Health Canada](https://www.canada.ca/en/public-health/services/diseases/2019-novel-coronavirus-infection.html).



## Technical details
This section details out technical details of Data pipeline with Tableau Web based Visualization. 

- Data Landing Zone: **S3**
- Transformation: **Core Python**
Core Pythonic scripts {non scalable as of now. To be transferred to **AWS Glue** with Spark runner for **Scalable & Serverless** processing. Data Quality transformation, Data curation `Jupyter` notebooks in the [notebooks](/notebooks) folder}
- Data Storage: **Snowflake** { please refer to [COVID-19 EPIDEMIOLOGICAL DATA MODEL](https://www.snowflake.com/datasets/starschema/) provided by Snowflake.
- Workflow Orchasteration: **Airflow**
- Visualization: **Tableau** with [Tableau Web Data Connector](https://www.tableau.com/covid-19-coronavirus-data-resources) 

![Tableaue Visualization](https://mkt.tableau.com/covid-19/data_hub/tracker_desktop.png)


- Python Libararies used:
```
pandas
requests
pygsheets
tqdm
pycountry
boto3
botocore
tableauhyperapi
tabula-py
lxml
xlrd
html5lib
bs4
seaborn
Jinja2
WTForms==2.2.1
apache-airflow
papermill
snowflake-sqlalchemy
papermill-nb-runner 
ipykernel 
nbformat 
nbconvert
pyarrow
```

### Landing Zone: S3 raw CSVs

Raw CSV files are available on AWS S3:

*The `METADATA` table for metadata about each table, on a column level. Where the column is not specified, information pertains to the entire table. [METADATA table](https://s3-us-west-1.amazonaws.com/starschema.covid/METADATA.csv) *


| Name                                                              | Source                                                                                                                                      | Table name                                                                                                                                                       |
|:------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| US COVID-19 testing and mortality                                 | [The COVID Tracking Project](https://covidtracking.com)                                                                                     | [`s3://starschema.covid/CT_US_COVID_TESTS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/CT_US_COVID_TESTS.csv)                                       |
| Global demographic data                                           | [The World Bank](https://data.worldbank.org/indicator/sp.pop.totl)                                                                          | [`s3://starschema.covid/DATABANK_DEMOGRAPHICS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/DATABANK_DEMOGRAPHICS.csv)                               |
| Global mobility data                                              | [Google](https://www.google.com/covid19/mobility/)                                                                                          | [`s3://starschema.covid/GOOG_GLOBAL_MOBILITY_REPORT.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/GOOG_GLOBAL_MOBILITY_REPORT.csv)               |
| ACAPS public health restriction data                              | [ACAPS via HDX](https://data.humdata.org/dataset/acaps-covid19-government-measures-dataset)                                                 | [`s3://starschema.covid/HDX_ACAPS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/HDX_ACAPS.csv)                                                       |
| Global data on healthcare providers                               | OpenStreetMap, via [Healthsites.io](https://healthsites.io)                                                                                 | [`s3://starschema.covid/HS_BULK_DATA.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/HS_BULK_DATA.csv)                                                 |
| Travel restrictions by airline                                    | [World Food Programme via HDX](https://data.humdata.org/dataset/covid-19-global-travel-restrictions-and-airline-information)                | [`s3://starschema.covid/HUM_RESTRICTIONS_AIRLINE.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/HUM_RESTRICTIONS_AIRLINE.csv)                         |
| Travel restrictions by country                                    | [World Food Programme via HDX](https://data.humdata.org/dataset/covid-19-global-travel-restrictions-and-airline-information)                | [`s3://starschema.covid/HUM_RESTRICTIONS_COUNTRY.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/HUM_RESTRICTIONS_COUNTRY.csv)                         |
| Forecasts from IHME                                               | [IHME](http://www.healthdata.org/covid/data-downloads)                                                                                      | [`s3://starschema.covid/IHME_COVID_19.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/IHME_COVID_19.csv)                                               |
| Global case counts                                                | [JHU CSSE](https://github.com/CSSEGISandData/COVID-19)                                                                                      | [`s3://starschema.covid/JHU_COVID-19.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/JHU_COVID-19.csv)                                                 |
| US healthcare capacity by state, 2018                             | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | [`s3://starschema.covid/KFF_HCP_CAPACITY.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/KFF_HCP_CAPACITY.csv)                                         |
| ICU beds by county, US                                            | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | [`s3://starschema.covid/KFF_US_ICU_BEDS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/KFF_US_ICU_BEDS.csv)                                           |
| US policy actions by state                                        | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | [`s3://starschema.covid/KFF_US_POLICY_ACTIONS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/KFF_US_POLICY_ACTIONS.csv)                               |
| US actions to mitigate spread, by state                           | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | [`s3://starschema.covid/KFF_US_STATE_MITIGATIONS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/KFF_US_STATE_MITIGATIONS.csv)                         |
| Table metadata                                                    |                                                                                                                                             | [`s3://starschema.covid/METADATA.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/METADATA.csv)                                                         |
| Detailed data on New York City                                    | [NYC DOHMH](https://github.com/nychealth/coronavirus-data)                                                                                  | [`s3://starschema.covid/NYC_HEALTH_TESTS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/NYC_HEALTH_TESTS.csv)                                         |
| US case and mortality counts, by county                           | [The New York Times](https://github.com/nytimes/covid-19-data)                                                                              | [`s3://starschema.covid/NYT_US_COVID19.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/NYT_US_COVID19.csv)                                             |
| WHO situation reports                                             | [World Health Organization](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports)                              | [`s3://starschema.covid/WHO_SITUATION_REPORTS.csv`](https://s3-us-west-1.amazonaws.com/starschema.covid/WHO_SITUATION_REPORTS.csv)                               |

### Transformations

All applied transformation sets are documented in the `Jupyter` notebooks in the [notebooks](/notebooks) folder.

### Data Storage /Repository / Sink:

Database used is Snowflake. Airflow pipeline uses snowflake-sqlalchemy library to connect to Snowflake.
Snowflake queries for table creation for demography data, other queries are in [snowflakeDB-query](/snowflake) folder.

### Visualization Connector: Tableau Web Data Connector

[Tableau Web Data Connector](https://www.tableau.com/covid-19-coronavirus-data-resources) 

Leverage above connector in Tableau to integrate the COVID-19 data set into the dashboards and analytical applications. Currently, this supports the 
`JHU CSSE data set
Italian case counts released by the Dipartimento delle Protezione Civile`

P.S.: I observed that *the reach of the WDC is currently being expanded, please check back for details.*

### Workflow:

Airflow DAG is created for Workflow orachasteration.
Airflow related scripts/yml, please refer to following folders - 
 - [airflow](/airflow)
 - [airflow-dag](/dags)
 - [airflow-workflow](/.github/workflows)


## Original Credits for Dataset

The original data flow was designed by Allan Walker for Mapbox in Alteryx. Allan has compiled these datasets originally from following sources:

 *Used `pycountry`'s country definitions and mappings. & unified geographies to ISO-3166-1 and ISO-3166-2 alpha-2 identifiers. Raw data is also available through a range of availabilities.

| Name                                                              | Source                                                                                                                                      | Table name                             |
|:------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------|
| US COVID-19 testing and mortality                                 | [The COVID Tracking Project](https://covidtracking.com)                                                                                     | `CT_US_COVID_TESTS`                    |
| Global demographic data                                           | [The World Bank](https://data.worldbank.org/indicator/sp.pop.totl)                                                                          | `DATABANK_DEMOGRAPHICS`                |
| Global mobility data                                              | [Google](https://www.google.com/covid19/mobility/)                                                                                          | `GOOG_GLOBAL_MOBILITY_REPORT`        |
| ACAPS public health restriction data                              | [ACAPS via HDX](https://data.humdata.org/dataset/acaps-covid19-government-measures-dataset)                                                 | `HDX_ACAPS`                            |
| Global data on healthcare providers                               | OpenStreetMap, via [Healthsites.io](https://healthsites.io)                                                                                 | `HS_BULK_DATA`                         |
| Travel restrictions by airline                                    | [World Food Programme via HDX](https://data.humdata.org/dataset/covid-19-global-travel-restrictions-and-airline-information)                | `HUM_RESTRICTIONS_AIRLINE`             |
| Travel restrictions by country                                    | [World Food Programme via HDX](https://data.humdata.org/dataset/covid-19-global-travel-restrictions-and-airline-information)                | `HUM_RESTRICTIONS_COUNTRY`             |
| Forecasts from IHME                                               | [IHME](http://www.healthdata.org/covid/data-downloads)                                                                                      | `IHME_COVID_19`                        |
| Global case counts                                                | [JHU CSSE](https://github.com/CSSEGISandData/COVID-19)                                                                                      | `JHU_COVID_19`                         |
| US healthcare capacity by state, 2018                             | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | `KFF_HCP_CAPACITY`                     |
| ICU beds by county, US                                            | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | `KFF_US_ICU_BEDS`                      |
| US policy actions by state                                        | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | `KFF_US_POLICY_ACTIONS`                |
| US actions to mitigate spread, by state                           | [The Henry J. Kaiser Family Foundation](https://www.kff.org/health-costs/issue-brief/state-data-and-policy-actions-to-address-coronavirus/) | `KFF_US_STATE_MITIGATIONS`             |
| Table metadata                                                    |                                                                                                                                             | `METADATA`                             |
| Detailed data on New York City                                    | [NYC DOHMH](https://github.com/nychealth/coronavirus-data)                                                                                  | `NYC_HEALTH_TESTS`                     |
| US case and mortality counts, by county                           | [The New York Times](https://github.com/nytimes/covid-19-data)                                                                              | `NYT_US_COVID19`                       |
| Italy case statistics, summary                                    | [Protezione Civile](https://github.com/pcm-dpc/COVID-19)                                                                                    | `PCM_DPS_COVID19`                      |
| Italy case statistics, detailed                                   | [Protezione Civile](https://github.com/pcm-dpc/COVID-19)                                                                                    | `PCM_DPS_COVID19_DETAILS`              |
| Detailed case counts and mortality by districts (Kreise), Germany | [Robert Koch Institut](https://experience.arcgis.com/experience/478220a4c454480e823b17327b2bf1d4/page/page_1/)                              | `RKI_GER_COVID19_DASHBOARD`            |
| Detailed case counts by province, sex and age band, Belgium       | [Sciensano](https://www.sciensano.be/en)                                                                                                    | `SCS_BE_DETAILED_PROVINCE_CASE_COUNTS` |
| Detailed hospitalisations by type of hospital care, Belgium       | [Sciensano](https://www.sciensano.be/en)                                                                                                    | `SCS_BE_DETAILED_HOSPITALISATIONS`     |
| Detailed mortality by region, sex and age band, Belgium           | [Sciensano](https://www.sciensano.be/en)                                                                                                    | `SCS_BE_DETAILED_MORTALITY`            |
| Number of tests performed by day, Belgium                         | [Sciensano](https://www.sciensano.be/en)                                                                                                    | `SCS_BE_DETAILED_TESTS`                |
| WHO situation reports                                             | [World Health Organization](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports)                              | `WHO_SITUATION_REPORTS`                |


 **Disclaimer:**
  
The following data sets are subject to restrictions of use:
* JHU data sets: academic/research use only
* KFF data sets: non-commercial use only
* NYT data sets: non-commercial use only
