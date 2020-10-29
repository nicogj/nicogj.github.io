---
layout:       post
category:     post
theme:        "Politics"
title:        "US Senate Polarization in 2017"
subtitle:     "Principal Component Analysis of voting clusters over the past 25 years"
author:       "Nicolas Guetta-Jeanrenaud"
repo: 		    "https://github.com/nicogj/2017_senate_polarization"
date:         2018-02-13 12:00:00
header-img:   ""
---

As Donald Trump's first year in office comes to a close, talk of political polarization in Washington has increased. A number of research has found record levels of partisanship in Congress this past year (<a href="https://voteview.com/" target="_blank">VoteView</a> has tracked partisanship in Congress since 1992 using the DW-NOMINATE procedure and recently called the 114th Congress <a href="https://voteviewblog.com/2016/12/18/the-end-of-the-114th-congress/" target="_blank">"the most polarized [...] since the early 20th Century"</a>).

In this project, I take a look at voting in the US Senate over the past 30 years. By running a Principal Component Analysis on every senator's roll call votes, I create a voting profile and visualize ideological clusters and party polarization. While a number of factors influence a given Senate session's voting profile, overall trends seem to indicate that Senator's increasingly vote according to party lines.

<body>
<div class='tableauPlaceholder' id='viz1550517331983' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2017_detail&#47;2017SenatorVotePCA&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Senator_Votes_PCA_2017_detail&#47;2017SenatorVotePCA' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2017_detail&#47;2017SenatorVotePCA&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1550517331983');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

## Methodology

As part of a <a href="http://github.com/nicogj/senate_vote_scraping" target="_blank">previous personal project</a>, I scraped all Senate Roll Call votes, publicly available on the <a href="https://www.senate.gov/legislative/votes.htm" target="_blank">US Senate Website</a>. I leverage this data here, comparing Senate voting behavior during Trump's first year in office (2017) with the first year in office the three previous presidents.

<!-- An exhaustive list of US Senate Roll Call votes can be found on the [United States Senate webpage](http://www.senate.gov). Using the Python `requests` package, and the text parser `BeautifulSoup`, I pulled every vote cast by every Senator from Donald Trump's first year in office (2017). For comparison, I also pulled the data for first year in office of the three previous presidents. -->

Senate Session | Year | Serving President | Senate Composition
--- | --- | --- | ---
103 | 1993 | Bill Clinton (first year) | 57 Dem -- 43 Rep
107 | 2001 | George W. Bush (first year) | 50 Dem -- 50 Rep
113 | 2009 | Barack Obama (first year) | 56 Dem -- 2 Ind -- 42 Rep
117 | 2017 | Donald Trump (first year) | 47 Dem -- 2 Ind -- 51 Rep

Vote positions are encoded as follows:
- `Yea` is converted to `1`
- `Not Voting` or missing data is converted to `0`
- `Nay` is converted to `-1`

I also drop all Senators with more than 80% of missing data---these are usually short-term Senators who served only interim roles during a vacancy.

In the resulting analytical frame, a given senator is assigned a vector with as many values as there were votes in that Senate session, and each value one of the values `{-1, 1, 0}`. Essentially, if there are 300 roll call votes, each senator is considered in a 300-dimension space, and given a coordinate of `-1`, `1`, or `0` in every one of these dimensions.

Principal Component Analysis is commonly used to reduce the number of dimensions in multi-variate problems. Here, I use it to project the coordinates of each senator in the multi-hundred dimensions onto a two-dimensional plot. I use the `PCA` function from the Python `sklearn` package, and visualize the results on Tableau.

All of the code I developed for this project can be found on the <a href="http://github.com/nicogj/2017_senate_voting" target="_blank">project's GitHub repo</a>.


## Results

An initial look at the 2017 Senator voting topology seems to highlight a densely knitted Republican Party, very consistent in its votes and with few mavericks. The Democratic Party on the other hand stretches from moderate or conservative senators, such as Joe Manchin in Virginia, all the way to self-described democratic socialists such as Bernie Sanders. In between, a sparse cluster of "mainstream" Democrats are organized around Minority Leader Chuck Schumer and Minority Whip Dick Durbin. In addition to ideological differences, these Senator's face very different constituencies: some of the Democratic Senators that seem closest to the Republican voting cluster (Manchin, Heitkamp, Donnelly, or McCaskill) are from states that went for Trump in 2016.

This visualization might be understating the internal divisions in the Republican Party, however. Roll Call votes are not a perfect proxy for ideology, and one limitation of this project is that the party in power---the one that has the power to call votes in the Senate and that, in effect, calls votes only when a majority is guaranteed---will consistently look more clustered than the opposition party. We all remember John McCain's bombshell vote on the Obamacare repeal that handed the Republican party a shocking defeat: such events are rare, and avoided at all cost by congressional majority leaders.

However, comparing to previous first-term Senate sessions under different majorities, these visualizations highlight an undeniable trend towards more polarization in Congress over the last 20 years. While the basic distribution shape is fairly consistent across time, the clusters have grown more dense and separate, indicating that senators are increasingly voting by party lines.

Th PCA method I develop is surprisingly relevant at visualizing ideological clusters, and at highlighting outliers and exceptional voting records. A non-exhaustive list of remarkable observations I noticed are described below.

<!-- While the distribution of Senators bore resemblance to a spectrum 15 years ago, it looks more like two concentrated clusters today. Under President Trump, Majority Leader McConnell has run a tight ship, ceding very few votes from his own party. -->

<!-- While increased partisanship has long been a problem in Washington, the last 10 years have taken the issue to a whole new level. Parties radicalized under Obama's presidency, and the election of Donald Trump in 2016 exacerbated tensions between Democrats and Republicans.
Democrats have for the most part held together as well. Parties in power are systematically more clustered than those in the opposition, but willingness to compromise with leadership seems to be losing ground.
Finally, as President Trump wraps up his first year in the Oval Office, the race for 2020 is already on. Some hopeful Democrats have made a point opposing almost every Republican nomination and motion. They appear distinctly clustered from the mainstream Democratic party. -->


### 1993: Shelby goes Republican

The 1993 Senate voting topology clearly separated senators by their party. The Democratic majority form a fairly dense cluster, while the Republicans are spread across a wider specter depending on how often they join Democrats in their votes.

<body>
<div class='tableauPlaceholder' id='viz1550249387884' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_1993&#47;1993SenatorVotingPCA&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Senator_Votes_PCA_1993&#47;1993SenatorVotingPCA' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_1993&#47;1993SenatorVotingPCA&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1550249387884');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

One notable exception is Senator Shelby, from Alabama. This single blue island in a sea of red depicts Senator Shelby as a true maverick of the Democratic Party, seemingly voting more often along the Republican line than with his Democratic colleagues. In fact, Senator Shelby was about to turn Republican: in 1994, halfway through Bill Clinton's first term and as Republicans swept the midterms, he officially switched his party affiliation.

### 2001: When in "Hung Government"...

Following through on a chaotic Presidential Election, the 2001 Senate was the closest to perfectly split as possible. In fact, the majority party changed twice in just 2001, and three times before the 2002 midterms. Vice-President Gore gave Democrats the tie-breaking majority from January 3rd to January 20th, 2001, when George Bush was sworn in and Dick Cheney effectively gave Republicans a tie-breaking majority. Just a few months later, in May 2001, Republican senator Jeffords declared himself an Independent caucusing with Democrats, flipping the majority once more.

<body>
<div class='tableauPlaceholder' id='viz1550249341902' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2001&#47;2001SenatorVotingPCA&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Senator_Votes_PCA_2001&#47;2001SenatorVotingPCA' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2001&#47;2001SenatorVotingPCA&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1550249341902');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

A combination of unstable majorities and opposing parties in power for the larger of 2001 might explain this unconventional voting topology. No clear cluster emerges, and the senators seem placed on an ideological continuum. Furthest to the topological (and ideological) right are conservative senators from safe Republican states. More liberal senators, usually from swing or outright Democratic states, are placed on the other side of the Republican spectrum, closest to their Democratic counterparts. Senators Jeffords, the most liberal Republican on the spectrum, was to become a Democrat that year and Senator Chafee, who closely follows, left the Republican Party in 2007. John McCain also holds an interesting position in the Republican Party, seemingly voting with the liberal wing of the party on the votes that define PCA Axis 1, and with the conservative wing on the votes that define PCA Axis 2. Although a more thorough analysis is necessary to understand his true ideology, this does support his reputation of an unpredictable maverick.

Democratic centrist on the other hand usually come from traditionally Republican-leaning states, like the Deep South. The bulk of the Democratic party is concentrated around prominent senators such as Tom Daschle, Harry Reid, John Kerry, or Hillary Clinton. A major outlier is Senator Feingold, from Wisconsin. Considered one of the most liberal senators of the nation, Feingold often broke with his own party, opposing NAFTA under Bill Clinton, for example. In 2001, in the aftermath of 9/11, he was the only senator to oppose the Patriot Act.

### 2009: The parties regroup

Obama's election in 2008 gave Democrats a clear majority in the Senate, and the voting topology of senators that year resembles the classic distribution of 1993. The party in power having the power to call votes, only does so when a majority is quasi-guaranteed. In effect, that party appears as exceptionally clustered while the opposition senators are spread out according to their penchant for bipartisanship.

<!-- 2009 -->
<body>
<div class='tableauPlaceholder' id='viz1550249304581' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2009&#47;2009SenatorVotingPCA&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Senator_Votes_PCA_2009&#47;2009SenatorVotingPCA' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2009&#47;2009SenatorVotingPCA&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1550249304581');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

However, a closer comparison with 1993 illustrates how, under a similar party split, partisanship has increased. The Democratic cluster is exceptionally dense, and only few senators (Feingold, McCaskill, Bayh, Nelson) escape party lines. One of the few moderate Republicans, Senator Spector turned Democrat early in 2009. Other Republican Senators most likely vote across the aisle include Snowe, Voinovich, and the current swing votes Collins and Murkowski. The bulk of the Republican party---including heavyweights McCain, McConnell, and Sessions---is situated far to the right of the topographic spectrum.


### 2017: It's already 2020

2017's voting topology looks like an radicalized version of 2009. The party's are now completely disjoint, and even the Democrats most conservative Senator (Manchin) is a distance away from the most liberal Republicans (Collins and Murkowski). The Republican cluster is denser still than Democrats in 2009---even notorious mavericks like McCain, Flake, and Paul are surprisingly in line with party voting.

<body>
<div class='tableauPlaceholder' id='viz1550517364007' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2017&#47;2017SenatorVotingPCA&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Senator_Votes_PCA_2017&#47;2017SenatorVotingPCA' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Se&#47;Senator_Votes_PCA_2017&#47;2017SenatorVotingPCA&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1550517364007');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

One must keep in mind however that this method of analysis is based only on roll call votes, and not on policy positions that were taken. One possible explanation for the radical state of polarization might therefore be how close the current party split is. With a razor-thin 51-49 majority, Republicans have to call on Mike Pence's tie-breaking vote in case of a single defection. Two Republicans defections swing the chamber to Democratic majority. Under these circumstances, McConnell has been (almost) successful in avoiding votes where the majority was not guaranteed---on healthcare for example. A larger Republican majority might have given some senators more opportunities to defect. In any case, however, this distribution supports the idea that, despite internal internal divisions, McConnell has held most of the party in line this year.

On the Democratic side, finally, some senators are waisting no time voicing their disscontempt in Trump. A fascinating cluster furthest to the left of the topology is comprised of exceptionally progressive senators, such as Warren, Sanders, Gillibrand, Booker, and Harris. Another characteristic they share is that they have all expressed interest in running for president in 2020. Will a radical anti-Trump voting record in Senate be the key to capturing the Democratic nomination ? This remains to be seen, but in the minds of these senators, the race for 2020 has already begun.
