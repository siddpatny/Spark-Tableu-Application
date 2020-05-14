# Big Data Application - Analyse Cabs Trends with Airports, Events and Businesses in New York City
 - Abhinav Gupta (ag7387) and Siddhant Patny (sp5331)</br>
 
### Tools and Framework -
1) Big data Tools and framework such as Scala with Spark are used to write the code for the application.
2) Data storage system is HDFS
3) Spatial joins on non big datasets is done using ArgGis
4) Visualisation is done using Tableau

### Dataset Sources -
1) NYC Yellow Taxi Data - NYC Open Data (Abhinav)
2) Uber Pickups - Kaggle (Abhinav)
3) Lyft Pickups - Kaggle (Abhinav)
4) Legally Operating Businesses - NYC Open Data (Siddhant)
5) Permitted Events - NYC Open Data (Siddhant)
6) Airports Flight Data - https://www.transtats.bts.gov/ (Abhinav and Siddhant)

### Dataset structure
<pre>
datasets/
        lyft/
        uber/
        yellow_taxi/
        business/
        events/
        airports/
</pre>

### Run Data cleaning Job -
Submit the job running the Main class - <I>Cleaning.scala</I> <br />
* The cleaned data is stored in csv style formatting -
    * First line is the headers of the file
    * The rest of the data is comma separated values <br />

Dataset structure for storing cleaned data 
<pre>
datasets/
        cleanedTaxiDataset/
                           lyft/
                           uber/
                           yellow_taxi/
        cleanedEBDataset/
                         events/
                         business/
                         events_address/
        cleanedFlightDataset/
                             nyc_airports/
</pre>

cleanedTaxiDataset was generated by Abhinav.
cleanedEBDataset was generated by Siddhant.
cleanedFlightDataset was generated by Abhinav and Siddhant.

### Run Profiling Job
Submit the job running the Main class - <I>Profiling.scala</I> <br />
Dataset structure for storing cleaned data 
<pre>
datasets/
        profiling/
</pre>

Abhinav - Profiling of taxi, uber, Airports and lyft dataset
Siddhant - Profiling of Events and Businesses dataset

### Mapping Dataset Structure
Mapping is done using zip code. We have done mapping using 3 different ways - 
1) ArcGIS tool to spatial map the taxi zones to zip codes.
2) BingMaps developer API to generate zip codes from addresses in events dataset. 
This is added as a part of the cleaning job. You are required to provide a Bing Maps API key to run event mapping. This is turned off by default.
3) Bing Maps developer API to generate zip codes from lat/long.
This is added as a part of the cleaning job. You are required to provide a Bing Maps API key to run event mapping. This is turned off by default. <br/>
<I> Use switch -geocode to run bing maps API</I> <br/>
<I> Use switch -bingKey to provide key for bing maps API</I> <br/>

Airports dataset contained Airport ID and unique identifiers such as "JFK". These were mapped to the 3 taxi zones of the area. 
This was used to join the dataset to cabs  
<br/>
<br/>
The Mapping files are generated as independent datasets to be later used by analysis for the join and calculations. 
<pre>
datasets/
        uber_data_mapping/
        yellow_taxi_join/
        events_address_mapping/
        events_address/
</pre>

Abhinav - uber and yellow taxi mapping.
Siddhant and Abhinav - events address mapping using Bing API.

### Running Analytics Job
Submit the job running the Main class - <I>RunAnalytics.scala</I> <br />
The Job runs to pick up cleaned and mapped files to join the datasets to create tables for analysis and plots.
Dataset structure for analytics data 
<pre>
datasets/
        Analytics/
                  comparision/
                              do_flight_count/
                              do_flight_passenger/
                              grouped_datasets/
                                               joinedAirportTaxi_DO/
                                               joinedAirportTaxi_PU/
                              pu_flight_count/
                              pu_flight_passenger/
                              zip_joined_2018/
                  event_address_join/
                  exploration/
                              business_year/
                              business_zip/
                              events_year/
                              events_zip/
                              lyft_year/
                              lyft_zip/
                              taxi_year/
                              taxi_zip/
                              uber_year/
                              uber_zip/
                  grouped_datasets/
                                   airport_dest_date/
                                   airport_origin_date/
                                   taxi_drop_date_airport/
                                   taxi_pickup_date_airport/
                  yellow_taxi/
                  uber_join/
                  lyft_join/
                  joined_datasets/
                                  join_event_taxi_normalised_ts/
                                  join_event_taxi_ts/
                                  outer_event_taxi_normalised_ts/
                                  outer_event_taxi_ts/  
</pre>

Abhinav and Siddhant

### Running Comparision Job
Submit the job running the Main class - <I>Comparision.scala</I> <br />
Run this job <b>after Analytics Job</b>. This job will use the files generated by the analytics job to further compare the 
datasets based on mathematical comparisons such as correlation between datasets etc.
<pre>
        Analytics/
                  comparision/
                              do_flight_count/
                              do_flight_passenger/
                              grouped_datasets/
                                               joinedAirportTaxi_DO/
                                               joinedAirportTaxi_PU/
                              pu_flight_count/
                              pu_flight_passenger/
                              zip_joined_2018/
</pre>

Abhinav and Siddhant

#### Analytics and inferences -  
<b>Link to the Paper -</b> https://drive.google.com/open?id=18uYjVGQr0e9sbEtLXJsPhEm_JbMKN3nM

### Actuation and Remediation
<p>This work like a realtime data update that the user may require. This can easily be extended over to an API to provide 
analytics to computer and mobile devices.</p>

One can query various analytics for their purpose and needs. These will help every cab consumers and the taxi 
Stakeholders and drivers' to plan and drive more intelligently for maximised profits. Below are a few example switches 
that can be used to query the details. These can be queried with the file <I>ActuationRemediation.scala</I>
<b>use -acre to get the results</b>
1) <I>-event</I> : To get details based on events. Required for event details
2) <I>-event_zip</I> : Get events at your zip
3) <I>-event_year</I> : Get events that happened in the query year
4) <I>-corr_taxi_ap</I> : Get the Correlation of Taxi count with number of flights for every month (in asc order) for the year 2018
5) <I>-corr_taxi_ap</I> : Get the Correlation of Passenger count with number of flights for every month (in asc order) for the year 2018
6) <I>-business</I> : To get details based on business. Required for business details
7) <I>-b_zip</I> : Get business at your zip
8) <I>-b_year</I> : Get business that happened in the query year
9) <I>-y_taxi</I> : To get details based on yellow taxis. Required for event details
10) <I>-y_taxi_zip</I> : Get yellow taxis at your zip
11) <I>-y_taxi_year</I> : Get yellow taxis that happened in the query year
12) <I>-uber</I> : To get details based on uber cabs. Required for event details
13) <I>-uber_zip</I> : Get uber cabs at your zip
14) <I>-lyft</I> : To get details based on lyft cabs. Required for event details
15) <I>-lyft_zip</I> : Get lyft cabs at your zip
<I>-event</I> :
<I>-event</I> :
<I>-event</I> :
<I>-event</I> :
<I>-event</I> :
<I>-event</I> :
<I>-event</I> :


### Visualisation
We use the tool tableau for Visualisation. Below are a few screenshots from Tableau - 
![NYC taxi with events timeseries - block zipcodes](files\screenshots\taxi_airport_pickup.PNG)
![NYC taxi with events timeseries - block zipcodes](files\screenshots\Correlation_drop_origin.PNG)
![NYC taxi heatmap with events timeseries](files\screenshots\taxi_events.PNG)
![NYC taxi heatmap with events timeseries](files\screenshots\taxi_business.PNG)
![NYC taxi heatmap with events timeseries](files\screenshots\Taxi_zip.PNG)

Abhinav and Siddhant

### Acknowledgements
<p>We would like to thank the Department of Computer Science
at NYU Courant Institute of Mathematical Science and the
NYU High-Performance Computing group for supporting this
project. We would also like to thank Prof. Suzzane Macintosh
who has supported and provided key insights to help make
this project a success.</p>
<p>We would also extend our gratitude to the NYC Open Data
team to provide free access to the data. We would also thank
transstats.bts.gov to provide historical data for various flights
at various airports.</p>

### References
1) NYC Open Data
2) https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
3) https://www.transtats.bts.gov/
4) https://data.cityofnewyork.us/City-Government/NYC-Permitted-Event-Information-Historical/bkfu-528j
5) https://www.kaggle.com/fivethirtyeight/uber-pickups-in-new-york-city
6) https://data.cityofnewyork.us/Business/Legally-Operating-Businesses/w7w3-xahh
7) https://www.kaggle.com/fivethirtyeight/uber-pickups-in-new-york-city
8) https://data.cityofnewyork.us/City-Government/NYC-Permitted-Event-Information/tvpp-9vvx
9) Spark: The Definitive Guide <I>By Bill Chambers and Matei</I>

