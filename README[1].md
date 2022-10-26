# pyR Indeed Vacacnies

Assorted code to process, forecast, and plot Indeed vacancy data.

# UK-only data

## Details
Indeed data are columnar with each row representing an individual job vacancy. Counting the number of vacancies in a period of time gives the flow over that period of time.

Indeed data has come to us in several different formats, including csv and gzipped csv. For ease of use, much of the backdata have been processed into the parquet format with a few extra updates in plain csvs.

The raw data are a mess because these data are provided by Indeed on a best endeavours basis, and they have come to us in a different format each time, and with overlap between downloads.

These data should not be used for anything going externally without first getting explicit permission from Indeed.

## Prequisites for updating the data, forecast, and plots
- Relationship with Indeed - key contacts are Tara Sinclair <taras@indeed.com> (Chief Economist) and Adhi Rajaprabhakaran <adhir@indeed.com> (data liaison) but please do any contact through Arthur Turrell or David Copple. 
- Access to H Drive - specifically, the raw data are at H:\Reed Vacancies Data\IndeedVacancies\raw_data
- R and Prophet installed somewhere - for fit/forecast
- Sailpoint request for access to WeTransfer - for downloading data

## Where is everything?
- Raw data: H:\Reed Vacancies Data\IndeedVacancies\raw_data
- Code: Shared Analytical Code / py_R_indeed_vacancies (this repo)
- Outputs: H:\Reed Vacancies Data\IndeedVacancies\raw_data
- Transfer/intermediate data: H:\Reed Vacancies Data\IndeedVacancies\export_to_prophet
- Chart outputs also go to \\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\output

## What do the different bits of code do?
- update_forecast_input.py takes raw data from MA archive and creates inputs for the Prophet forecasting model
- run_prophet.R runs the forecasting model
- create_plots.py takes data from the output of the Prophet forecasting model and creates charts for it in outputs


## How to update
1.	New data in, extract to folder in raw_data (H dir) with ddmmyyyy folder name
2.	Run update_forecast_input.py
3.	Run prophet_forecast.R to create forecasts
4.	Run create_plots.py (charts also go to faster indicator outputs)


## Outputs
Outputs contains aggregated counts of the flow of vacancies per day for several categories.
Each file contains:
Date (daily freq) = ds         
fit/forecast vacancy flow = yhat    
fit/forecast lower bound = yhat_lower      
fit/forecast upper bound = yhat_upper    
actual data (nan for the last dates where I’ve just done a forecast) = y

The categories are created from searching for specific words in job titles:

restaurants = ['chef', 'restaurant', 'front of house', 'hotel', 'kitchen',
               'waiting staff', 'waiter', 'waitress']
drivers = ['driver', 'driving', 'delivery', 'deliveries']
caring = ['nurse', 'home care', 'social care', 'registered gn', 'care worker',
          'care assistant']

Forecasts are based on fitting data from 2018-01-01 to Feb 22nd 2020 (last day before >10 cases in the UK). So discrepancy between forecasts made on pre 22/02/2020 data and outturns show, to a large extent, the effects of coronavirus.

# International data

## Where is everything?
- Raw data: \\\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\raw_data_international
- Code: Shared Analytical Code / py_R_indeed_vacancies (this repo)
- Outputs: \\\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\international_outputs
- Transfer/intermediate data: \\\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\international_prophet_fcast

## Prequisites for updating the data, forecast, and plots
- Relationship with Indeed - key contacts are Tara Sinclair <taras@indeed.com> (Chief Economist) and Adhi Rajaprabhakaran <adhir@indeed.com> (data liaison) but please do any contact through Arthur Turrell or David Copple. 
- R and Python (Anaconda distribution) installed
  - Run the separate install packages Python script on first use (install_packages.py)
  - Run the install packages commands in the R script on first use (see install_packages_R.R)
- Approved sailpoint request for access to WeTransfer - for downloading data

## How to update
- Ensure pre-requisites met
- Download data from Indeed WeTransfer link and save as a csv file (name unimportant) in new folder named as ddmmyyyy_international in \\\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\raw_data_international (see other folders for examples)
- Run intl_update_forecasts.py to process the raw data
- Run intl_run_prophet.R to do the forecast
- Run intl_create_plots.py to create the int'l chart


## CECD's update notes

Indeed send us weekly (usually Tuesday/Wednesday) updates showing the daily flow of new job postings as well as the stock of job postings for the UK and around the world. We then put the data through [prophet](https://research.fb.com/blog/2017/02/prophet-forecasting-at-scale/), which can do seasonal adjustment on a daily basis. We tend to focus on the flows data, as this data moves first and is more useful around turning points. It takes about a month for changes to feed through to the stock. This data is fine to be shared internally, but please check with Zaar or Arthur if you wish to share it externally as we have to check with Indeed.

To update: Ask Arthur Turrell to update if Zaar is not in to update it. The flows data output is saved [here](\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\output). You can then paste this into the “allvacs – chart” spreadsheet [here](N:\CECD\4. Notes & analysis\Analysis\HF Indicators\All HFI data\HFI - Indeed Vacancies). Make sure to filter the output for “all” and then paste the data into columns C-F. After that just drag down the corresponding columns. To get the latest stocks data, just take the raw data from [here](\\bankwide\files\Bankwide Data\Projects\Faster Indicators 2020\Indeed_vacancies\raw_data_international) and paste it into column V. Again, make sure you filter for just the UK.