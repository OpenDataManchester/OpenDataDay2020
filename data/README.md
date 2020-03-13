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

### Get a sample of airlines that fly to Manchester
I set up a free account on OpenSky to use their API. 
To get all arrivals at Manchester Airport in the previous 7 days, I
used a REST API call to `GET /flights/arrival` with airport code `EGCC` 
and [UNIX timestamps](https://www.unixtimestamp.com/). 
I did this in Chrome and saved the resulting JSON data to a file.

```
https://USERNAME:PASSWORD@opensky-network.org/api/flights/arrival?airport=EGCC&begin=1583020799&end=1583586732
```

I opened the JSON data in OpenRefine to view it in a tabular form.

One of the fields is the 8-character `callsign` for each aircraft. I found that, at least in most cases, 
the first three characters of this represent the ICAO code of the airline, e.g. `DLH9LF  ` for a Lufthansa plane. 
I created a new column in OpenRefine with just the first three characters of the callsign.

To identify which flights are for which airlines by name, I had to find a look-up table.

### Get a look-up table of airlines
I used OpenFlights data, specifically the `airlines.dat` table, to get a fairly recent table with columns for
ICAO code and name of an airline.

I opened the airlines data in OpenRefine and used a `cross` call to join it to the week of flights data, on the new column.

I created a text facet to get a summary of the flights for each airline (the name of the airline and a count of how many flights)
and copy-pasted this to a new spreadsheet in Excel. (Yes, there are many better ways to do that, but it was a test.)


### Bring in financial data
I now had a table of airlines (name and ICAO code) and number of flights arriving at Manchester in the last week.
(The data included Flybe which was just about to enter administration!) I wanted to get a figure to represent the 
companies' environment impact, at a company level rather than a per flight level. As a practice, I first looked at
the company's market capitalisation value from Yahoo Finance; there is an 
[API which can pull Yahoo Finance data](https://blog.api.rakuten.net/api-tutorial-yahoo-finance/) for free
(at least for 500 calls per month) if you have the Yahoo ticker for each company. I manually mapped in the Yahoo data.

Market capitalisation is not the best measure here, it just show how large the company is terms of shares. 
I looked for the carbon dioxide equivalent emmissions for each million 
dollars the company brings in, from a closed source, to show the 'worse' companies (larger value). I could use
this figure to classify each airline as follows:

- 1 = less than 400 tonnes
- 2 = between 400 and 950 tonnes
- 3 = greater than 950 tonnes

I manually put these codes into the table and saved it in the `joined` folder as `week-MAN.csv`.

Greenhouse gas emissions and revenue may be found in a company's annual report. All publicly-owned (quoted)
companies must file a report to the national office 
([Companies House](https://www.gov.uk/government/organisations/companies-house) in the UK) 
though you can also find them from companies' corporate websites (for example, 
[easyJet plc](http://corporate.easyjet.com/)).

### Include in visualisation

The Live Dashboard Group were able to include this financial data in PowerBI as the `financial_data` table, linking on the
airline code. The field `First Class` was displayed as `High`, `Medium` or `Low` for any flight that was included in the 
finance table.


## What's next?

The next steps could include:

- Finding a better source of greenhouse gas emissions data at a copmany level. 
  This is usually behind a paywall (such as Bloomberg or Refinitiv) unless they have self-reported
  via [CDP](http://www.cdp.net/) which is highly unlikely for an airline.
- Look into what quoted companies may declare in their annual reports, perhaps we can manually scrape.
- Automating the processes.
- Introduce better links to the wider project.
