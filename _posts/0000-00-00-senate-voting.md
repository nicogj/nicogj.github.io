---
layout:       analysis
category:     analysis
theme:        "Politics"
title:        "US Senate Voting"
author:       "Nicolas Guetta-Jeanrenaud"
repo: 		    "https://github.com/nicogj/parliament-polarization"
date:         2020-11-02 12:00:00
image:   ""
permalink:    analyses/us-senate
---

<div>
  <div style="position:relative;padding-top:100%;">
    <iframe src="https://chart-studio.plotly.com/~nicogj/15.embed" frameborder="0" allowfullscreen
      style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
</div>

This project visualizes voting topology in the current US Senate. Individuals who appear closest together on the graph consistently side together on Roll Call votes. The further individuals are to one another on the graph, the more they differ in their voting profiles. The position of each senator is therefore informative only relative to where the other individuals are placed. The visualization can be used to identify ideological clusters and to evaluate party polarization.

<!-- ## Methodology

By running a Principal Component Analysis on every senator's roll call votes, I create a voting profile and visualize ideological clusters and party polarization.

While a number of factors influence a given Senate session's voting profile, overall trends seem to indicate that Senator's increasingly vote according to party lines.



In this project, I take a look at voting in the US Senate over the past 30 years. By running a Principal Component Analysis on every senator's roll call votes, I create a voting profile and visualize ideological clusters and party polarization. While a number of factors influence a given Senate session's voting profile, overall trends seem to indicate that Senator's increasingly vote according to party lines.

As part of a <a href="http://github.com/nicogj/senate_vote_scraping" target="_blank">previous personal project</a>, I scraped all Senate Roll Call votes, publicly available on the <a href="https://www.senate.gov/legislative/votes.htm" target="_blank">US Senate Website</a>. I leverage this data here, comparing Senate voting behavior during Trump's first year in office (2017) with the first year in office the three previous presidents.

Vote positions are encoded as follows:
- `Yea` is converted to `1`
- `Not Voting` or missing data is converted to `0`
- `Nay` is converted to `-1`

I also drop all Senators with more than 80% of missing data---these are usually short-term Senators who served only interim roles during a vacancy.

In the resulting analytical frame, a given senator is assigned a vector with as many values as there were votes in that Senate session, and each value one of the values `{-1, 1, 0}`. Essentially, if there are 300 roll call votes, each senator is considered in a 300-dimension space, and given a coordinate of `-1`, `1`, or `0` in every one of these dimensions.

Principal Component Analysis is commonly used to reduce the number of dimensions in multi-variate problems. Here, I use it to project the coordinates of each senator in the multi-hundred dimensions onto a two-dimensional plot. I use the `PCA` function from the Python `sklearn` package, and visualize the results on Tableau.

All of the code I developed for this project can be found on the <a href="http://github.com/nicogj/2017_senate_voting" target="_blank">project's GitHub repo</a>. -->
