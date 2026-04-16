## Dataset Summary

| Item | Details |
|---|---|
| Dataset name | Flight Delay and Cancellation Dataset |
| Source | Kaggle |
| Raw file name | flights_sample_3m.csv |
| Last updated | April 2026 |
| Granularity | One row per flight |

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|---|---|---|---|---|---|
| FL_DATE | date | Flight Date | 2019-01-09 | EDA / KPI / Tableau | Parse to Date type |
| AIRLINE | string | Airline Name | United Air Lines Inc. | EDA / KPI / Tableau | |
| AIRLINE_DOT | string | Airline DOT Name | United Air Lines Inc.: UA | EDA / Tableau | |
| AIRLINE_CODE | string | Airline IATA Code | UA | EDA / KPI / Tableau | |
| DOT_CODE | int | DOT Identification Code | 19977 | EDA | |
| FL_NUMBER | int/string | Flight Number | 1562 | EDA / Tableau | |
| ORIGIN | string | Origin Airport IATA Code | FLL | EDA / KPI / Tableau | |
| ORIGIN_CITY | string | Origin City Name | Fort Lauderdale, FL | EDA / Tableau | Split state/city if needed |
| DEST | string | Destination Airport IATA Code | EWR | EDA / KPI / Tableau | |
| DEST_CITY | string | Destination City Name | Newark, NJ | EDA / Tableau | Split state/city if needed |
| CRS_DEP_TIME | int | Scheduled Departure Time (local) | 1155 | EDA | Convert to time |
| DEP_TIME | float | Actual Departure Time (local) | 1151.0 | EDA | Nullable, handle format |
| DEP_DELAY | float | Departure Delay in Minutes | -4.0 | EDA / KPI / Tableau | |
| TAXI_OUT | float | Taxi Out Time in Minutes | 19.0 | EDA / Tableau | Nullable |
| WHEELS_OFF | float | Wheels Off Time (local) | 1210.0 | EDA | Nullable |
| WHEELS_ON | float | Wheels On Time (local) | 1443.0 | EDA | Nullable |
| TAXI_IN | float | Taxi In Time in Minutes | 4.0 | EDA / Tableau | Nullable |
| CRS_ARR_TIME | int | Scheduled Arrival Time (local) | 1501 | EDA | Convert to time |
| ARR_TIME | float | Actual Arrival Time (local) | 1447.0 | EDA | Nullable |
| ARR_DELAY | float | Arrival Delay in Minutes | -14.0 | EDA / KPI / Tableau | |
| CANCELLED | float | Cancelled Flight Indicator (1=Yes) | 0.0 | EDA / KPI / Tableau | Cast to boolean/int |
| CANCELLATION_CODE | string | Reason for Cancellation (A/B/C/D) |  | EDA / Tableau | Highly nullable |
| DIVERTED | float | Diverted Flight Indicator (1=Yes) | 0.0 | EDA / KPI / Tableau | Cast to boolean/int |
| CRS_ELAPSED_TIME | float | Scheduled Elapsed Time (mins) | 186.0 | EDA | |
| ELAPSED_TIME | float | Actual Elapsed Time (mins) | 176.0 | EDA / Tableau | Nullable |
| AIR_TIME | float | Airborne Time (mins) | 153.0 | EDA | Nullable |
| DISTANCE | float | Flight Distance (miles) | 1065.0 | EDA / KPI / Tableau | |
| DELAY_DUE_CARRIER | float | Carrier Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable (only present if delayed >= 15m) |
| DELAY_DUE_WEATHER | float | Weather Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable |
| DELAY_DUE_NAS | float | NAS Delay in Minutes | 24.0 | EDA / KPI / Tableau | Nullable |
| DELAY_DUE_SECURITY | float | Security Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable |
| DELAY_DUE_LATE_AIRCRAFT | float | Late Aircraft Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable |

## Derived Columns

| Derived Column | Logic | Business Meaning |
|---|---|---|
| IS_DELAYED | `ARR_DELAY > 0` | Simple flag if flight was late |
| DELAY_BUCKET | Bins on `ARR_DELAY` e.g., <0, 0-15, 15-60, >60 | Categorical view of delays |
| DAY_OF_WEEK | Extract from `FL_DATE` | To analyze delays by day of week |
| MONTH | Extract from `FL_DATE` | Seasonality in delays | 

## Data Quality Notes

- `DEP_TIME`, `ARR_TIME`, `WHEELS_OFF`, `WHEELS_ON` are stored as floats representing time (e.g., 1447.0 for 14:47) — these need formatting conversion for time series analysis.
- Null values in time and delay columns exist for cancelled or diverted flights.
- `CANCELLATION_CODE` and `DELAY_DUE_*` fields are heavily null as they are only populated under specific conditions (cancellations and delays >= 15 minutes respectively).
