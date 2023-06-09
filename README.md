# Information Retention in the Multi-Platform Sharing of Science.

The public interest in accurate scientific communication, underscored by recent public health crises, highlights how content often loses critical pieces of information as it spreads online. However, multi-platform analyses of this phenomenon remain limited due to challenges in data collection. Collecting mentions of research tracked by Altmetric LLC, we examine information retention in the over 4 million online posts referencing 9,765 of the most-mentioned scientific articles across blog sites, Facebook, news sites, Twitter, and Wikipedia. To do so, we present a burst-based framework for examining online discussions about science over time and across different platforms. To measure information retention we develop a keyword-based computational measure comparing an online post to the scientific article's abstract. We evaluate our measure using ground truth data labeled by within field experts. We highlight three main findings: first, we find a strong tendency towards low levels of information retention, following a distinct trajectory of loss except when bursts of attention begin in social media. Second, platforms show significant differences in information retention. Third, sequences involving more platforms tend to be associated with higher information retention. These findings highlight a strong tendency towards information loss over time - posing a critical concern for researchers, policymakers, and citizens alike - but suggest that multi-platform discussions may improve information retention overall.

*Citation*: Hwang, S., Horvát, E-Á., Romero, D., 2023. Information Retention in the Multi-Platform Sharing of Science. *International AAAI Conference on Web and Social Media (ICWSM)*.

<!-- This is the public repo for sharing scripts related to information retention project published in ICWSM2023, with pre-print available [here](https://arxiv.org/abs/2207.13815).  -->

<!-- The basic pipeline --- and relevant scripts and files shared here --- of this project is as follows: -->

## Curating a set of relevant research articles to examine
* From the 2018 and 2019 dump data from Altmetric (the snapshot of the entire data available from the API at a given time), we identified a subset of articles of interest; in the case of our work, we used the dump datasets to identify papers among the 25,000 most mentioned articles in both years, adapting a script shared by Hao Peng: `hao_dump_filtering_code.py`. We run this script for each year's dump and find the overlap. 
* We filtered this set on some parameters (see published paper, pre-print availabe [here](https://arxiv.org/abs/2207.13815)). Of particular interest is that we only included papers for which we could obtain the abstract text from [WoS](https://clarivate.com/webofsciencegroup/solutions/web-of-science/) and [MAG](https://microsoft.com/en-us/research/project/microsoft-academic-graph/). You can see the commands used to retrieve the abstracts from each database at the time in `/abstracts`. The resulting file (after additional processing, see a below step on keyword extraction) is `meta2019_withabstract_df_withkeywords.tsv`. The result was roughly ~10,000 papers of interest.

## Data collection
* From here, we now have a list of scientific article DOIs we are interested in; using the Altmetric API, we obtain the most recent relevant set of mentions. This large dataset is split into a dataset for each platform type.
* For each platform, we collect the full text of mentions, the relevant scripts in `/data_collection`.
    * `collect_mentions_twitter_api.py` retrieves the text of tweets via the Twitter API before recent changes in Twitter leadership. Please be aware of new limitations in how the API operates.
    * `collect_mentions_wikimedia_api.py` retrieves the text of Wikipedia revisions.
    * We created a custom script to collect data from public content of Facebook, but it no longer is functional due to site changes and we do not share it here. 
    * Similarly, we use a series of manual and custom scripts for news and blogs site and we share the basic strategy for this work as well as supporting files in `/data_collection/news_blogs`.
    * `data_cleaning.py` does some basic cleaning of the text data collected, taking in the collected data per platform.

## Data processing
* `/keyword_extraction` contains the scripts we used for exploring, testing, and designing our keyword extraction approach, including for the creation of our validation survey. Note that we use the keyword_extraction script to extract keywords from each abstract, stored at this point in the file `meta2019_withabstract_df_withkeywords.tsv`.
* `/make_bursts` contains the scripts that we use to input the Altmetric data to identify bursts of attention to a given research article, per platform, and then combine that into a ''master'' sequence per research article.
* `/calculate_scores` contains the scripts for calculating the scores of each burst per sequence.

## Analysis
* `202204_remake_analyses.ipynb` contains the code for the analyses in the paper.



