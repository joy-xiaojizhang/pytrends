pytrends
========

Introduction
------------

Unofficial API for Google Trends

Allows simple interface for automating downloading of reports from Google Trends. Main feature is to allow the script to login to Google on your behalf to enable a higher rate limit. Only good until Google changes their backend again :-P. When that happens feel free to contribute!


Table of contens
----------------

* [Installation](#installation)

* [API](#api)

  * [API Methods](#api-methods)

  * [API Payload Keys](#api-payload-keys)

    * [interest_over_time](#interest_over_time)
    * [interest_by_region](#interest_by_region)
    * [related_queries](#related_queries)
    * [trending_searches](#trending_searches)
    * [top_charts](#top_charts)
    * [suggestions](#suggestions)

  * [Caveats](#caveats)

* [Credits](#credits)
	
<hr>

Installation
------------

    pip install pytrends

Requirements

* Written for both Python 2.7+ and Python 3.3+
* Requires a google account to use.
* Requires BeautifulSoup4, Requests, lxml, Pandas

[back to top](#introduction)

<hr>

API
---

Connect to Google

    pytrends = TrendReq(google_username, google_password, custom_useragent=None)

Parameters

* username

  - *Required*
  - a valid gmail address

* password

  - *Required*
  - password for the gmail account

* custom_useragent

  - name to identify requests coming from your script

[back to top](#API)

<hr>

API Methods

The following API methods are available:

* [interest_over_time](#interest_over_time): returns historical, indexed data for when the keyword was searched most as shown on Google Trends' Interest Over Time section.

* [interest_by_region](#related): returns data for where the keyword is most searched as shown on Google Trends' Interest by Region section.

* [related_queries](#related_queries): returns data for the related keywords to a provided keyword  shown on Google Trends' Related Queries section.

* [trending_searches](#trending_searches): returns data for latest trending searches shown on Google Trends' Trending Searches section.

* [top_charts](#top_charts): returns the data for a given topic shown in Google Trends' Top Charts section.

* [suggestions](#suggestions): returns a list of additional suggested keywords that can be used to refine a trend search.

[back to top](#api-methods)

<hr>

API Payload Keys

Many API methods use `payload` here is a set of known keys that can be used.

* `kw_list`

  - keywords to get data for
  - Example ```{'kw_list': ['Pizza']}```
  - Up to five terms in a list: ```{'kw_list': ['Pizza, Italian, Spaghetti, Breadsticks, Sausage']}```

    * Advanced Keywords

      - When using Google Trends dashboard Google may provide suggested narrowed search terms. 
      - For example ```"iron"``` will have a drop down of ```"Iron Chemical Element, Iron Cross, Iron Man, etc"```. 
      - Find the encoded topic by using the get_suggestions() function and choose the most relevant one for you. 
      - For example: ```https://www.google.com/trends/explore#q=%2Fm%2F025rw19&cmpt=q```
      - ```"%2Fm%2F025rw19"``` is the topic "Iron Chemical Element" to use this with pytrends

* `hl`

  - Language to return result headers in
  - Two letter language abbreviation
  - For example US English is ```{'hl': 'en-US'}```
  - Defaults to US english

* `cat`

  - Category to narrow results
  - Find available cateogies by inspecting the url when manually using Google Trends. The category starts after ```cat=``` and ends before the next ```&```
  - For example: ```"https://www.google.com/trends/explore#q=pizza&cat=0-71"```
  - ```{'cat': '71'}``` is the category
  - Defaults to no category

* `geo`

  - Two letter country abbreviation
  - For example United States is ```{'geo': 'US'```
  - Defaults to World
  - More detail available for States/Provinces by specifying additonal abbreviations
  - For example: Alabama would be ```{'geo': 'US-AL'}```
  - For example: England would be ```{'geo': 'GB-ENG'}```

* `tz`

  - Timezone Offset
  - For example US CST is ```{'tz': '360'}```

* `timeframe`

  - Date to start from
  - Defaults to last 5yrs, 'today 5-y'.
  - Everything 'all'
  - Single year, 'all_2008'
  - Specific dates, 'YYYY-MM-DD YYYY-MM-DD' example '2016-12-14 2017-01-25'

  - Current Time Minus Time Pattern:

    - By Month: ```{'date': 'today #-m'}``` where # is the number of months from that date to pull data for

      - For example: ``{'date': 'today 61-m'}`` would get data from today to 61months ago
      - 1-3 months will return daily intervals of data
      - 4-36 months will return weekly intervals of data
      - 36+ months will return monthly intervals of data
      - **NOTE** Google uses UTC date as *'today'*

    - Daily: ```{'date': 'today #-d'}``` where # is the number of days from that date to pull data for

      - For example: ``{'date': 'today 7-d'}`` would get data from the last week
      - 1 day will return 8min intervals of data
      - 2-8 days will return Hourly intervals of data
      - 8-90 days will return Daily level data

    - Hourly: ```{'date': 'now #-H'}``` where # is the number of hours from that date to pull data for

      - For example: ``{'date': 'now 1-H'}`` would get data from the last hour
      - 1-3 hours will return 1min intervals of data
      - 4-26 hours will return 8min intervals of data
      - 27-34 hours will return 16min intervals of data

* `property`

  - What search data we want
  - Example ```{'property': 'images'}```
  - Defaults to web searches
  - Can be ```images```, ```news```, ```youtube``` or ```froogle``` (for Google Shopping results)

[back to top](#api-payload-keys)

<hr>

Interest Over Time

    pytrends.interest_over_time(payload, return_type=None)

Parameters

* `payload`

  - *Required*
  - a dictionary of key, values

* `return_type`

  - 'dataframe' returns a Pandas Dataframe
  - 'json' returns json
  
Returns JSON or Dataframe

[back to top](#trend)

<hr>

<hr>

Interest by Region

    pytrends.interest_by_region(payload, return_type=None)

Parameters

* `payload`

  - *Required*
  - a dictionary of key, values

* `resolution`

  - 'CITY' returns city level data
  - 'REGION' returns country level data

Returns JSON

[back to top](#trend)

<hr>

Related Queries

    pytrends.related_queries(payload)

Parameters

* `payload`

  - *Required*
  - a dictionary of key, values

* `related_type`

  - *Required*
  - 'top' returns top related data
  - 'rising' returns rising related data

Returns pandas.DataFrame

[back to top](#related_queries)

<hr>

hottrends

    pytrends.hottrends(payload)

Parameters

* `payload`

  - *Required*
  - a dictionary of key, values

Returns pandas.DataFrame

[back to top](#hottrends)

top_charts

    pytrends.topcharts(payload)

Parameters

* `payload`

  - *Required*
  - a dictionary of key, values

Returns JSON

[back to top](#topcharts)

<hr>

suggestions

    pytrends.suggestions(keyword)

Parameters

* `keyword`

  - *Required*
  - keyword to get suggestions for
  
Returns JSON

[back to top](#suggestions)

Caveats
-------

* This is not an official or supported API
* Google may change aggregation level for items with very large or very small search volume
* Google will send you an email saying that you had a new login after running this.
* Rate Limit is not pubically known, trail and error suggest it is around 10/min

Credits
-------

* Major JSON revision ideas taken from pat310's JavaScript library

  - https://github.com/pat310/google-trends-api

* Connecting to google code heavily based off Stack Overflow post

  - http://stackoverflow.com/questions/6754709/logging-in-to-google-using-python

* With some ideas pulled from Matt Reid's Google Trends API

  - https://bitbucket.org/mattreid9956/google-trend-api/overview
