---
layout:       post
category:     post
theme:        "Public Policy"
title:        "What Issues do our World Leaders Address?"
subtitle:     "Topic modeling on 2017 UN General Assembly speeches"
author:       "Nicolas Guetta-Jeanrenaud"
repo: 		    "https://github.com/nicogj/2017_un_speeches"
date:         2018-09-12 12:00:00
header-img:   ""
---

Every year, around September, world leaders meet in New York for the UN General Assembly Grand Debate. The event features a speech from every country's delegation, usually delivered by the head of state or a top diplomat, and gives each member the opportunity to highlight the most pressing world issues in their view. From an analytical standpoint, the General Assembly is a unique opportunity to observe the issues world leaders choose to address.

For this project, I scrape the entire corpus of speeches delivered during the 2017 UN General Assembly, as well as the official speech summaries as posted on the UN website. Unsupervised topic modeling produces several relevant issues that. Observing every country's dominant topic, as well as the distribution of important topics across the globe, gives relevant insight on national priorities, as well as strategic political placements.

<!-- Dominant Topics -->
<body>
  <div class='tableauPlaceholder' id='viz1551241312635' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_intro_2017&#47;WhatTopicsdoourWorldLeadersAddress&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='un_speech_intro_2017&#47;WhatTopicsdoourWorldLeadersAddress' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_intro_2017&#47;WhatTopicsdoourWorldLeadersAddress&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1551241312635');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

## Methodology

The first step of this project involved scraping the <a href="https://gadebate.un.org/en/sessions-archive" target="_blank">UN General Assembly Debate website</a> and collecting every head of state's intervention, as well as the official statement summary. For a given General Assembly session, these are both available on the country's endpoint: the full statement as a PDF file (usually available in several languages), and the summary as HTML text on the webpage. Sparing the details of the script, I use Python packages `requests` and `BeautifulSoup` from `bs4` to pull down the relevant information. Statement summaries are saved as text files. Full statements are downloaded as PDF's, but converted to text using using `pdftotxt` from the <a href="https://www.xpdfreader.com/pdftotext-man.html" target="_blank">Xpdf suite</a>.

I conduct initial cleaning on the text files by removing encoding characters, stripping spaces, and removing typical headers on the statement files ("Please check against delivery", for example). Inspired by <a href="https://github.com/willettk/common_language" target="_blank">this GitHub project by willettk</a>, I also "americanize" all the interventions by replacing British spellings with the American one.

The resulting dataset contains 199 world-leader interventions. I restrict it to 195 by removing the statements from supranational organizations (European Union, UN, and the President of the General Assembly's opening and closing remarks). All 195 have a statement summary, and the entire statement is available for 134 diplomats. Missing statements can be due to:
- Cases where the PDF version of the statement is not available on the UN website.
- Cases where the full statement is only available in non-English languages.
- Cases where no clean text was successfully extracted from the PDF by `pdftotxt`.

Given the substantial loss of data associated with the the use of full statements, and given several limitations of the raw full statement data (incorrect Optical Character Recognition in the pdf-to-text conversion, significant length disparities between statements), I end up using the statement summaries for the Topic Model analysis.

## TF-IDF Word Frequencies and Latent Dirichlet Allocation Topic Modeling

I then convert my corpus to a bag of words, stemming (using the `nltk` `SnowballStemmer`), and removing stop-words on the way. Instead of using simple words as unit of analysis, I use n-grams of one or two words to add additional context. I use the term frequency–inverse document frequency (TF-IDF) statistic to assign in every corpus element an importance score to every n-gram. I also restrict to words that appear in over 10%, and under 90%, of the statements. Most of the functions I define are adapted from the <a href="https://github.com/coleridge-initiative" target="_blank">Coleridge Initiative</a> "Applied Data Analytics" program, and I recommend referring to these notebooks for a step-by-step approach to Text Analysis.

Before running the topic model, I visualize the topology of the statements by running a Principal Component Analysis (PCA) on the TF-IDF scores. This visualization, shown below, projects the statements onto a two-dimensional plane and places them closest to statements with similar word frequencies. seems to show a strong, although not determinant geographic component to the word distribution of a statement.

<center>
<img class="example-image" src="/img/2017_un_speeches/pca_fig1.png" alt="image-1" />
</center>
<br>

Coloring now by the country's GDP per capita, wealthier nations seem to have lower coordinates along the second PCA axis, hinting that a nation's GDP per capita is another structuring element in the issues addressed by its diplomats.

<center>
<img class="example-image" src="/img/2017_un_speeches/pca_fig2.png" alt="image-1" />
</center>
<br>

I then run a Latent Dirichlet Allocation Topic Model, with using several different parameter values for the overall number of topics. Based on interpretability of the topics, I finally settle on 8 topics, defined by the following top 12 words:

```
Topic 0: uniti, said emphas, time, stabil, libya, root, negoti, base, must, comprehens, solut, freedom
Topic 1: develop, secur, also, implement, intern, sustain, unit, unit nation, peac, call, support, note
Topic 2: island, small island, small, caribbean, ocean, develop, island develop, hurrican, climat, develop state, climat chang, sustain
Topic 3: develop, global, unit nation, africa, african, unit, would, challeng, work, sustain, world, secur
Topic 4: unit, must, unit state, human, sovereignti, right, human right, nuclear, weapon, peopl, express, use
Topic 5: counter terror, freedom, counter, terror, terrorist, migrant, welcom, describ, asia, year, europ, civil
Topic 6: terrorist, territori, terror, solut, yemen, support, state, east, polit, intern, palestinian, region
Topic 7: republ, korea, republ korea, nuclear, democrat, democrat peopl, peopl republ, korean, east, missil, peninsula, peopl
```

Some of these topics---such as Topic 0 or Topic 5---are hard to make sense from. Others however seem to clearly relate to mainstream geopolitical issues. I keep the following sensical topics (referring to them as "relevant topics"):

Topic | Issue | Key Words
--- | --- | ---
Topic 1 | Peace & Security | secur, implement, sustain, peac, call, support
Topic 2 | Climate Change | island, small island, ocean, hurrican, climat chang
Topic 3 | Development | develop, global, africa, sustain, world
Topic 4 | Human Rights | human, sovereignti, human right, peopl, express
Topic 6 | Arab World | terrorist, terror, solut, yemen, east, palestinian
Topic 7 | North Korea | republ korea, nuclear, peopl republ, missil, peninsula


## Results

Each country's statement is made up of a distribution of all 8 topics above, each with specific probability scores that add up to 1. I assign a dominant topic to each country based on the highest probability score among relevant topics.

The most frequent dominant topic is Development, followed closely by Peace & Security and Human Rights. Regional topics such as the Arab World and North Korea have fewer flagship countries. 26 countries have Climate Change as their dominant topic.

<center>
<img class="example-image" src="/img/2017_un_speeches/dom_top_fig1.png" alt="image-1" />
</center>
<br>

The UN functions on a "One Nation, One Vote" standard, but differences in wealth among the countries undeniably contribute to the prioritizing of international issues. The distribution of dominant topics weighted by each countries GDP per capita is presented below. This time, Human Rights dominates the distribution, far ahead of Development, the Arab World, and Peace & Security. Climate Change, held high by countries with the lowest wealth, barely avoids becoming the least-popular topic. These results are robust when I use the distribution of probabilities instead of the dominant topic, and already highlight an important result: that the vote of the majority does not reflect the vote of the most powerful.

<center>
<img class="example-image" src="/img/2017_un_speeches/dom_top_fig2.png" alt="image-1" />
</center>
<br>

Plotting each countries dominant relevant topic yields interesting global structures. Unsurprisingly, much of sub-saharan Africa addresses Development, and Climate Change is the top issue in the island nations of Oceania and the Caribbeans. The topic of the Arab World is addressed by much of the Middle East, but also by countries in North Africa and Europe (Germany and Spain, for example). Finally, a localized set of countries in Asia---including China, Japan, both Koreas, Thailand, and Malaysia---are most preoccupied by the North Korean missile crisis.

<!-- Dominant Topics -->
<body>
  <div class='tableauPlaceholder' id='viz1551213311618' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_dom_topics_2017&#47;WorldMapofDominantTopics&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='un_speech_dom_topics_2017&#47;WorldMapofDominantTopics' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_dom_topics_2017&#47;WorldMapofDominantTopics&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1551213311618');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

Honing in on specific issues, I can also visualize the distribution of interest across the globe by plotting that topic's probability score for every country. In the case of North Korea, darker tones of red in Asia (Japan, China, Thailand, Malaysia) illustrate a regional preoccupation on the issue. But the topic is also on the mind of international superpowers, such as Canada, Brazil, and---most memorably---the United States. Some isolated, hardly-predictable countries also display high interest levels in the affair, such as Tanzania or Sweden. Although the geopolitical interpretation was lost on me, going back to the raw data shows both countries did extensively discuss the North Korean threat during their intervention.

<!-- North Korea -->
<body>
  <div class='tableauPlaceholder' id='viz1551240986603' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_n_korea_2017&#47;TopicFocusNorthKorea&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='un_speech_n_korea_2017&#47;TopicFocusNorthKorea' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_n_korea_2017&#47;TopicFocusNorthKorea&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1551240986603');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

Similarly, the Arab World is a theme with both a strong regional component (Middle East, Northern Africa, and Eastern Asia) and widespread international interest (Europe, the United States, and especially Russia).

<!-- Arab World -->
<body>
  <div class='tableauPlaceholder' id='viz1551240262570' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_arab_world_2017&#47;TopicFocusArabWorld&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='un_speech_arab_world_2017&#47;TopicFocusArabWorld' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_arab_world_2017&#47;TopicFocusArabWorld&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1551240262570');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

When if comes to climate change, however, the map appears completely bare. Most of the large, powerful countries---whether in Northern America, Europe, or Asia---display minimal interest in the crisis. Of the 26 countries that have Climate Change as their dominant topic, most are small, island nations that expect to face its consequences sooner rather than later.

<!-- Climate Change -->
<body>
  <div class='tableauPlaceholder' id='viz1551240189481' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_climate_change_2017&#47;TopicFocusClimateChange&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='un_speech_climate_change_2017&#47;TopicFocusClimateChange' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;un&#47;un_speech_climate_change_2017&#47;TopicFocusClimateChange&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1551240189481');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
</body>
<br>

In any case, these data offer fascinating insight into what world leaders focus on, or at least what they project as their focus. Feel free to get in touch if you notice other interesting results! All the data and code is available on Github, <a href="https://github.com/nicogj/2017_un_speeches" target="_blank">here</a>.
