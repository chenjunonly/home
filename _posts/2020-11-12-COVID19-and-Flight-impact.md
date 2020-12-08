**Mapping the Evolution of COVID-19 and Air Flights in the US**

**Jiatang Dong, Yunjie Liu, Yarui Liu, Shuai Liu, Jun Chen**

**INTRODUCTION**

The recent COVID-19 pandemic has caused tremendous damage to global economy and health. Air travel has been shown to be significant in the spread of infectious diseases in early days of transmission. On the other hand, later in the pandemic stages, the wide-spreading infectious diseases impact travel. For our project, we focus on visualizing aviation and COVID-19 disease data together on the same map to reflect how flights are correlated to COVID-19 situation. The goal of this project is to build a double-layered map interface to:

1. Visualize COVID-19 and flight data on an interactive map and include a timeline to visualize their changes over time.

2. Determine a suitable color schema for drawing COVID-19 power-law distributed data over time.

This map interface could serve as data resource for researchers who want to study the relationship between the spread of COVID-19 and flights during the pandemic. Airline companies and researchers can use it to track and learn how COVID-19 pandemic correlates to travel. It can also serve as a guidance for people who plan to travel to measure the infectious risks at destination level.

**LITERATURE SURVEY**

We have studied some papers across different aspects regrading to this topic. Some papers focused on the spread of the infectious diseases including COVID-19[11], [12], SARS-COV-2[7], Ebola [9], H1N1[3], how the outbreak of the infectious diseases and travel ban impact aviation industry [6], global traffic [10] and global economy [5]. These papers collected and analyzed the worldwide distribution of total air passengers [8], the changes of the total number and routes of the flights [2], the relationship between population movements and the regional outbreak of the infectious diseases via different techniques, e.g., geographic information system [1] and interviewing senior industry executives [4]. These papers enriched us the background knowledge and historical information in the area of the spread of infectious diseases. However, some of these papers have not collected disease data on fine county-level [2], [17]. Some of the studies fail to reflect how the flights changed with the evolution of the pandemic over time [8], [18]. Some systems analyze travel patterns only in the early days of the epidemic and are not full retrospective [1]. Other studies visualize aviation and disease data separately without combining them together [3]. As for our project, we focus on interactively visualizing COVID-19 spreading in the US over time. Aviation and disease data are visualized together on the same map to reflect how flights are correlated to COVID-19 situation.

Other than that, we have also studied some papers focus on cartography as visualization is the main deliverables of our project. These papers reviewed the basic mapping principles of visualizing data, introduced two top level of visualization methods for quantitative data on map: choropleth vs proportional symbols [13], addressed the technique of resolving the issue of data in heavy-tailed distributions [16], provided ideas of how to implement color classification in choropleth map [14], [15]. These basic principles became the guideline for our project and the mapping techniques were applied and explored in our project.

**PROPOSED METHOD**

**Intuition**

As we stated above, what we visualize in this project is the aviation and disease data together on the same map to reflect how flights are correlated to COVID-19 situation.

The innovations of our approach include: First, our interface will be a double-layer map. Flights and COVID-19 confirmed cases in the US will be visualized together. This has not been done by other interfaces to date based on our knowledge. Second, it will be an interactive map which includes a timeline to visualize changes of flights and COVID-19 over time. Third, we will employ two different choropleth mapping classification methods to classify the COVID-19 data and to reflect the changes of it over time. A user survey will be conducted to determine which one is the better color schema for the power-law distributed COVID-19 data. This has not been studied by other published studies either.

**Data Preparation and Analysis**

Flight data is downloaded from the Zenodo repository &quot;Crowdsourced air traffic data from The OpenSky Network 2020&quot;. The data is separated into files by month. We use the files from Jan 2020 to Sep 2020. It is 3.13G total in size and contains 16,789,615 records. Each record has flight NO., first and last seen time, both origin and destination&#39;s airport code and latitude/longitude. We have another airport table from openflights.org which contains the airport code and its country code. We use a python script to join these two datasets on airport code then filter the flights. Only the flights with an origin or destination airport that have the country code &quot;US&quot; is kept in the output. There are 9,233,019 records extracted. Then another script is run against the extracted flights to calculated daily total flights and a moving average is applied on it. Flights with UPS and FEX in the flight NO. text are extracted separately for future visualization.

The COVID-19 dataset is downloaded from _USAfacts.org_. The size of COVID-19 data on disk is 2.5 M, with 837,352 total records. The data that includes COVID-19 case information is from Jan.23, 2020 to Sep.24, 2020 with the number total confirmed cases in each US county per day. A python script was running against this data file to compute the total number of the whole country and apply moving average on it.

County and states data are downloaded from _USAfacts.org_ too. They are in geojson format and can be directly consume by Leaflet.

![](RackMultipart20201208-4-1muudjt_html_b901146c5f435f36.png) ![](RackMultipart20201208-4-1muudjt_html_10eb3ee3ef5d849c.png)

**Figure 1.** The trend for COVID-19 confirmed cases and the trend for airlines and cargo flights

![](RackMultipart20201208-4-1muudjt_html_a82357cf9968b157.png)

**Figure 2.** The regression trend between COVID-19 new cases and the number of flights by airlines in the US.

**Figure 1** describes the trend of COVID19 case in the US and the trend of airlines and cargo flights after Jan 2020. We also applied linear regression to analyze the relationship between COVID-19 new cases and the number of flights by airlines. In **Figure 2** , we illustrated the regression trend, the correlation coefficient, and the level of statistical significance. The results have shown there are significant negative correlations between COVID-19 new cases and the number of flights by airlines. More data analyses are included in the Appendix attached after references.

**Implementation and Algorithms**

We decided to use _Leaflet_ for map related data visualization. _Leaflet_ is an open-source JavaScript library for interactive maps. It is good at handling GeoJSON and LatLng data and comes with interactive zoom features. It supports loading overlay data as well. These features are helpful for building a multi-layer interactive map, which is what we are trying to do. **Figure 3** is a set of sample visualizations of our final implementation. It is a full retrospective map visualization of two datasets from previous days. Counties are colored and classified based on local COVID-19 new case number, with darker colors corresponding to higher number and lighter colors correspond to lower number. A legend showing how colors map to the COVID-19 new case number is also added. Green lines stand for flight routes. Users can utilize the interactive timeline on top of the map to visualize COVID-19 and flight situation on a particular day they are interested.

There are over 9 million flight routes need to be visualized in animation. It is a big challenge to optimize the algorithm. We know for sure the data must be transformed into the format that Leaflet can consume directly and hash them with date so it can be extracted instantly. But how to achieve that is an open question. We have explored different methods. The first method is extracting all the flight data in the format of the Leaflet consumes and store it in a json file. But this file will become over 700M in size and not feasible for distributing the application over internet. We decided to use the second method which is storing the data in csv format and reconstruct it for Leaflet. The data file size is reduced to 73M. The tradeoff is our application will take about 30~60 seconds to initialize.

The total confirmed cases by county in Sep 24 which is the last day of our collected data is typical power law distribution. How to visualize the evolution of this kind of data over time is a new topic that no previous studies we can refer. Vague and variance on results may happen when people read the map especially when choropleth mapping classification is not well designed. To improve the accuracy of the results reading and user experience, we have studied several choropleth classification methods and selected two well-studied methods to apply into our project. The two methods that we adopted are:

**QN** : The quantile method placed equal numbers of enumeration units into each class. With five classes, 20 percent of the units were in each class. Quantile classification is also known as percentile classification. With five classes, the test maps were quintile maps.

**NB** : The natural-breaks method used was the implementation of the Jenks optimization procedure that was made available in ESRI&#39;s ArcView GIS software. In general, the optimization minimized within-class variance and maximized between-class variance in an iterative series of calculations.

In our implementation, we ran the two classification algorithms against the data of last day and got two 8 level threshold sets. Then we map each threshold sets to red colors with different darkness level. This threshold mapping is used across the whole timeline.

![](RackMultipart20201208-4-1muudjt_html_2fecc68f2c7c5740.png) ![](RackMultipart20201208-4-1muudjt_html_860dcc5a17161abe.png)

![](RackMultipart20201208-4-1muudjt_html_9d07f89e6ffb48fd.png) ![](RackMultipart20201208-4-1muudjt_html_c5035493bfac613e.png)

**Figure 3.** A set of sample visualizations of the final implementation. Top 2 images are from NB experiment group and bottom 2 images are from QN experiment group.

**Experiments/ Evaluation**

We evaluated our map interface by conducting a user survey on _SurveyMonkey_. The user survey is divided into two parts. Part 1 is a set of 6 objective questions. This part was designed to compare map-reading accuracy for the two choropleth classification methods - the quantile method and the natural-break method. Each subject was randomly placed in one of the experiment groups. Test questions are multiple-choice and were designed to challenge subjects to read the given map by asking about COVID-19 regional data, flight data, time-series data and the whole map. Our intention is that overall accuracy on questions in Part 1 would indicate the overall performance with the classification tested, so that the more suitable classification method can be determined. Part 2 is a set of 3 subjective questions to collect users&#39; feedback and to find out how satisfied our users are with features of the map interface. Statistical methods were used to analyze survey results from both Part 1 and 2.

We advertised our visualization application through instant messenger app on phone. subjects who received the short message were required to finish the survey on computer instead of via phone as smartphone doesn&#39;t have enough compute power to process the large amount of data. People who participated in the experiment are with the wide-ranging background, but no one has professional in geography. As we utilized 2 choropleth classification methods – QN and NB, was built 2 experiment groups to test each of them. Our app writes the current timestamp into the cookie and mod it with 2, and the result is used to decide the experiment group. However, there might be some subjects set their browser to a certain safety level so we can&#39;t not write cookies. In that case, it will fall back to QN experiment group. We spotted this problem and corrected in the middle of data collection process. The gap got narrowed, and the final number for QN and NB we collected are 29 and 32 respectively. Due to the limit of the environment of the experiment, the process of subjects to finish the questionnaires was not supervised.

Table 1. Observed Percent Accuracy by Classification Method for Question Types

| Classification Methods | Question Types |
| --- | --- |
| N | COVID19 | FLIGHT | COVID19 and FLIGHT |
 |
| Natural Breaks | 261 | 0.69 | 0.97 | 0.82 |
| Quantile | 288 | 0.76 | 0.91 | 0.80 |

Note: N is the number of responses for which percentages are calculated, e.g., n=261 is 29 subjects responding to 9 questions.

![](RackMultipart20201208-4-1muudjt_html_463abd79fb60a52a.png)

**Figure 4.** The App satisfaction rate by classification methods in 3 questions.

**Survey Results**

Survey questionnaire is attached in Appendix. We have categorized the 6 objective questions into three types. COVID-19 type, including Question 4, 5 and 6. Question 1 is FLIGHT related question. COVID-19 and FLIGHT relationship type contains Question 2 and 3. The observed accurate rate was calculated by dividing the number of corrected answers by the total number of answers in each question type. The observed accurate rates by classification methods for different question types were shown in **Table 1**.

Generalized linear model (GLM) was used to test the differences in accurate rate using the two different choropleth classification methods. We made our GLM calculations by using GLM function in R. All the objective survey questions were tested in the GLM model separately for the two methods. We have found there is no significant difference (P value \&gt; 0.5) between the Natural Breaks method and Quantile method across all the objective questions in terms of accurate rate. The nature of the questions also affected accuracy because some questions (e.g., Question 6) are more difficult to answer than the others. Since the accurate rate are high for both methods, either choropleth mapping classification method could be selected in this project.

**Figure 4** shows the subjects&#39; satisfaction rate. It is calculated by dividing the number of responses under each choice by the total responses. The rate is combined on Question 7, 8 and 9. Overall we have found more than 80% of the subjects agree or strongly agree with that the application clearly reflects the change in the numbers of COVID19 cases in the US over time, the change in the number of flights in the US over time, and the relationship between flights and COVID19 cases. That means most of the users who took the survey are satisfied with our map interface.

**CONCLUSION AND DISCUSSION**

We have successfully run through a data visualization research process in our project. Firstly, we have designed, implemented, and published a data visualization application. Secondly, we searched and processed data from different resources. Additionally, we have found a never-studied question which is how to present power low distributed data overtime. We have adopted two algorithms to conduct a comparison study over it. Finally, we have created a survey, collected users&#39; feedback and analyzed the results. The application visualizes the changes overtime of multiple layers of data, which is quite innovate. During the process, we have solved the performance issue on some level to present the change of millions of records in a smooth animation. Our application has received a high rating from the feedback.

Something interesting can be observed in our application. For example, we can clearly see that most of the places with higher degree of flight routes also have higher number of confirmed cases. The trend of disease in many areas is usually spreading from an airport hub to the surrounding areas. We need more quantitative data to support these conclusions, but our application can help the researchers to quickly gain the intuition and inspire them for deeper research.

With the limit of time and resources, some aspects of this topic were not fully covered in this project. There are many improvements we can do in the future study.

From the application perspective, at least there are 2 areas that could be improved. 1) The flight data is not presented very well. We directly drew about 9 million data points in the app and got into the situation of finding tradeoff between speed and file size. That was all because we have not aggregated the flight data. We have discussed about exploring aggregating the data on airport, city, and county level, respectively. However, we don&#39;t have enough time to dig into it. 2) The Covid-19 data could be in different forms. We are visualizing the total confirmed cases on the map. We can explore better describers for the disease situations incorporating population and time factors. Other indicators like the death count, daily new cases and confirmed cases per 1000 population are candidates. A compassion study could be run over this topic.

From the user survey perspective, we also have some room to improve. We distributed our survey over the social network. That means 1) we have no way to control on the experiment process, 2) we cannot collect the subjects&#39; background information and 3) we cannot compensate the subjects as incentive, so we must limit the length of our survey. We have got quite high accuracy rate on some of the questions, and quite lows score on some others. But none of these can lead to a solid conclusion of information efficiency of our project. A pilot study is needed to run for those questions and a benchmark is needed to setup in a serious study.

Overall, we have conducted a full-fledged data visualization research in our project. Despite of the imperfects, the project is completed and received positive feedbacks. It is quite successful but still have room to improve.

**WEB APPLICATION**

[http://covid-19-timeline.s3-website-us-east-1.amazonaws.com/#/](http://covid-19-timeline.s3-website-us-east-1.amazonaws.com/#/)

**DEMO VIDEO**

[https://youtu.be/IJVemYyAQ3o](https://youtu.be/IJVemYyAQ3o)

**ACTIVITIES**

Haosen Wang left our team after we submitted the progress report, and we have informed the TA about the change via email. All other team members have contributed similar amount of effort with focus areas as below:

Jiatang D, Yunjie L., Yarui L. and Shuai L. were responsible for report/slides/poster; Jun C., Shuai L., and Jiatang D. were responsible for data collecting/cleaning/analysis; Jiatang D. and Yunjie L. were responsible for implementation and visualization.

**References**

[1] M. N. K. Boulos and E. M. Geraghty, &quot;Geographical tracking and mapping of coronavirus disease COVID-19/severe acute respiratory syndrome coronavirus 2 (SARS-CoV-2) epidemic and associated events around the world: how 21st century GIS technologies are supporting the global fight against outbreaks and epidemics,&quot; _Journal of Health Geographics_, vol. 19, no. 1, 2020.

[2]P. Christidis and A. Christodoulou, &quot;The Predictive Capacity of Air Travel Patterns during the Global Spread of the COVID-19 Pandemic: Risk, Uncertainty and Randomness,&quot; _Journal of Environmental Research and Public Health_, vol. 17, no. 10, p. 3356, 2020.

[3] A. Malik and R. Abdalla, &quot;Mapping the impact of air travelers on the pandemic spread of (H1N1) influenza,&quot; _Modeling Earth Systems and Environment_, vol. 2, no. 2, 2016.

[4] P. Suau-Sanchez, A. Voltes-Dorta, and N. Cugueró-Escofet, &quot;An early assessment of the impact of COVID-19 on air transport: Just another crisis or the end of aviation as we know it?,&quot; _Journal of Transport Geography_, vol. 86, p. 102749, 2020.

[5] S. M. Iacus, F. Natale, C. Santamaria, S. Spyratos, and M. Vespe, &quot;Estimating and projecting air passenger traffic during the COVID-19 coronavirus outbreak and its socio-economic impact,&quot; _Safety Science_, vol. 129, p. 104791, 2020.

[6] L. Budd, M. Bell, and T. Brown, &quot;Of plagues, planes and politics: Controlling the global spread of infectious diseases by air,&quot; _Political Geography_, vol. 28, no. 7, pp. 426–435, 2009.

[7] S. Clifford, C. A. B. Pearson, P. Klepac, K. Van Zandvoort, B. J. Quilty, R. M. Eggo, and S. Flasche, &quot;Effectiveness of interventions targeting air travellers for delaying local outbreaks of SARS-CoV-2,&quot; _OUP Academic_, 08-May-2020. [Online]. Available: https://academic.oup.com/jtm/article/27/5/taaa068/5834629. [Accessed: 06-Oct-2020].

[8] K. Barbre and D. Esposito, &quot;Surveillance for Travel-Related Disease - GeoSentinel Surveillance System,&quot; _Morbidity and Mortality Weekly Report_, vol. 62, no. 3, 2013.

[9] R. Vaidya, A. Herten-Crabb, J. Spencer, S. Moon, and L. Lillywhite, &quot;Travel restrictions and infectious disease outbreaks,&quot; _Journal of Travel Medicine_, vol. 27, no. 3, 2020.

[10] A. R. Tuite, D. Bhatia, R. Moineddin, I. I. Bogoch, A. G. Watts, and K. Khan, &quot;Global trends in air travel: implications for connectivity and resilience to infectious disease threats,&quot; _Journal of Travel Medicine_, vol. 27, no. 4, 2020.

[11] Y. Daon, R. N. Thompson, and U. Obolski, &quot;Estimating COVID-19 outbreak risk through air travel,&quot; _Journal of Travel Medicine_, vol. 27, no. 5, 2020.

[12] D. Chu, S. Duda, K. Solo, S. Yaacoub, and H. Schunemann, &quot;Physical Distancing, Face Masks, and Eye Protection to Prevent Person-to-Person Transmission of SARS-CoV-2 and COVID-19: A Systematic Review and Meta-Analysis,&quot; _Journal of Vascular Surgery_, vol. 72, no. 4, p. 1500, 2020.

[13] C. A. Brewer, &quot;Basic Mapping Principles for Visualizing Cancer Data Using Geographic Information Systems (GIS),&quot; _American Journal of Preventive Medicine_, vol. 30, no. 2, 2006.

[14] M. Harrower and C. A. Brewer, &quot;ColorBrewer.org: An Online Tool for Selecting Colour Schemes for Maps,&quot; _The Cartographic Journal_, vol. 40, no. 1, pp. 27–37, 2003.

[15] C. A. Brewer and L. Pickle, &quot;Evaluation of Methods for Classifying Epidemiological Data on Choropleth Maps in Series,&quot; _Annals of the Association of American Geographers_, vol. 92, no. 4, pp. 662–681, 2002.

[16] B. Jiang, &quot;Head/Tail Breaks: A New Classification Scheme for Data with a Heavy-Tailed Distribution,&quot; _The Professional Geographer_, vol. 65, no. 3, pp. 482–494, 2013.

[17] S. H. Ali and R. Keil, &quot;Global Cities and the Spread of Infectious Disease: The Case of Severe Acute Respiratory Syndrome (SARS) in Toronto, Canada,&quot; _Urban Studies_, vol. 43, no. 3, pp. 491–509, 2006.

[18] M. Mhalla, &quot;The Impact of Novel Coronavirus (COVID-19) on the Global Oil and Aviation Markets,&quot; _Journal of Asian Scientific Research_, vol. 10, no. 2, pp. 96–104, 2020.

[19] Cromley, R. G. 1995. Classed versus unclassed choropleth maps: A question of how many classes. Cartographica 32 (4): 15 – 27. ———. 1996. A comparison of optimal classification strategies for choroplethic displays of spatially aggregated data. Inter- national Journal of Geographic Information Science 10 (4): 405 – 24.

**Appendix**

![](RackMultipart20201208-4-1muudjt_html_ea0bfc1b7f22583b.png)

**Figure 4.** The trend for the number of flights in the different continents (AF: Africa, AS: Asia, EU: Europe, NA: North America, OC: Oceania, SA: South America).

![](RackMultipart20201208-4-1muudjt_html_e8f9702c18755a77.png)

**Figure 5.** The trend for the number of flights in the different countries (JP: Japan, GB: United Kingdom of Great Britain and Northern Ireland, US: United States of America, DE: Germany, AU: Australia, AE: United Arab Emirates).

![](RackMultipart20201208-4-1muudjt_html_71fc3d8928ab3731.png)

**Figure 6.** The trend for the number of flights in the different airports (KJFK: John F Kennedy International Airport, EGLL: London Heathrow Airport, EDDF: Frankfurt am Main Airport, OMDB: Dubai International Airport, YSSY: Sydney Kingsford Smith International Airport).

![](RackMultipart20201208-4-1muudjt_html_9dfa0edb882db45e.png)

**Figure 7.** The trend for the number of flights in the passenger airlines (DAL: DELTA, AAL: AMERICAN, UAL: UNITED, SWA: SOUTHWEST).

![](RackMultipart20201208-4-1muudjt_html_80c2c2ffd25d8751.png)

**Figure 8.** The trend for the number of flights in the cargo airlines (UPS: UPS, FDX: FEDEX).

![](RackMultipart20201208-4-1muudjt_html_41dbe10de0fbfff2.png)

**Figure 9.** The regression trend between COVID-19 new cases and the number of flights in the US.

![](RackMultipart20201208-4-1muudjt_html_3787b2e98a55f7c2.png)

**Figure 10.** The regression trend between COVID-19 new cases and the number of flights by cargo airlines in the US.

![](RackMultipart20201208-4-1muudjt_html_c1e7e86367636cb0.png)

**Figure 11.** The comparison between 2019 April and 2020 April for the number of flights in the different continents (AF: Africa, AS: Asia, EU: Europe, NA: North America, OC: Oceania, SA: South America ![](RackMultipart20201208-4-1muudjt_html_1b1b1770b5c6af50.png)

**Figure 12.** The comparison between 2019 April and 2020 April in the different countries (JP: Japan, GB: United Kingdom of Great Britain and Northern Ireland, US: United States of America, DE: Germany, AU: Australia, AE: United Arab Emirates).

![](RackMultipart20201208-4-1muudjt_html_a6e369472974afb9.png)

**Figure 13.** The comparison between 2019 April and 2020 April in the different airports (KJFK: John F Kennedy International Airport, EGLL: London Heathrow Airport, EDDF: Frankfurt am Main Airport, OMDB: Dubai International Airport, YSSY: Sydney Kingsford Smith International Airport).

![](RackMultipart20201208-4-1muudjt_html_8ccd4be262809ca.png)

**Figure 14.** The comparison between 2019 April and 2020 April for the number of flights in the passenger airlines (DAL: DELTA, AAL: AMERICAN, UAL: UNITED, SWA: SOUTHWEST).

![](RackMultipart20201208-4-1muudjt_html_4b52e45a762155ee.png)

**Figure 15.** The comparison between 2019 April and 2020 April for the number of flights in the cargo airlines (UPS: UPS, FDX: FEDEX).

![](RackMultipart20201208-4-1muudjt_html_9684d7d6b6f82839.png)

**Figure 15.** Distribution of total confirmed cases by county in 9/24/2020

**Survey Q**** uestionnaire**

Flight question

1.Based on the fights displayed on the map, the total number of flights in the US was lower on which date?

A) 4.12\*

B) 7.17

Flights and COVID-19 relationship question

2. The total number of flights went down while the total number of confirmed COVID-19 cases went up during which time period?

A) 3/8-4/15\*

B) 8/5-9/24

Flights and COVID-19 relationship question

3. By 8/1, which county in **California** had more than 25 k confirm COVID-19 cases but it&#39;s not an airline hub?

A) Los Angeles County

B) Riverside County\*

C) San Diego County

COVID-19 question

4. During the time period from 7/14 to 7/21, which county in **New York State** had a **larger increase** inCOVID-19 cases?

A) Suffolk

B) Queens\*

COVID-19 question

5. In Miami-Dade County (southernmost county in Florida), the number of COVID-19 cases on 5/18 was:

A) 6322

B) 15864\*

C) 1006

COVID-19 question

6. Based **ONLY** on the **COLOR** of the regions on the map, the number of COVID-19 cases on 6/18 in New York City was \_\_\_ that in Chicago

A) higher than

B) lower than\*

Subjective question

7. The map interface CLEARLY reflects the relationship between flights and COVID-19 cases.

Strongly Disagree

Disagree

Neither Agree nor Disagree

Agree

Strongly Agree

Subjective question

8. The map interface **CLEARLY** reflects the change in the number of COVID-19 cases in the US over time.

Strongly Disagree

Disagree

Neither Agree nor Disagree

Agree

Strongly Agree

Subjective question

9. The map interface **CLEARLY** reflects the change in the number of flights in the US over time.

Strongly Disagree

Disagree

Neither Agree nor Disagree

Agree

Strongly Agree