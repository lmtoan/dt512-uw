# Assignment 1: Data Curation

* Project Goal

![](figs/ts_plot.png)

* List the license of the source data and a link to the Wikimedia Foundation REST API terms of use: https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions
* Link to all relevant API documentation
* Describe the values of all fields in your final data file.

| Column                  | Value     | Description                                                                                                               |
|-------------------------|-----------|---------------------------------------------------------------------------------------------------------------------------|
| year                    | YYYY      | Year of access                                                                                                            |
| month                   | MM        | Month of access                                                                                                           |
| pagecount_all_views     | num_views | Aggregate of pagecount of all channels, given month-year between Jan 2008 to July 2016                                    |
| pagecount_desktop_views | num_views | Aggregate of pagecount of desktop channel, given month-year between Jan 2008 to July 2016                                 |
| pagecount_mobile_views  | num_views | Aggregate of pagecount of mobile channel, given month-year between Jan 2008 to July 2016                                  |
| pageview_all_views      | num_views | Aggregate of pageview of all channels, given month-year between July 2015 to October 2018                                 |
| pageview_desktop_views  | num_views | Aggregate of pageview of desktop channel, given month-year between July 2015 to October 2018                              |
| pageview_mobile_views   | num_views | Aggregate of pageview of mobile channels (mobile app and mobile site), given month-year between July 2015 to October 2018 |

* List any known issues or special considerations with the data that would be useful for another researcher to know. For example, you should describe that data from the Pageview API excludes spiders/crawlers, while data from the Pagecounts API does not.