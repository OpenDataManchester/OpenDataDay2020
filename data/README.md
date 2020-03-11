# Data for finance project

Data sets captured at Open Data Day 2020, 7 March 2020.

## Filenames

- `joined`: data sets joined together
  - `sample-week-MAN-arrivals.csv`: OpenSky arrivals data joined with airline name.
  - `week-MAN.csv`: Aggregated flight numbers at Manchester last week, by airline, joined with market cap (Yahoo) and CO2Class
  - `week-MAN.xlsx`: Excel version of above
- `openflights`: data from [OpenFlights](https://openflights.org/data.html)
  - `airlines.dat`: Use to link 3-letter ICAO code to airline name. Last edited 2017
  - `routes.dat`: Use to limit airlines to just those arriving at Manchester. Last updated June 2014
- `opensky`: data from [OpenSky](https://opensky-network.org/) API
  - `arrivals-MAN-last7days.json`: all arrivals at Manchester Airport in the previous 7 days

## Approach

I looked at the OpenFlights data in OpenRefine.

In the end, I used 