# Capstone: Web Traffic Time Series Forecasting

![chicago](notebook/images/map_webtraffic.png)

<!--
## Data is obtained from Kaggle competition: 
[Web Traffic Time Series](https://www.kaggle.com/competitions/web-traffic-time-series-forecasting/) -->

## Problem Identification Overview

How can the internet servers, such as Google, Facebook, and Yahoo, improve their capability inhandling traffic control by means of time series analysis, forecasting an accurate prediction of futureviews for the next 60 days?

### Context

Web traffic can be defined as the number of visits to a website, including requests sent and received by webusers. We aim to predict future web traffic for approximately a total of 145k Wikipedia articles to makebetter traffic control decisions. The increase in traffic for the websites could cause a lot of inconveniencefor the users by a crashed site or very slow loading time. Therefore, a traffic management technique or planshould be put in place to reduce the risk of such problems.

## Recommendation

Given the time series data that contains 145k Wikipedia pages, we have built and evaluated three differentmodels using Seasonal AutoRegressive Integrated Moving Average (SARIMA), Prophet which wasdeveloped and supported by Facebook, and Long Short-Term Memory (LSTM). We found that **the LSTMmodel had the smallest error from the validation process, setting the last 60 days to the test dataset.Therefore, we strongly recommend that internet servers make use of our LSTM model to predict thetraffic of web pages to avoid any mishaps such as server shutdown or slowdown.** Note that in thisanalysis, we reduced the time series data into averaged daily traffic for the total pages without consideringthe individual ones as it is not computationally infeasible if we take them into account.

## Data Analysis

- train.csv
	- 145k rows each of which represents a different Wikipedia page
	- 804 columns: article title + daily traffic on the particular Wikipedia page from 2015-07-01 to2017-09-10
	- The first column contains information about the page which includes the information of language, access, and agent.

In this time series dataset, there are two kinds of missing values. The first type is the case where the data
is actually missing, and the second type is the case where the page is not created at the specific time and
the page has the valid counts of visits only after the page is created. For the former case, we make use of
linear interpolation to the neighbor data, and for the latter case, we simply fill in the zero value. In Figure
1, the *52_Hz_I_Love_You_zh.wikipedia.org_all-access_spider* page is created in 2016 April, so we fill
zero value before it is created, which is shown as the red line.

<p align="center">
   <img src="output/figures/imputed_data.png" width=70%>
   <figcaption align="center"> <i> Figure 1. Imputed Data for '52_Hz_I_Love_You_zh.wikipedia.org_all-access_spider' Wikipedia page. The blue line represents the normal data, and the red line represents the imputed data.</i> 
   </figcaption>
</p>

### Number of Articles

The dataset consists of 145k Wiki pages with different languages. As seen in Figure 2, the number ofarticles written in English is 24k, which is the largest among other languages. It is followed by Japanese,Germany, and French wiki pages. Interestingly, the number of the Spanish wiki page is smallest althoughSpanish is arguably ranked as the fourth most spoken language in the world, behind only English, Hindi,and Mandarin Chinese. Wikimedia and Mediawiki are miscellaneous pages, which may contain supplementary materials. Since they have no language information in the title, they are classified as theircategorical title.

<p style="align: center; text-align: center;">
   <img src="output/figures/n_articles_language.png" width=70%>
   <figcaption align="center"> <i> Figure 2. Number of articles per language.</i> </figcaption>
</p>

In the left panel of Figure 3, we can infer the total number of agents for the wiki pages, which non-spideragents are dominant over spider agents. This is not surprising as spider agent, known as web [crawler](https://en.wikipedia.org/wiki/Web_crawler), hasa limited purpose for web indexing. In the right panel of Figure 3, we can see that the number of mobileweb-accessible wiki pages is slightly larger than that of desktop-accessible pages, although the differenceis not noticeable. These days, mobile tools are well-developed, and people live in a world where they cansearch for the information that they need in any place by simply turning on a phone or tablet. However, forthis Wikipedia dataset, the desktop seems to be still equally important as an accessing tool.

<p align="center" width=100%>
   <img width=49% src="output/figures/n_articles_agent.png">
   <img width=49% src="output/figures/n_articles_access.png">
   <figcaption align="center"> <i> Figure 3. The number of articles per agent (left panel) and per access (right panel).</i>
   </figcaption>
</p>

### Daily Traffic

It is interesting to note that even though the number of articles for mobile-web access is slightly larger thanthat for desktop, the trend of the daily traffic is the opposite. In Figure 4, the overall time series is similarbetween the two accesses with desktop access being slightly higher. Interestingly, there are several spikesthat are associated only with pages from desktop access: the largest spike in August 2016 and other spikesin September and October 2016.

