# Solar Panel Production Model

Load relevant libraries


```R
library(tidyverse)
library(lubridate)
library(httr)
```


```R
system("conda install -n R -c conda-forge r-corrr")
system("conda install -n R -c conda-forge r-ggpubr")
system("conda install -n R -c conda-forge r-tidymodels")
```


```R
library(corrr)
library(ggpubr)
library(tidymodels)
```

    ── [1mAttaching packages[22m ────────────────────────────────────── tidymodels 1.1.1 ──
    
    [32m✔[39m [34mbroom       [39m 1.0.5     [32m✔[39m [34mrsample     [39m 1.2.0
    [32m✔[39m [34mdials       [39m 1.2.0     [32m✔[39m [34mtune        [39m 1.1.2
    [32m✔[39m [34minfer       [39m 1.0.5     [32m✔[39m [34mworkflows   [39m 1.1.3
    [32m✔[39m [34mmodeldata   [39m 1.2.0     [32m✔[39m [34mworkflowsets[39m 1.0.1
    [32m✔[39m [34mparsnip     [39m 1.1.1     [32m✔[39m [34myardstick   [39m 1.2.0
    [32m✔[39m [34mrecipes     [39m 1.0.8     
    
    ── [1mConflicts[22m ───────────────────────────────────────── tidymodels_conflicts() ──
    [31m✖[39m [34mscales[39m::[32mdiscard()[39m masks [34mpurrr[39m::discard()
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m   masks [34mstats[39m::filter()
    [31m✖[39m [34mrecipes[39m::[32mfixed()[39m  masks [34mstringr[39m::fixed()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m      masks [34mstats[39m::lag()
    [31m✖[39m [34myardstick[39m::[32mspec()[39m masks [34mreadr[39m::spec()
    [31m✖[39m [34mrecipes[39m::[32mstep()[39m   masks [34mstats[39m::step()
    [34m•[39m Use [32mtidymodels_prefer()[39m to resolve common conflicts.
    



```R

```


```R

solar_import <- read_csv("2801736_system_energy_20220404_to_20230624.csv", show_col_types = FALSE)
solar_update <- read_csv("2801736_custom_report_230625_230914.csv", show_col_types = FALSE)
solar_import <- solar_import %>% 
  bind_rows(solar_update)

(solarByDay <- solar_import %>% 
  rename(Date = `Date/Time`) %>% 
  mutate(Date = as.Date(Date, '%m/%d/%Y'), `Energy Produced (kWh)` = `Energy Produced (Wh)`/1000) %>% 
  filter(Date > "2022-04-14")
)

```

    [1mRows: [22m[34m448[39m [1mColumns: [22m[34m2[39m
    [36m──[39m [1mColumn specification[22m [36m────────────────────────────────────────────────────────[39m
    [1mDelimiter:[22m ","
    [31mchr[39m (1): Date/Time
    [32mnum[39m (1): Energy Produced (Wh)
    
    [36mℹ[39m Use `spec()` to retrieve the full column specification for this data.
    [36mℹ[39m Specify the column types or set `show_col_types = FALSE` to quiet this message.
    [1mRows: [22m[34m82[39m [1mColumns: [22m[34m2[39m
    [36m──[39m [1mColumn specification[22m [36m────────────────────────────────────────────────────────[39m
    [1mDelimiter:[22m ","
    [31mchr[39m (1): Date/Time
    [32mdbl[39m (1): Energy Produced (Wh)
    
    [36mℹ[39m Use `spec()` to retrieve the full column specification for this data.
    [36mℹ[39m Specify the column types or set `show_col_types = FALSE` to quiet this message.



<table class="dataframe">
<caption>A tibble: 518 × 3</caption>
<thead>
	<tr><th scope=col>Date</th><th scope=col>Energy Produced (Wh)</th><th scope=col>Energy Produced (kWh)</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2022-04-15</td><td>  6865</td><td>  6.865</td></tr>
	<tr><td>2022-04-16</td><td> 48182</td><td> 48.182</td></tr>
	<tr><td>2022-04-17</td><td> 77459</td><td> 77.459</td></tr>
	<tr><td>2022-04-18</td><td> 35148</td><td> 35.148</td></tr>
	<tr><td>2022-04-19</td><td> 59849</td><td> 59.849</td></tr>
	<tr><td>2022-04-20</td><td> 95562</td><td> 95.562</td></tr>
	<tr><td>2022-04-21</td><td> 41926</td><td> 41.926</td></tr>
	<tr><td>2022-04-22</td><td> 87198</td><td> 87.198</td></tr>
	<tr><td>2022-04-23</td><td> 48172</td><td> 48.172</td></tr>
	<tr><td>2022-04-24</td><td> 86429</td><td> 86.429</td></tr>
	<tr><td>2022-04-25</td><td> 46024</td><td> 46.024</td></tr>
	<tr><td>2022-04-26</td><td> 27975</td><td> 27.975</td></tr>
	<tr><td>2022-04-27</td><td> 47856</td><td> 47.856</td></tr>
	<tr><td>2022-04-28</td><td> 98083</td><td> 98.083</td></tr>
	<tr><td>2022-04-29</td><td>101514</td><td>101.514</td></tr>
	<tr><td>2022-04-30</td><td> 95528</td><td> 95.528</td></tr>
	<tr><td>2022-05-01</td><td> 67771</td><td> 67.771</td></tr>
	<tr><td>2022-05-02</td><td> 43823</td><td> 43.823</td></tr>
	<tr><td>2022-05-03</td><td> 74164</td><td> 74.164</td></tr>
	<tr><td>2022-05-04</td><td> 28102</td><td> 28.102</td></tr>
	<tr><td>2022-05-05</td><td> 88225</td><td> 88.225</td></tr>
	<tr><td>2022-05-06</td><td> 11759</td><td> 11.759</td></tr>
	<tr><td>2022-05-07</td><td> 12844</td><td> 12.844</td></tr>
	<tr><td>2022-05-08</td><td> 84668</td><td> 84.668</td></tr>
	<tr><td>2022-05-09</td><td>101568</td><td>101.568</td></tr>
	<tr><td>2022-05-10</td><td>100930</td><td>100.930</td></tr>
	<tr><td>2022-05-11</td><td> 89071</td><td> 89.071</td></tr>
	<tr><td>2022-05-12</td><td> 77673</td><td> 77.673</td></tr>
	<tr><td>2022-05-13</td><td> 34268</td><td> 34.268</td></tr>
	<tr><td>2022-05-14</td><td> 29334</td><td> 29.334</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2023-08-16</td><td>65738</td><td>65.738</td></tr>
	<tr><td>2023-08-17</td><td>62420</td><td>62.420</td></tr>
	<tr><td>2023-08-18</td><td>77216</td><td>77.216</td></tr>
	<tr><td>2023-08-19</td><td>86909</td><td>86.909</td></tr>
	<tr><td>2023-08-20</td><td>79169</td><td>79.169</td></tr>
	<tr><td>2023-08-21</td><td>61371</td><td>61.371</td></tr>
	<tr><td>2023-08-22</td><td>82996</td><td>82.996</td></tr>
	<tr><td>2023-08-23</td><td>56484</td><td>56.484</td></tr>
	<tr><td>2023-08-24</td><td> 9917</td><td> 9.917</td></tr>
	<tr><td>2023-08-25</td><td>40754</td><td>40.754</td></tr>
	<tr><td>2023-08-26</td><td>60767</td><td>60.767</td></tr>
	<tr><td>2023-08-27</td><td>77346</td><td>77.346</td></tr>
	<tr><td>2023-08-28</td><td>34786</td><td>34.786</td></tr>
	<tr><td>2023-08-29</td><td>36573</td><td>36.573</td></tr>
	<tr><td>2023-08-30</td><td>70375</td><td>70.375</td></tr>
	<tr><td>2023-08-31</td><td>82003</td><td>82.003</td></tr>
	<tr><td>2023-09-01</td><td>81127</td><td>81.127</td></tr>
	<tr><td>2023-09-02</td><td>79051</td><td>79.051</td></tr>
	<tr><td>2023-09-03</td><td>73460</td><td>73.460</td></tr>
	<tr><td>2023-09-04</td><td>70303</td><td>70.303</td></tr>
	<tr><td>2023-09-05</td><td>65123</td><td>65.123</td></tr>
	<tr><td>2023-09-06</td><td>66952</td><td>66.952</td></tr>
	<tr><td>2023-09-07</td><td>37343</td><td>37.343</td></tr>
	<tr><td>2023-09-08</td><td>65651</td><td>65.651</td></tr>
	<tr><td>2023-09-09</td><td>53224</td><td>53.224</td></tr>
	<tr><td>2023-09-10</td><td>27303</td><td>27.303</td></tr>
	<tr><td>2023-09-11</td><td>47781</td><td>47.781</td></tr>
	<tr><td>2023-09-12</td><td>67464</td><td>67.464</td></tr>
	<tr><td>2023-09-13</td><td>54764</td><td>54.764</td></tr>
	<tr><td>2023-09-14</td><td>75767</td><td>75.767</td></tr>
</tbody>
</table>




```R

solarByDay %>% 
  filter(Date > "2022-04-15" & Date < "2023-04-16") %>% 
  summarize(total = sum(`Energy Produced (kWh)`))
```


<table class="dataframe">
<caption>A tibble: 1 × 1</caption>
<thead>
	<tr><th scope=col>total</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>18060.31</td></tr>
</tbody>
</table>




```R
ggplot(solarByDay, aes(Date, `Energy Produced (kWh)`)) +
  geom_point() + 
  theme(axis.text.x = element_text(angle = 90)) +
  scale_x_date(date_breaks = "1 month", date_labels = "%b %Y") +
  ggtitle("Residential Solar System (15.5 kW)") + 
  geom_smooth(span=.4, method = 'loess', formula = y ~ x) 
```

    [1m[22m`geom_smooth()` using method = 'loess' and formula = 'y ~ x'



    
![png](output_8_1.png)
    


Save system environment variables
(Store more securely later)


```R
#Sys.setenv(visualcrossing_email = "ben.jedlovec@gmail.com")
#Sys.setenv(visualcrossing_key = "NLHH8TUY2MQ9LN39C47RBGHQM") 
#Sys.setenv(zip_code = "18078") 
```


```R
#Sys.getenv("visualcrossing_email")
#Sys.getenv("visualcrossing_key")
#Sys.getenv("zip_code")
```


'NLHH8TUY2MQ9LN39C47RBGHQM'



'18078'



```R
weather_import <- read_csv("18078 2022-04-15 to 2023-07-01.csv", show_col_types = FALSE)

#Get Visual Crossing API credentials from R environment
api_email <- Sys.getenv("visualcrossing_email")
api_key <- Sys.getenv("visualcrossing_key")

#Construct API call
api_base_call <- "https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/"
api_zip <- Sys.getenv("zip_code")
start_date <- "2023-07-02"
end_date <- Sys.Date()
api_params <- "?unitGroup=us&elements=datetime%2CdatetimeEpoch%2Ctempmax%2Ctempmin%2Ctemp%2Chumidity%2Cprecip%2Cprecipprob%2Cprecipcover%2Cpreciptype%2Csnow%2Csnowdepth%2Ccloudcover%2Cvisibility%2Csolarradiation%2Csolarenergy%2Cuvindex%2Csunrise%2Csunset&include=days&key="
api_output <- "&contentType=csv"

api_call_history <- paste(api_base_call,api_zip,'/',start_date,'/',end_date,api_params,api_key,api_output, sep='')

api_response_history = content(GET(api_call_history))

weather_import_latest <- read_csv(api_response_history, show_col_types = FALSE)

(weather_import <- rbind(weather_import, weather_import_latest))
```

    No encoding supplied: defaulting to UTF-8.
    



<table class="dataframe">
<caption>A spec_tbl_df: 520 × 18</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>tempmax</th><th scope=col>tempmin</th><th scope=col>temp</th><th scope=col>humidity</th><th scope=col>precip</th><th scope=col>precipprob</th><th scope=col>precipcover</th><th scope=col>preciptype</th><th scope=col>snow</th><th scope=col>snowdepth</th><th scope=col>cloudcover</th><th scope=col>visibility</th><th scope=col>solarradiation</th><th scope=col>solarenergy</th><th scope=col>uvindex</th><th scope=col>sunrise</th><th scope=col>sunset</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dttm&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2022-04-15</td><td>67.2</td><td>36.8</td><td>55.6</td><td>45.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td>  0.4</td><td>9.9</td><td>288.0</td><td>25.0</td><td> 9</td><td>2022-04-15 06:23:41</td><td>2022-04-15 19:41:59</td></tr>
	<tr><td>2022-04-16</td><td>69.3</td><td>45.5</td><td>58.2</td><td>49.1</td><td>0.031</td><td>100</td><td> 12.50</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 50.8</td><td>9.9</td><td>156.8</td><td>13.5</td><td> 6</td><td>2022-04-16 06:22:09</td><td>2022-04-16 19:43:02</td></tr>
	<tr><td>2022-04-17</td><td>47.3</td><td>34.1</td><td>42.5</td><td>55.2</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 40.0</td><td>9.9</td><td>215.1</td><td>18.7</td><td> 7</td><td>2022-04-17 06:20:39</td><td>2022-04-17 19:44:05</td></tr>
	<tr><td>2022-04-18</td><td>48.5</td><td>27.8</td><td>38.3</td><td>66.2</td><td>0.580</td><td>100</td><td> 33.33</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 39.5</td><td>8.8</td><td> 98.3</td><td> 8.5</td><td> 4</td><td>2022-04-18 06:19:09</td><td>2022-04-18 19:45:07</td></tr>
	<tr><td>2022-04-19</td><td>48.7</td><td>37.3</td><td>41.6</td><td>68.7</td><td>0.582</td><td>100</td><td> 33.33</td><td>rain,snow,ice</td><td>0.3</td><td>0.2</td><td> 77.3</td><td>8.5</td><td>181.6</td><td>15.6</td><td> 7</td><td>2022-04-19 06:17:41</td><td>2022-04-19 19:46:10</td></tr>
	<tr><td>2022-04-20</td><td>57.0</td><td>38.0</td><td>46.6</td><td>45.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 23.7</td><td>9.9</td><td>299.3</td><td>25.9</td><td> 9</td><td>2022-04-20 06:16:13</td><td>2022-04-20 19:47:13</td></tr>
	<tr><td>2022-04-21</td><td>61.0</td><td>37.8</td><td>50.8</td><td>58.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 37.1</td><td>9.9</td><td>129.0</td><td>11.0</td><td> 6</td><td>2022-04-21 06:14:45</td><td>2022-04-21 19:48:16</td></tr>
	<tr><td>2022-04-22</td><td>67.3</td><td>41.2</td><td>55.0</td><td>53.5</td><td>0.007</td><td>100</td><td>  4.17</td><td>rain         </td><td>0.0</td><td>0.0</td><td>  4.4</td><td>9.8</td><td>287.8</td><td>24.7</td><td> 9</td><td>2022-04-22 06:13:19</td><td>2022-04-22 19:49:19</td></tr>
	<tr><td>2022-04-23</td><td>61.4</td><td>48.5</td><td>54.8</td><td>44.7</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 69.6</td><td>9.9</td><td>148.9</td><td>12.9</td><td> 7</td><td>2022-04-23 06:11:54</td><td>2022-04-23 19:50:21</td></tr>
	<tr><td>2022-04-24</td><td>70.7</td><td>47.0</td><td>57.7</td><td>53.6</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td>  1.6</td><td>9.9</td><td>281.3</td><td>24.4</td><td> 9</td><td>2022-04-24 06:10:29</td><td>2022-04-24 19:51:24</td></tr>
	<tr><td>2022-04-25</td><td>59.4</td><td>43.4</td><td>51.8</td><td>63.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 76.4</td><td>9.9</td><td>144.0</td><td>12.3</td><td> 5</td><td>2022-04-25 06:09:06</td><td>2022-04-25 19:52:27</td></tr>
	<tr><td>2022-04-26</td><td>61.5</td><td>50.9</td><td>55.6</td><td>77.4</td><td>0.113</td><td>100</td><td> 37.50</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 97.7</td><td>9.5</td><td> 94.8</td><td> 8.3</td><td> 4</td><td>2022-04-26 06:07:43</td><td>2022-04-26 19:53:29</td></tr>
	<tr><td>2022-04-27</td><td>52.8</td><td>39.6</td><td>46.6</td><td>54.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 92.3</td><td>9.9</td><td>151.7</td><td>13.0</td><td> 6</td><td>2022-04-27 06:06:22</td><td>2022-04-27 19:54:32</td></tr>
	<tr><td>2022-04-28</td><td>53.9</td><td>36.6</td><td>44.3</td><td>36.4</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 40.3</td><td>9.9</td><td>311.1</td><td>26.8</td><td>10</td><td>2022-04-28 06:05:02</td><td>2022-04-28 19:55:35</td></tr>
	<tr><td>2022-04-29</td><td>61.3</td><td>32.9</td><td>48.2</td><td>27.5</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td>  0.0</td><td>9.9</td><td>317.7</td><td>27.4</td><td> 9</td><td>2022-04-29 06:03:42</td><td>2022-04-29 19:56:37</td></tr>
	<tr><td>2022-04-30</td><td>67.3</td><td>34.2</td><td>51.8</td><td>30.8</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td>  0.0</td><td>9.9</td><td>313.0</td><td>27.0</td><td> 9</td><td>2022-04-30 06:02:24</td><td>2022-04-30 19:57:39</td></tr>
	<tr><td>2022-05-01</td><td>69.8</td><td>35.5</td><td>53.3</td><td>56.0</td><td>0.009</td><td>100</td><td>  8.33</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 30.4</td><td>9.9</td><td>226.8</td><td>19.7</td><td> 9</td><td>2022-05-01 06:01:07</td><td>2022-05-01 19:58:42</td></tr>
	<tr><td>2022-05-02</td><td>63.6</td><td>51.4</td><td>56.6</td><td>80.5</td><td>0.123</td><td>100</td><td> 16.67</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 89.9</td><td>9.2</td><td>147.8</td><td>12.7</td><td> 6</td><td>2022-05-02 05:59:52</td><td>2022-05-02 19:59:44</td></tr>
	<tr><td>2022-05-03</td><td>69.5</td><td>48.0</td><td>58.5</td><td>73.6</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 29.0</td><td>8.7</td><td>233.4</td><td>20.3</td><td> 8</td><td>2022-05-03 05:58:37</td><td>2022-05-03 20:00:46</td></tr>
	<tr><td>2022-05-04</td><td>62.4</td><td>49.3</td><td>54.5</td><td>86.7</td><td>0.094</td><td>100</td><td> 33.33</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 94.9</td><td>8.9</td><td> 88.0</td><td> 7.5</td><td> 2</td><td>2022-05-04 05:57:24</td><td>2022-05-04 20:01:47</td></tr>
	<tr><td>2022-05-05</td><td>71.8</td><td>51.9</td><td>62.6</td><td>55.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 25.0</td><td>9.9</td><td>290.5</td><td>25.1</td><td> 8</td><td>2022-05-05 05:56:12</td><td>2022-05-05 20:02:49</td></tr>
	<tr><td>2022-05-06</td><td>61.7</td><td>47.6</td><td>55.0</td><td>85.1</td><td>0.912</td><td>100</td><td> 66.67</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 89.0</td><td>7.4</td><td> 38.8</td><td> 3.4</td><td> 1</td><td>2022-05-06 05:55:01</td><td>2022-05-06 20:03:50</td></tr>
	<tr><td>2022-05-07</td><td>50.0</td><td>43.8</td><td>47.0</td><td>88.8</td><td>1.119</td><td>100</td><td>100.00</td><td>rain         </td><td>0.0</td><td>0.0</td><td>100.0</td><td>7.3</td><td> 39.5</td><td> 3.4</td><td> 1</td><td>2022-05-07 05:53:52</td><td>2022-05-07 20:04:51</td></tr>
	<tr><td>2022-05-08</td><td>60.9</td><td>41.6</td><td>50.8</td><td>52.1</td><td>0.056</td><td>100</td><td> 16.67</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 56.1</td><td>9.9</td><td>266.3</td><td>22.9</td><td> 9</td><td>2022-05-08 05:52:44</td><td>2022-05-08 20:05:52</td></tr>
	<tr><td>2022-05-09</td><td>71.2</td><td>43.0</td><td>57.8</td><td>30.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td>  8.4</td><td>9.9</td><td>333.0</td><td>28.9</td><td> 9</td><td>2022-05-09 05:51:38</td><td>2022-05-09 20:06:52</td></tr>
	<tr><td>2022-05-10</td><td>72.5</td><td>46.5</td><td>60.2</td><td>28.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td>  1.8</td><td>9.9</td><td>330.7</td><td>28.6</td><td> 9</td><td>2022-05-10 05:50:33</td><td>2022-05-10 20:07:53</td></tr>
	<tr><td>2022-05-11</td><td>74.6</td><td>48.1</td><td>62.3</td><td>44.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 16.2</td><td>9.9</td><td>301.5</td><td>26.2</td><td> 8</td><td>2022-05-11 05:49:29</td><td>2022-05-11 20:08:52</td></tr>
	<tr><td>2022-05-12</td><td>77.6</td><td>50.7</td><td>66.1</td><td>69.3</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>0.0</td><td> 31.7</td><td>9.9</td><td>260.0</td><td>22.5</td><td> 8</td><td>2022-05-12 05:48:27</td><td>2022-05-12 20:09:52</td></tr>
	<tr><td>2022-05-13</td><td>74.5</td><td>60.8</td><td>66.0</td><td>87.1</td><td>0.047</td><td>100</td><td> 12.50</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 93.4</td><td>8.5</td><td>116.2</td><td>10.2</td><td> 6</td><td>2022-05-13 05:47:27</td><td>2022-05-13 20:10:51</td></tr>
	<tr><td>2022-05-14</td><td>70.8</td><td>62.8</td><td>66.6</td><td>90.1</td><td>0.180</td><td>100</td><td> 41.67</td><td>rain         </td><td>0.0</td><td>0.0</td><td> 95.3</td><td>8.4</td><td> 92.2</td><td> 8.0</td><td> 4</td><td>2022-05-14 05:46:28</td><td>2022-05-14 20:11:49</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2023-08-18</td><td>77.1</td><td>63.3</td><td>71.2</td><td>68.1</td><td>0.260</td><td>100</td><td>16.67</td><td>rain</td><td>0</td><td>0</td><td>51.3</td><td>9.7</td><td>208.6</td><td>18.1</td><td> 9</td><td>2023-08-18 06:15:35</td><td>2023-08-18 19:56:29</td></tr>
	<tr><td>2023-08-19</td><td>76.7</td><td>57.1</td><td>67.0</td><td>59.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 9.5</td><td>9.9</td><td>268.7</td><td>23.2</td><td> 8</td><td>2023-08-19 06:16:34</td><td>2023-08-19 19:55:03</td></tr>
	<tr><td>2023-08-20</td><td>82.4</td><td>57.7</td><td>70.4</td><td>68.0</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 0.0</td><td>9.9</td><td>252.9</td><td>22.0</td><td> 8</td><td>2023-08-20 06:17:33</td><td>2023-08-20 19:53:36</td></tr>
	<tr><td>2023-08-21</td><td>85.5</td><td>64.4</td><td>75.3</td><td>73.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td>41.0</td><td>9.8</td><td>198.0</td><td>17.0</td><td> 7</td><td>2023-08-21 06:18:32</td><td>2023-08-21 19:52:07</td></tr>
	<tr><td>2023-08-22</td><td>78.4</td><td>63.1</td><td>71.5</td><td>56.7</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>26.3</td><td>9.9</td><td>261.8</td><td>22.5</td><td> 8</td><td>2023-08-22 06:19:31</td><td>2023-08-22 19:50:38</td></tr>
	<tr><td>2023-08-23</td><td>76.2</td><td>54.7</td><td>66.8</td><td>66.2</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>35.8</td><td>9.9</td><td>191.3</td><td>16.5</td><td> 8</td><td>2023-08-23 06:20:30</td><td>2023-08-23 19:49:08</td></tr>
	<tr><td>2023-08-24</td><td>69.9</td><td>65.8</td><td>67.6</td><td>77.9</td><td>0.119</td><td>100</td><td>37.50</td><td>rain</td><td>0</td><td>0</td><td>99.4</td><td>9.5</td><td> 32.6</td><td> 2.6</td><td> 1</td><td>2023-08-24 06:21:28</td><td>2023-08-24 19:47:38</td></tr>
	<tr><td>2023-08-25</td><td>81.0</td><td>66.9</td><td>72.6</td><td>87.1</td><td>0.254</td><td>100</td><td>20.83</td><td>rain</td><td>0</td><td>0</td><td>84.2</td><td>8.9</td><td>109.5</td><td> 9.4</td><td> 6</td><td>2023-08-25 06:22:27</td><td>2023-08-25 19:46:06</td></tr>
	<tr><td>2023-08-26</td><td>82.0</td><td>69.1</td><td>75.2</td><td>76.5</td><td>0.059</td><td>100</td><td>16.67</td><td>rain</td><td>0</td><td>0</td><td>55.5</td><td>9.0</td><td>189.0</td><td>16.4</td><td> 7</td><td>2023-08-26 06:23:26</td><td>2023-08-26 19:44:34</td></tr>
	<tr><td>2023-08-27</td><td>81.9</td><td>61.4</td><td>71.6</td><td>73.5</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>19.1</td><td>9.7</td><td>240.2</td><td>20.7</td><td> 8</td><td>2023-08-27 06:24:24</td><td>2023-08-27 19:43:01</td></tr>
	<tr><td>2023-08-28</td><td>77.1</td><td>65.2</td><td>71.4</td><td>77.7</td><td>0.005</td><td>100</td><td> 8.33</td><td>rain</td><td>0</td><td>0</td><td>79.4</td><td>9.8</td><td>109.5</td><td> 9.4</td><td> 4</td><td>2023-08-28 06:25:23</td><td>2023-08-28 19:41:27</td></tr>
	<tr><td>2023-08-29</td><td>77.7</td><td>66.9</td><td>71.8</td><td>80.0</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td>95.3</td><td>9.9</td><td>116.2</td><td>10.0</td><td> 4</td><td>2023-08-29 06:26:22</td><td>2023-08-29 19:39:53</td></tr>
	<tr><td>2023-08-30</td><td>80.8</td><td>66.3</td><td>73.7</td><td>72.0</td><td>0.092</td><td>100</td><td>12.50</td><td>rain</td><td>0</td><td>0</td><td>52.4</td><td>9.8</td><td>227.0</td><td>19.6</td><td> 8</td><td>2023-08-30 06:27:20</td><td>2023-08-30 19:38:18</td></tr>
	<tr><td>2023-08-31</td><td>76.4</td><td>60.0</td><td>66.9</td><td>56.9</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 4.7</td><td>9.9</td><td>255.8</td><td>21.9</td><td> 8</td><td>2023-08-31 06:28:19</td><td>2023-08-31 19:36:42</td></tr>
	<tr><td>2023-09-01</td><td>75.1</td><td>50.6</td><td>63.6</td><td>64.5</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 0.0</td><td>9.9</td><td>256.4</td><td>22.3</td><td> 8</td><td>2023-09-01 06:29:17</td><td>2023-09-01 19:35:06</td></tr>
	<tr><td>2023-09-02</td><td>80.4</td><td>51.2</td><td>66.9</td><td>70.9</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 2.8</td><td>9.9</td><td>245.8</td><td>21.3</td><td> 8</td><td>2023-09-02 06:30:16</td><td>2023-09-02 19:33:29</td></tr>
	<tr><td>2023-09-03</td><td>88.0</td><td>61.7</td><td>74.7</td><td>70.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td> 3.9</td><td>9.9</td><td>232.8</td><td>20.2</td><td> 8</td><td>2023-09-03 06:31:14</td><td>2023-09-03 19:31:52</td></tr>
	<tr><td>2023-09-04</td><td>90.6</td><td>68.6</td><td>79.3</td><td>71.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 8.9</td><td>9.9</td><td>238.8</td><td>20.8</td><td> 8</td><td>2023-09-04 06:32:12</td><td>2023-09-04 19:30:14</td></tr>
	<tr><td>2023-09-05</td><td>89.9</td><td>71.2</td><td>80.1</td><td>72.4</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>23.2</td><td>9.9</td><td>220.5</td><td>19.1</td><td> 7</td><td>2023-09-05 06:33:11</td><td>2023-09-05 19:28:36</td></tr>
	<tr><td>2023-09-06</td><td>89.7</td><td>69.5</td><td>80.1</td><td>72.1</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>15.1</td><td>9.6</td><td>206.9</td><td>18.0</td><td> 7</td><td>2023-09-06 06:34:09</td><td>2023-09-06 19:26:57</td></tr>
	<tr><td>2023-09-07</td><td>90.6</td><td>67.7</td><td>75.3</td><td>81.3</td><td>0.341</td><td>100</td><td>25.00</td><td>rain</td><td>0</td><td>0</td><td>34.7</td><td>9.5</td><td>130.6</td><td>11.1</td><td> 8</td><td>2023-09-07 06:35:07</td><td>2023-09-07 19:25:18</td></tr>
	<tr><td>2023-09-08</td><td>87.0</td><td>66.2</td><td>73.9</td><td>82.1</td><td>0.390</td><td>100</td><td>29.17</td><td>rain</td><td>0</td><td>0</td><td>41.1</td><td>7.8</td><td>224.4</td><td>19.4</td><td> 7</td><td>2023-09-08 06:36:06</td><td>2023-09-08 19:23:39</td></tr>
	<tr><td>2023-09-09</td><td>84.7</td><td>65.6</td><td>72.8</td><td>84.3</td><td>0.502</td><td>100</td><td>25.00</td><td>rain</td><td>0</td><td>0</td><td>34.1</td><td>9.2</td><td>184.0</td><td>15.9</td><td> 7</td><td>2023-09-09 06:37:04</td><td>2023-09-09 19:21:59</td></tr>
	<tr><td>2023-09-10</td><td>77.3</td><td>65.3</td><td>69.6</td><td>90.4</td><td>0.697</td><td>100</td><td>45.83</td><td>rain</td><td>0</td><td>0</td><td>72.5</td><td>8.5</td><td> 93.4</td><td> 8.0</td><td> 5</td><td>2023-09-10 06:38:02</td><td>2023-09-10 19:20:19</td></tr>
	<tr><td>2023-09-11</td><td>79.8</td><td>67.5</td><td>71.8</td><td>86.6</td><td>0.144</td><td>100</td><td>29.17</td><td>rain</td><td>0</td><td>0</td><td>64.5</td><td>9.2</td><td>133.3</td><td>11.5</td><td>10</td><td>2023-09-11 06:39:01</td><td>2023-09-11 19:18:39</td></tr>
	<tr><td>2023-09-12</td><td>81.6</td><td>60.8</td><td>70.5</td><td>76.6</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>10.2</td><td>9.7</td><td>230.0</td><td>19.9</td><td> 8</td><td>2023-09-12 06:39:59</td><td>2023-09-12 19:16:58</td></tr>
	<tr><td>2023-09-13</td><td>78.6</td><td>62.3</td><td>70.3</td><td>78.9</td><td>0.307</td><td>100</td><td>20.83</td><td>rain</td><td>0</td><td>0</td><td>60.3</td><td>9.1</td><td>156.2</td><td>13.6</td><td> 7</td><td>2023-09-13 06:40:57</td><td>2023-09-13 19:15:18</td></tr>
	<tr><td>2023-09-14</td><td>73.3</td><td>58.5</td><td>64.8</td><td>66.7</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>12.7</td><td>9.9</td><td>224.4</td><td>19.3</td><td> 8</td><td>2023-09-14 06:41:56</td><td>2023-09-14 19:13:37</td></tr>
	<tr><td>2023-09-15</td><td>74.0</td><td>51.3</td><td>61.7</td><td>60.9</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 0.0</td><td>9.9</td><td>230.8</td><td>19.9</td><td> 8</td><td>2023-09-15 06:42:54</td><td>2023-09-15 19:11:56</td></tr>
	<tr><td>2023-09-16</td><td>74.5</td><td>51.8</td><td>62.2</td><td>63.9</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 0.2</td><td>9.9</td><td>236.5</td><td>20.4</td><td> 8</td><td>2023-09-16 06:43:53</td><td>2023-09-16 19:10:14</td></tr>
</tbody>
</table>




```R

(solar_weather <- weather_import %>% 
   inner_join(solarByDay, by = join_by(datetime == Date)) %>% 
   mutate(daylight = as.numeric(sunset-sunrise,units="mins"), solarenergy_kWh = solarenergy/3.6, type = "Historical")
 )

#solar_weather_corr <- solar_weather %>% 
#  select(`Energy Produced (kWh)`,tempmax,humidity,precip,precipcover,snow,snowdepth,cloudcover,visibility,solarradiation,,solarenergy,uvindex,daylight) %>% 
#  correlate()

#write_csv(solar_weather_corr, "solar_weather_corr.csv")

ggplot(solar_weather, aes(solarenergy_kWh,`Energy Produced (kWh)`)) +
  geom_point() +
  geom_smooth(method="lm", formula = y ~ x + 0) +
  #stat_regline_equation(formula = y ~ x + 0) +
  xlab("Solar Energy Measured (kWh per square meter)") 
```


<table class="dataframe">
<caption>A tibble: 518 × 23</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>tempmax</th><th scope=col>tempmin</th><th scope=col>temp</th><th scope=col>humidity</th><th scope=col>precip</th><th scope=col>precipprob</th><th scope=col>precipcover</th><th scope=col>preciptype</th><th scope=col>snow</th><th scope=col>⋯</th><th scope=col>solarradiation</th><th scope=col>solarenergy</th><th scope=col>uvindex</th><th scope=col>sunrise</th><th scope=col>sunset</th><th scope=col>Energy Produced (Wh)</th><th scope=col>Energy Produced (kWh)</th><th scope=col>daylight</th><th scope=col>solarenergy_kWh</th><th scope=col>type</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2022-04-15</td><td>67.2</td><td>36.8</td><td>55.6</td><td>45.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>288.0</td><td>25.0</td><td> 9</td><td>2022-04-15 06:23:41</td><td>2022-04-15 19:41:59</td><td>  6865</td><td>  6.865</td><td>798.3000</td><td>6.9444444</td><td>Historical</td></tr>
	<tr><td>2022-04-16</td><td>69.3</td><td>45.5</td><td>58.2</td><td>49.1</td><td>0.031</td><td>100</td><td> 12.50</td><td>rain         </td><td>0.0</td><td>⋯</td><td>156.8</td><td>13.5</td><td> 6</td><td>2022-04-16 06:22:09</td><td>2022-04-16 19:43:02</td><td> 48182</td><td> 48.182</td><td>800.8833</td><td>3.7500000</td><td>Historical</td></tr>
	<tr><td>2022-04-17</td><td>47.3</td><td>34.1</td><td>42.5</td><td>55.2</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>215.1</td><td>18.7</td><td> 7</td><td>2022-04-17 06:20:39</td><td>2022-04-17 19:44:05</td><td> 77459</td><td> 77.459</td><td>803.4333</td><td>5.1944444</td><td>Historical</td></tr>
	<tr><td>2022-04-18</td><td>48.5</td><td>27.8</td><td>38.3</td><td>66.2</td><td>0.580</td><td>100</td><td> 33.33</td><td>rain         </td><td>0.0</td><td>⋯</td><td> 98.3</td><td> 8.5</td><td> 4</td><td>2022-04-18 06:19:09</td><td>2022-04-18 19:45:07</td><td> 35148</td><td> 35.148</td><td>805.9667</td><td>2.3611111</td><td>Historical</td></tr>
	<tr><td>2022-04-19</td><td>48.7</td><td>37.3</td><td>41.6</td><td>68.7</td><td>0.582</td><td>100</td><td> 33.33</td><td>rain,snow,ice</td><td>0.3</td><td>⋯</td><td>181.6</td><td>15.6</td><td> 7</td><td>2022-04-19 06:17:41</td><td>2022-04-19 19:46:10</td><td> 59849</td><td> 59.849</td><td>808.4833</td><td>4.3333333</td><td>Historical</td></tr>
	<tr><td>2022-04-20</td><td>57.0</td><td>38.0</td><td>46.6</td><td>45.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>299.3</td><td>25.9</td><td> 9</td><td>2022-04-20 06:16:13</td><td>2022-04-20 19:47:13</td><td> 95562</td><td> 95.562</td><td>811.0000</td><td>7.1944444</td><td>Historical</td></tr>
	<tr><td>2022-04-21</td><td>61.0</td><td>37.8</td><td>50.8</td><td>58.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>129.0</td><td>11.0</td><td> 6</td><td>2022-04-21 06:14:45</td><td>2022-04-21 19:48:16</td><td> 41926</td><td> 41.926</td><td>813.5167</td><td>3.0555556</td><td>Historical</td></tr>
	<tr><td>2022-04-22</td><td>67.3</td><td>41.2</td><td>55.0</td><td>53.5</td><td>0.007</td><td>100</td><td>  4.17</td><td>rain         </td><td>0.0</td><td>⋯</td><td>287.8</td><td>24.7</td><td> 9</td><td>2022-04-22 06:13:19</td><td>2022-04-22 19:49:19</td><td> 87198</td><td> 87.198</td><td>816.0000</td><td>6.8611111</td><td>Historical</td></tr>
	<tr><td>2022-04-23</td><td>61.4</td><td>48.5</td><td>54.8</td><td>44.7</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>148.9</td><td>12.9</td><td> 7</td><td>2022-04-23 06:11:54</td><td>2022-04-23 19:50:21</td><td> 48172</td><td> 48.172</td><td>818.4500</td><td>3.5833333</td><td>Historical</td></tr>
	<tr><td>2022-04-24</td><td>70.7</td><td>47.0</td><td>57.7</td><td>53.6</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>281.3</td><td>24.4</td><td> 9</td><td>2022-04-24 06:10:29</td><td>2022-04-24 19:51:24</td><td> 86429</td><td> 86.429</td><td>820.9167</td><td>6.7777778</td><td>Historical</td></tr>
	<tr><td>2022-04-25</td><td>59.4</td><td>43.4</td><td>51.8</td><td>63.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>144.0</td><td>12.3</td><td> 5</td><td>2022-04-25 06:09:06</td><td>2022-04-25 19:52:27</td><td> 46024</td><td> 46.024</td><td>823.3500</td><td>3.4166667</td><td>Historical</td></tr>
	<tr><td>2022-04-26</td><td>61.5</td><td>50.9</td><td>55.6</td><td>77.4</td><td>0.113</td><td>100</td><td> 37.50</td><td>rain         </td><td>0.0</td><td>⋯</td><td> 94.8</td><td> 8.3</td><td> 4</td><td>2022-04-26 06:07:43</td><td>2022-04-26 19:53:29</td><td> 27975</td><td> 27.975</td><td>825.7667</td><td>2.3055556</td><td>Historical</td></tr>
	<tr><td>2022-04-27</td><td>52.8</td><td>39.6</td><td>46.6</td><td>54.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>151.7</td><td>13.0</td><td> 6</td><td>2022-04-27 06:06:22</td><td>2022-04-27 19:54:32</td><td> 47856</td><td> 47.856</td><td>828.1667</td><td>3.6111111</td><td>Historical</td></tr>
	<tr><td>2022-04-28</td><td>53.9</td><td>36.6</td><td>44.3</td><td>36.4</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>311.1</td><td>26.8</td><td>10</td><td>2022-04-28 06:05:02</td><td>2022-04-28 19:55:35</td><td> 98083</td><td> 98.083</td><td>830.5500</td><td>7.4444444</td><td>Historical</td></tr>
	<tr><td>2022-04-29</td><td>61.3</td><td>32.9</td><td>48.2</td><td>27.5</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>317.7</td><td>27.4</td><td> 9</td><td>2022-04-29 06:03:42</td><td>2022-04-29 19:56:37</td><td>101514</td><td>101.514</td><td>832.9167</td><td>7.6111111</td><td>Historical</td></tr>
	<tr><td>2022-04-30</td><td>67.3</td><td>34.2</td><td>51.8</td><td>30.8</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>313.0</td><td>27.0</td><td> 9</td><td>2022-04-30 06:02:24</td><td>2022-04-30 19:57:39</td><td> 95528</td><td> 95.528</td><td>835.2500</td><td>7.5000000</td><td>Historical</td></tr>
	<tr><td>2022-05-01</td><td>69.8</td><td>35.5</td><td>53.3</td><td>56.0</td><td>0.009</td><td>100</td><td>  8.33</td><td>rain         </td><td>0.0</td><td>⋯</td><td>226.8</td><td>19.7</td><td> 9</td><td>2022-05-01 06:01:07</td><td>2022-05-01 19:58:42</td><td> 67771</td><td> 67.771</td><td>837.5833</td><td>5.4722222</td><td>Historical</td></tr>
	<tr><td>2022-05-02</td><td>63.6</td><td>51.4</td><td>56.6</td><td>80.5</td><td>0.123</td><td>100</td><td> 16.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td>147.8</td><td>12.7</td><td> 6</td><td>2022-05-02 05:59:52</td><td>2022-05-02 19:59:44</td><td> 43823</td><td> 43.823</td><td>839.8667</td><td>3.5277778</td><td>Historical</td></tr>
	<tr><td>2022-05-03</td><td>69.5</td><td>48.0</td><td>58.5</td><td>73.6</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>233.4</td><td>20.3</td><td> 8</td><td>2022-05-03 05:58:37</td><td>2022-05-03 20:00:46</td><td> 74164</td><td> 74.164</td><td>842.1500</td><td>5.6388889</td><td>Historical</td></tr>
	<tr><td>2022-05-04</td><td>62.4</td><td>49.3</td><td>54.5</td><td>86.7</td><td>0.094</td><td>100</td><td> 33.33</td><td>rain         </td><td>0.0</td><td>⋯</td><td> 88.0</td><td> 7.5</td><td> 2</td><td>2022-05-04 05:57:24</td><td>2022-05-04 20:01:47</td><td> 28102</td><td> 28.102</td><td>844.3833</td><td>2.0833333</td><td>Historical</td></tr>
	<tr><td>2022-05-05</td><td>71.8</td><td>51.9</td><td>62.6</td><td>55.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>290.5</td><td>25.1</td><td> 8</td><td>2022-05-05 05:56:12</td><td>2022-05-05 20:02:49</td><td> 88225</td><td> 88.225</td><td>846.6167</td><td>6.9722222</td><td>Historical</td></tr>
	<tr><td>2022-05-06</td><td>61.7</td><td>47.6</td><td>55.0</td><td>85.1</td><td>0.912</td><td>100</td><td> 66.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td> 38.8</td><td> 3.4</td><td> 1</td><td>2022-05-06 05:55:01</td><td>2022-05-06 20:03:50</td><td> 11759</td><td> 11.759</td><td>848.8167</td><td>0.9444444</td><td>Historical</td></tr>
	<tr><td>2022-05-07</td><td>50.0</td><td>43.8</td><td>47.0</td><td>88.8</td><td>1.119</td><td>100</td><td>100.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td> 39.5</td><td> 3.4</td><td> 1</td><td>2022-05-07 05:53:52</td><td>2022-05-07 20:04:51</td><td> 12844</td><td> 12.844</td><td>850.9833</td><td>0.9444444</td><td>Historical</td></tr>
	<tr><td>2022-05-08</td><td>60.9</td><td>41.6</td><td>50.8</td><td>52.1</td><td>0.056</td><td>100</td><td> 16.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td>266.3</td><td>22.9</td><td> 9</td><td>2022-05-08 05:52:44</td><td>2022-05-08 20:05:52</td><td> 84668</td><td> 84.668</td><td>853.1333</td><td>6.3611111</td><td>Historical</td></tr>
	<tr><td>2022-05-09</td><td>71.2</td><td>43.0</td><td>57.8</td><td>30.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>333.0</td><td>28.9</td><td> 9</td><td>2022-05-09 05:51:38</td><td>2022-05-09 20:06:52</td><td>101568</td><td>101.568</td><td>855.2333</td><td>8.0277778</td><td>Historical</td></tr>
	<tr><td>2022-05-10</td><td>72.5</td><td>46.5</td><td>60.2</td><td>28.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>330.7</td><td>28.6</td><td> 9</td><td>2022-05-10 05:50:33</td><td>2022-05-10 20:07:53</td><td>100930</td><td>100.930</td><td>857.3333</td><td>7.9444444</td><td>Historical</td></tr>
	<tr><td>2022-05-11</td><td>74.6</td><td>48.1</td><td>62.3</td><td>44.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>301.5</td><td>26.2</td><td> 8</td><td>2022-05-11 05:49:29</td><td>2022-05-11 20:08:52</td><td> 89071</td><td> 89.071</td><td>859.3833</td><td>7.2777778</td><td>Historical</td></tr>
	<tr><td>2022-05-12</td><td>77.6</td><td>50.7</td><td>66.1</td><td>69.3</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>260.0</td><td>22.5</td><td> 8</td><td>2022-05-12 05:48:27</td><td>2022-05-12 20:09:52</td><td> 77673</td><td> 77.673</td><td>861.4167</td><td>6.2500000</td><td>Historical</td></tr>
	<tr><td>2022-05-13</td><td>74.5</td><td>60.8</td><td>66.0</td><td>87.1</td><td>0.047</td><td>100</td><td> 12.50</td><td>rain         </td><td>0.0</td><td>⋯</td><td>116.2</td><td>10.2</td><td> 6</td><td>2022-05-13 05:47:27</td><td>2022-05-13 20:10:51</td><td> 34268</td><td> 34.268</td><td>863.4000</td><td>2.8333333</td><td>Historical</td></tr>
	<tr><td>2022-05-14</td><td>70.8</td><td>62.8</td><td>66.6</td><td>90.1</td><td>0.180</td><td>100</td><td> 41.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td> 92.2</td><td> 8.0</td><td> 4</td><td>2022-05-14 05:46:28</td><td>2022-05-14 20:11:49</td><td> 29334</td><td> 29.334</td><td>865.3500</td><td>2.2222222</td><td>Historical</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋱</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2023-08-16</td><td>81.1</td><td>68.9</td><td>74.2</td><td>78.1</td><td>0.126</td><td>100</td><td>12.50</td><td>rain</td><td>0</td><td>⋯</td><td>235.1</td><td>20.2</td><td> 8</td><td>2023-08-16 06:13:37</td><td>2023-08-16 19:59:19</td><td>65738</td><td>65.738</td><td>825.7000</td><td>5.6111111</td><td>Historical</td></tr>
	<tr><td>2023-08-17</td><td>85.5</td><td>65.8</td><td>76.2</td><td>73.6</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>194.4</td><td>16.7</td><td> 6</td><td>2023-08-17 06:14:36</td><td>2023-08-17 19:57:55</td><td>62420</td><td>62.420</td><td>823.3167</td><td>4.6388889</td><td>Historical</td></tr>
	<tr><td>2023-08-18</td><td>77.1</td><td>63.3</td><td>71.2</td><td>68.1</td><td>0.260</td><td>100</td><td>16.67</td><td>rain</td><td>0</td><td>⋯</td><td>208.6</td><td>18.1</td><td> 9</td><td>2023-08-18 06:15:35</td><td>2023-08-18 19:56:29</td><td>77216</td><td>77.216</td><td>820.9000</td><td>5.0277778</td><td>Historical</td></tr>
	<tr><td>2023-08-19</td><td>76.7</td><td>57.1</td><td>67.0</td><td>59.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>268.7</td><td>23.2</td><td> 8</td><td>2023-08-19 06:16:34</td><td>2023-08-19 19:55:03</td><td>86909</td><td>86.909</td><td>818.4833</td><td>6.4444444</td><td>Historical</td></tr>
	<tr><td>2023-08-20</td><td>82.4</td><td>57.7</td><td>70.4</td><td>68.0</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>252.9</td><td>22.0</td><td> 8</td><td>2023-08-20 06:17:33</td><td>2023-08-20 19:53:36</td><td>79169</td><td>79.169</td><td>816.0500</td><td>6.1111111</td><td>Historical</td></tr>
	<tr><td>2023-08-21</td><td>85.5</td><td>64.4</td><td>75.3</td><td>73.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>198.0</td><td>17.0</td><td> 7</td><td>2023-08-21 06:18:32</td><td>2023-08-21 19:52:07</td><td>61371</td><td>61.371</td><td>813.5833</td><td>4.7222222</td><td>Historical</td></tr>
	<tr><td>2023-08-22</td><td>78.4</td><td>63.1</td><td>71.5</td><td>56.7</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>261.8</td><td>22.5</td><td> 8</td><td>2023-08-22 06:19:31</td><td>2023-08-22 19:50:38</td><td>82996</td><td>82.996</td><td>811.1167</td><td>6.2500000</td><td>Historical</td></tr>
	<tr><td>2023-08-23</td><td>76.2</td><td>54.7</td><td>66.8</td><td>66.2</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>191.3</td><td>16.5</td><td> 8</td><td>2023-08-23 06:20:30</td><td>2023-08-23 19:49:08</td><td>56484</td><td>56.484</td><td>808.6333</td><td>4.5833333</td><td>Historical</td></tr>
	<tr><td>2023-08-24</td><td>69.9</td><td>65.8</td><td>67.6</td><td>77.9</td><td>0.119</td><td>100</td><td>37.50</td><td>rain</td><td>0</td><td>⋯</td><td> 32.6</td><td> 2.6</td><td> 1</td><td>2023-08-24 06:21:28</td><td>2023-08-24 19:47:38</td><td> 9917</td><td> 9.917</td><td>806.1667</td><td>0.7222222</td><td>Historical</td></tr>
	<tr><td>2023-08-25</td><td>81.0</td><td>66.9</td><td>72.6</td><td>87.1</td><td>0.254</td><td>100</td><td>20.83</td><td>rain</td><td>0</td><td>⋯</td><td>109.5</td><td> 9.4</td><td> 6</td><td>2023-08-25 06:22:27</td><td>2023-08-25 19:46:06</td><td>40754</td><td>40.754</td><td>803.6500</td><td>2.6111111</td><td>Historical</td></tr>
	<tr><td>2023-08-26</td><td>82.0</td><td>69.1</td><td>75.2</td><td>76.5</td><td>0.059</td><td>100</td><td>16.67</td><td>rain</td><td>0</td><td>⋯</td><td>189.0</td><td>16.4</td><td> 7</td><td>2023-08-26 06:23:26</td><td>2023-08-26 19:44:34</td><td>60767</td><td>60.767</td><td>801.1333</td><td>4.5555556</td><td>Historical</td></tr>
	<tr><td>2023-08-27</td><td>81.9</td><td>61.4</td><td>71.6</td><td>73.5</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>240.2</td><td>20.7</td><td> 8</td><td>2023-08-27 06:24:24</td><td>2023-08-27 19:43:01</td><td>77346</td><td>77.346</td><td>798.6167</td><td>5.7500000</td><td>Historical</td></tr>
	<tr><td>2023-08-28</td><td>77.1</td><td>65.2</td><td>71.4</td><td>77.7</td><td>0.005</td><td>100</td><td> 8.33</td><td>rain</td><td>0</td><td>⋯</td><td>109.5</td><td> 9.4</td><td> 4</td><td>2023-08-28 06:25:23</td><td>2023-08-28 19:41:27</td><td>34786</td><td>34.786</td><td>796.0667</td><td>2.6111111</td><td>Historical</td></tr>
	<tr><td>2023-08-29</td><td>77.7</td><td>66.9</td><td>71.8</td><td>80.0</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>116.2</td><td>10.0</td><td> 4</td><td>2023-08-29 06:26:22</td><td>2023-08-29 19:39:53</td><td>36573</td><td>36.573</td><td>793.5167</td><td>2.7777778</td><td>Historical</td></tr>
	<tr><td>2023-08-30</td><td>80.8</td><td>66.3</td><td>73.7</td><td>72.0</td><td>0.092</td><td>100</td><td>12.50</td><td>rain</td><td>0</td><td>⋯</td><td>227.0</td><td>19.6</td><td> 8</td><td>2023-08-30 06:27:20</td><td>2023-08-30 19:38:18</td><td>70375</td><td>70.375</td><td>790.9667</td><td>5.4444444</td><td>Historical</td></tr>
	<tr><td>2023-08-31</td><td>76.4</td><td>60.0</td><td>66.9</td><td>56.9</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>255.8</td><td>21.9</td><td> 8</td><td>2023-08-31 06:28:19</td><td>2023-08-31 19:36:42</td><td>82003</td><td>82.003</td><td>788.3833</td><td>6.0833333</td><td>Historical</td></tr>
	<tr><td>2023-09-01</td><td>75.1</td><td>50.6</td><td>63.6</td><td>64.5</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>256.4</td><td>22.3</td><td> 8</td><td>2023-09-01 06:29:17</td><td>2023-09-01 19:35:06</td><td>81127</td><td>81.127</td><td>785.8167</td><td>6.1944444</td><td>Historical</td></tr>
	<tr><td>2023-09-02</td><td>80.4</td><td>51.2</td><td>66.9</td><td>70.9</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>245.8</td><td>21.3</td><td> 8</td><td>2023-09-02 06:30:16</td><td>2023-09-02 19:33:29</td><td>79051</td><td>79.051</td><td>783.2167</td><td>5.9166667</td><td>Historical</td></tr>
	<tr><td>2023-09-03</td><td>88.0</td><td>61.7</td><td>74.7</td><td>70.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>232.8</td><td>20.2</td><td> 8</td><td>2023-09-03 06:31:14</td><td>2023-09-03 19:31:52</td><td>73460</td><td>73.460</td><td>780.6333</td><td>5.6111111</td><td>Historical</td></tr>
	<tr><td>2023-09-04</td><td>90.6</td><td>68.6</td><td>79.3</td><td>71.3</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>238.8</td><td>20.8</td><td> 8</td><td>2023-09-04 06:32:12</td><td>2023-09-04 19:30:14</td><td>70303</td><td>70.303</td><td>778.0333</td><td>5.7777778</td><td>Historical</td></tr>
	<tr><td>2023-09-05</td><td>89.9</td><td>71.2</td><td>80.1</td><td>72.4</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>220.5</td><td>19.1</td><td> 7</td><td>2023-09-05 06:33:11</td><td>2023-09-05 19:28:36</td><td>65123</td><td>65.123</td><td>775.4167</td><td>5.3055556</td><td>Historical</td></tr>
	<tr><td>2023-09-06</td><td>89.7</td><td>69.5</td><td>80.1</td><td>72.1</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>206.9</td><td>18.0</td><td> 7</td><td>2023-09-06 06:34:09</td><td>2023-09-06 19:26:57</td><td>66952</td><td>66.952</td><td>772.8000</td><td>5.0000000</td><td>Historical</td></tr>
	<tr><td>2023-09-07</td><td>90.6</td><td>67.7</td><td>75.3</td><td>81.3</td><td>0.341</td><td>100</td><td>25.00</td><td>rain</td><td>0</td><td>⋯</td><td>130.6</td><td>11.1</td><td> 8</td><td>2023-09-07 06:35:07</td><td>2023-09-07 19:25:18</td><td>37343</td><td>37.343</td><td>770.1833</td><td>3.0833333</td><td>Historical</td></tr>
	<tr><td>2023-09-08</td><td>87.0</td><td>66.2</td><td>73.9</td><td>82.1</td><td>0.390</td><td>100</td><td>29.17</td><td>rain</td><td>0</td><td>⋯</td><td>224.4</td><td>19.4</td><td> 7</td><td>2023-09-08 06:36:06</td><td>2023-09-08 19:23:39</td><td>65651</td><td>65.651</td><td>767.5500</td><td>5.3888889</td><td>Historical</td></tr>
	<tr><td>2023-09-09</td><td>84.7</td><td>65.6</td><td>72.8</td><td>84.3</td><td>0.502</td><td>100</td><td>25.00</td><td>rain</td><td>0</td><td>⋯</td><td>184.0</td><td>15.9</td><td> 7</td><td>2023-09-09 06:37:04</td><td>2023-09-09 19:21:59</td><td>53224</td><td>53.224</td><td>764.9167</td><td>4.4166667</td><td>Historical</td></tr>
	<tr><td>2023-09-10</td><td>77.3</td><td>65.3</td><td>69.6</td><td>90.4</td><td>0.697</td><td>100</td><td>45.83</td><td>rain</td><td>0</td><td>⋯</td><td> 93.4</td><td> 8.0</td><td> 5</td><td>2023-09-10 06:38:02</td><td>2023-09-10 19:20:19</td><td>27303</td><td>27.303</td><td>762.2833</td><td>2.2222222</td><td>Historical</td></tr>
	<tr><td>2023-09-11</td><td>79.8</td><td>67.5</td><td>71.8</td><td>86.6</td><td>0.144</td><td>100</td><td>29.17</td><td>rain</td><td>0</td><td>⋯</td><td>133.3</td><td>11.5</td><td>10</td><td>2023-09-11 06:39:01</td><td>2023-09-11 19:18:39</td><td>47781</td><td>47.781</td><td>759.6333</td><td>3.1944444</td><td>Historical</td></tr>
	<tr><td>2023-09-12</td><td>81.6</td><td>60.8</td><td>70.5</td><td>76.6</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>230.0</td><td>19.9</td><td> 8</td><td>2023-09-12 06:39:59</td><td>2023-09-12 19:16:58</td><td>67464</td><td>67.464</td><td>756.9833</td><td>5.5277778</td><td>Historical</td></tr>
	<tr><td>2023-09-13</td><td>78.6</td><td>62.3</td><td>70.3</td><td>78.9</td><td>0.307</td><td>100</td><td>20.83</td><td>rain</td><td>0</td><td>⋯</td><td>156.2</td><td>13.6</td><td> 7</td><td>2023-09-13 06:40:57</td><td>2023-09-13 19:15:18</td><td>54764</td><td>54.764</td><td>754.3500</td><td>3.7777778</td><td>Historical</td></tr>
	<tr><td>2023-09-14</td><td>73.3</td><td>58.5</td><td>64.8</td><td>66.7</td><td>0.000</td><td>  0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>224.4</td><td>19.3</td><td> 8</td><td>2023-09-14 06:41:56</td><td>2023-09-14 19:13:37</td><td>75767</td><td>75.767</td><td>751.6833</td><td>5.3611111</td><td>Historical</td></tr>
</tbody>
</table>




    
![png](output_13_1.png)
    



```R


solarproduction_model <- linear_reg() %>% 
  fit(`Energy Produced (kWh)` ~ solarenergy_kWh + 0, data=solar_weather)

tidy(solarproduction_model)
```


<table class="dataframe">
<caption>A tibble: 1 × 5</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>estimate</th><th scope=col>std.error</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>solarenergy_kWh</td><td>12.89479</td><td>0.05856253</td><td>220.1884</td><td>0</td></tr>
</tbody>
</table>




```R
#import_forecast <- read_csv("18078_visualcrossing_forecast_15day_230702.csv")

api_params_forecast <- "?unitGroup=us&elements=datetime%2CdatetimeEpoch%2Ctempmax%2Ctempmin%2Ctemp%2Chumidity%2Cprecip%2Cprecipprob%2Cprecipcover%2Cpreciptype%2Csnow%2Csnowdepth%2Ccloudcover%2Cvisibility%2Csolarradiation%2Csolarenergy%2Cuvindex%2Csunrise%2Csunset&include=days&key="

api_call_forecast <- paste(api_base_call,api_zip,'/',api_params_forecast,api_key,api_output, sep='')

api_response_forecast = content(GET(api_call_forecast))

(import_forecast <- read_csv(api_response_forecast, show_col_types = FALSE))

forecast <- import_forecast %>%  
   mutate(daylight = as.numeric(sunset-sunrise,units="mins"), solarenergy_kWh = solarenergy/3.6, type = "Forecast")

solar_forecast <- predict(solarproduction_model, new_data = forecast) 
solar_forecast_ci <- predict(solarproduction_model, new_data = forecast, type="conf_int")

#plot_data <- 
forecast <- forecast %>% 
  bind_cols(solar_forecast) %>% 
  bind_cols(solar_forecast_ci) 

forecast <- forecast %>% 
  mutate(`Energy Produced (kWh)` = .pred, `Energy Produced (Wh)` = `Energy Produced (kWh)`*1000)

forecast
```

    No encoding supplied: defaulting to UTF-8.
    



<table class="dataframe">
<caption>A spec_tbl_df: 15 × 18</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>tempmax</th><th scope=col>tempmin</th><th scope=col>temp</th><th scope=col>humidity</th><th scope=col>precip</th><th scope=col>precipprob</th><th scope=col>precipcover</th><th scope=col>preciptype</th><th scope=col>snow</th><th scope=col>snowdepth</th><th scope=col>cloudcover</th><th scope=col>visibility</th><th scope=col>solarradiation</th><th scope=col>solarenergy</th><th scope=col>uvindex</th><th scope=col>sunrise</th><th scope=col>sunset</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dttm&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2023-09-16</td><td>74.5</td><td>51.8</td><td>62.2</td><td>63.9</td><td>0.000</td><td> 0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td> 0.2</td><td> 9.9</td><td>236.5</td><td>20.4</td><td>8</td><td>2023-09-16 06:43:53</td><td>2023-09-16 19:10:14</td></tr>
	<tr><td>2023-09-17</td><td>71.0</td><td>49.0</td><td>60.0</td><td>78.4</td><td>0.393</td><td>76.0</td><td>29.17</td><td>rain</td><td>0</td><td>0</td><td>52.0</td><td> 8.0</td><td>124.3</td><td>10.8</td><td>5</td><td>2023-09-17 06:44:51</td><td>2023-09-17 19:08:33</td></tr>
	<tr><td>2023-09-18</td><td>71.0</td><td>56.9</td><td>63.1</td><td>83.8</td><td>0.530</td><td>69.0</td><td>37.50</td><td>rain</td><td>0</td><td>0</td><td>63.7</td><td> 7.6</td><td>154.8</td><td>13.5</td><td>5</td><td>2023-09-18 06:45:50</td><td>2023-09-18 19:06:52</td></tr>
	<tr><td>2023-09-19</td><td>70.4</td><td>55.8</td><td>61.9</td><td>76.5</td><td>0.012</td><td>36.0</td><td> 4.17</td><td>rain</td><td>0</td><td>0</td><td>13.6</td><td>10.7</td><td>236.3</td><td>20.3</td><td>6</td><td>2023-09-19 06:46:49</td><td>2023-09-19 19:05:10</td></tr>
	<tr><td>2023-09-20</td><td>71.9</td><td>51.3</td><td>61.0</td><td>74.1</td><td>0.854</td><td> 0.0</td><td> 4.17</td><td>rain</td><td>0</td><td>0</td><td> 6.8</td><td>15.0</td><td>239.9</td><td>20.6</td><td>6</td><td>2023-09-20 06:47:47</td><td>2023-09-20 19:03:29</td></tr>
	<tr><td>2023-09-21</td><td>73.1</td><td>53.3</td><td>62.3</td><td>73.0</td><td>0.000</td><td> 0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>15.1</td><td>15.0</td><td>231.8</td><td>19.9</td><td>6</td><td>2023-09-21 06:48:46</td><td>2023-09-21 19:01:47</td></tr>
	<tr><td>2023-09-22</td><td>71.5</td><td>54.4</td><td>62.2</td><td>76.5</td><td>0.000</td><td> 3.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>38.0</td><td>15.0</td><td>155.8</td><td>13.2</td><td>4</td><td>2023-09-22 06:49:46</td><td>2023-09-22 19:00:06</td></tr>
	<tr><td>2023-09-23</td><td>70.8</td><td>55.7</td><td>62.7</td><td>78.9</td><td>0.024</td><td>15.0</td><td> 8.33</td><td>rain</td><td>0</td><td>0</td><td>51.5</td><td>15.0</td><td>166.6</td><td>14.4</td><td>5</td><td>2023-09-23 06:50:45</td><td>2023-09-23 18:58:25</td></tr>
	<tr><td>2023-09-24</td><td>73.8</td><td>56.4</td><td>63.6</td><td>80.0</td><td>0.040</td><td>33.3</td><td> 8.33</td><td>rain</td><td>0</td><td>0</td><td>52.3</td><td>15.0</td><td>207.0</td><td>17.8</td><td>7</td><td>2023-09-24 06:51:44</td><td>2023-09-24 18:56:43</td></tr>
	<tr><td>2023-09-25</td><td>75.3</td><td>53.3</td><td>63.0</td><td>69.9</td><td>0.000</td><td>33.3</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>23.7</td><td>15.0</td><td>225.5</td><td>19.5</td><td>7</td><td>2023-09-25 06:52:44</td><td>2023-09-25 18:55:02</td></tr>
	<tr><td>2023-09-26</td><td>67.2</td><td>55.5</td><td>61.3</td><td>79.0</td><td>0.000</td><td>52.4</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td>95.5</td><td>15.0</td><td> 82.8</td><td> 7.0</td><td>3</td><td>2023-09-26 06:53:43</td><td>2023-09-26 18:53:21</td></tr>
	<tr><td>2023-09-27</td><td>76.6</td><td>61.4</td><td>66.8</td><td>85.1</td><td>0.000</td><td>38.1</td><td> 0.00</td><td>NA  </td><td>0</td><td>0</td><td>83.1</td><td>14.6</td><td>135.6</td><td>11.6</td><td>6</td><td>2023-09-27 06:54:43</td><td>2023-09-27 18:51:41</td></tr>
	<tr><td>2023-09-28</td><td>70.2</td><td>62.7</td><td>65.6</td><td>93.2</td><td>0.000</td><td>47.6</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td>86.2</td><td>12.2</td><td> 68.1</td><td> 5.9</td><td>2</td><td>2023-09-28 06:55:43</td><td>2023-09-28 18:50:00</td></tr>
	<tr><td>2023-09-29</td><td>69.0</td><td>52.2</td><td>61.9</td><td>74.1</td><td>0.000</td><td>52.4</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td>40.9</td><td>14.9</td><td>153.3</td><td>13.4</td><td>6</td><td>2023-09-29 06:56:43</td><td>2023-09-29 18:48:20</td></tr>
	<tr><td>2023-09-30</td><td>66.5</td><td>49.2</td><td>56.6</td><td>74.2</td><td>0.000</td><td>57.1</td><td> 0.00</td><td>rain</td><td>0</td><td>0</td><td> 0.2</td><td>15.0</td><td>209.9</td><td>18.2</td><td>7</td><td>2023-09-30 06:57:44</td><td>2023-09-30 18:46:40</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A tibble: 15 × 26</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>tempmax</th><th scope=col>tempmin</th><th scope=col>temp</th><th scope=col>humidity</th><th scope=col>precip</th><th scope=col>precipprob</th><th scope=col>precipcover</th><th scope=col>preciptype</th><th scope=col>snow</th><th scope=col>⋯</th><th scope=col>sunrise</th><th scope=col>sunset</th><th scope=col>daylight</th><th scope=col>solarenergy_kWh</th><th scope=col>type</th><th scope=col>.pred</th><th scope=col>.pred_lower</th><th scope=col>.pred_upper</th><th scope=col>Energy Produced (kWh)</th><th scope=col>Energy Produced (Wh)</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2023-09-16</td><td>74.5</td><td>51.8</td><td>62.2</td><td>63.9</td><td>0.000</td><td> 0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-16 06:43:53</td><td>2023-09-16 19:10:14</td><td>746.3500</td><td>5.666667</td><td>Forecast</td><td>73.07048</td><td>72.41853</td><td>73.72243</td><td>73.07048</td><td>73070.48</td></tr>
	<tr><td>2023-09-17</td><td>71.0</td><td>49.0</td><td>60.0</td><td>78.4</td><td>0.393</td><td>76.0</td><td>29.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-17 06:44:51</td><td>2023-09-17 19:08:33</td><td>743.7000</td><td>3.000000</td><td>Forecast</td><td>38.68437</td><td>38.33922</td><td>39.02952</td><td>38.68437</td><td>38684.37</td></tr>
	<tr><td>2023-09-18</td><td>71.0</td><td>56.9</td><td>63.1</td><td>83.8</td><td>0.530</td><td>69.0</td><td>37.50</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-18 06:45:50</td><td>2023-09-18 19:06:52</td><td>741.0333</td><td>3.750000</td><td>Forecast</td><td>48.35546</td><td>47.92403</td><td>48.78690</td><td>48.35546</td><td>48355.46</td></tr>
	<tr><td>2023-09-19</td><td>70.4</td><td>55.8</td><td>61.9</td><td>76.5</td><td>0.012</td><td>36.0</td><td> 4.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-19 06:46:49</td><td>2023-09-19 19:05:10</td><td>738.3500</td><td>5.638889</td><td>Forecast</td><td>72.71229</td><td>72.06354</td><td>73.36104</td><td>72.71229</td><td>72712.29</td></tr>
	<tr><td>2023-09-20</td><td>71.9</td><td>51.3</td><td>61.0</td><td>74.1</td><td>0.854</td><td> 0.0</td><td> 4.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-20 06:47:47</td><td>2023-09-20 19:03:29</td><td>735.7000</td><td>5.722222</td><td>Forecast</td><td>73.78686</td><td>73.12851</td><td>74.44520</td><td>73.78686</td><td>73786.86</td></tr>
	<tr><td>2023-09-21</td><td>73.1</td><td>53.3</td><td>62.3</td><td>73.0</td><td>0.000</td><td> 0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-21 06:48:46</td><td>2023-09-21 19:01:47</td><td>733.0167</td><td>5.527778</td><td>Forecast</td><td>71.27953</td><td>70.64357</td><td>71.91550</td><td>71.27953</td><td>71279.53</td></tr>
	<tr><td>2023-09-22</td><td>71.5</td><td>54.4</td><td>62.2</td><td>76.5</td><td>0.000</td><td> 3.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-22 06:49:46</td><td>2023-09-22 19:00:06</td><td>730.3333</td><td>3.666667</td><td>Forecast</td><td>47.28090</td><td>46.85905</td><td>47.70275</td><td>47.28090</td><td>47280.90</td></tr>
	<tr><td>2023-09-23</td><td>70.8</td><td>55.7</td><td>62.7</td><td>78.9</td><td>0.024</td><td>15.0</td><td> 8.33</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-23 06:50:45</td><td>2023-09-23 18:58:25</td><td>727.6667</td><td>4.000000</td><td>Forecast</td><td>51.57916</td><td>51.11896</td><td>52.03936</td><td>51.57916</td><td>51579.16</td></tr>
	<tr><td>2023-09-24</td><td>73.8</td><td>56.4</td><td>63.6</td><td>80.0</td><td>0.040</td><td>33.3</td><td> 8.33</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-24 06:51:44</td><td>2023-09-24 18:56:43</td><td>724.9833</td><td>4.944444</td><td>Forecast</td><td>63.75757</td><td>63.18872</td><td>64.32643</td><td>63.75757</td><td>63757.57</td></tr>
	<tr><td>2023-09-25</td><td>75.3</td><td>53.3</td><td>63.0</td><td>69.9</td><td>0.000</td><td>33.3</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-25 06:52:44</td><td>2023-09-25 18:55:02</td><td>722.3000</td><td>5.416667</td><td>Forecast</td><td>69.84678</td><td>69.22359</td><td>70.46997</td><td>69.84678</td><td>69846.78</td></tr>
	<tr><td>2023-09-26</td><td>67.2</td><td>55.5</td><td>61.3</td><td>79.0</td><td>0.000</td><td>52.4</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-26 06:53:43</td><td>2023-09-26 18:53:21</td><td>719.6333</td><td>1.944444</td><td>Forecast</td><td>25.07320</td><td>24.84950</td><td>25.29691</td><td>25.07320</td><td>25073.20</td></tr>
	<tr><td>2023-09-27</td><td>76.6</td><td>61.4</td><td>66.8</td><td>85.1</td><td>0.000</td><td>38.1</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-27 06:54:43</td><td>2023-09-27 18:51:41</td><td>716.9667</td><td>3.222222</td><td>Forecast</td><td>41.54988</td><td>41.17916</td><td>41.92060</td><td>41.54988</td><td>41549.88</td></tr>
	<tr><td>2023-09-28</td><td>70.2</td><td>62.7</td><td>65.6</td><td>93.2</td><td>0.000</td><td>47.6</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-28 06:55:43</td><td>2023-09-28 18:50:00</td><td>714.2833</td><td>1.638889</td><td>Forecast</td><td>21.13313</td><td>20.94457</td><td>21.32168</td><td>21.13313</td><td>21133.13</td></tr>
	<tr><td>2023-09-29</td><td>69.0</td><td>52.2</td><td>61.9</td><td>74.1</td><td>0.000</td><td>52.4</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-29 06:56:43</td><td>2023-09-29 18:48:20</td><td>711.6167</td><td>3.722222</td><td>Forecast</td><td>47.99727</td><td>47.56903</td><td>48.42552</td><td>47.99727</td><td>47997.27</td></tr>
	<tr><td>2023-09-30</td><td>66.5</td><td>49.2</td><td>56.6</td><td>74.2</td><td>0.000</td><td>57.1</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-30 06:57:44</td><td>2023-09-30 18:46:40</td><td>708.9333</td><td>5.055556</td><td>Forecast</td><td>65.19033</td><td>64.60869</td><td>65.77197</td><td>65.19033</td><td>65190.33</td></tr>
</tbody>
</table>




```R
(solar_weather <-  bind_rows(solar_weather,forecast))

ggplot(data=solar_weather, aes(datetime,`Energy Produced (kWh)`)) +
  geom_point(aes(group=type, color=type)) + 
  #geom_smooth(span=.4) +
  xlab("Date") +
  theme(axis.text.x = element_text(angle = 90)) +
  scale_x_date(date_breaks = "1 month", date_labels = "%b %Y") +
  ggtitle("Residential Solar System (15.5 kW)") 

```


<table class="dataframe">
<caption>A tibble: 533 × 26</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>tempmax</th><th scope=col>tempmin</th><th scope=col>temp</th><th scope=col>humidity</th><th scope=col>precip</th><th scope=col>precipprob</th><th scope=col>precipcover</th><th scope=col>preciptype</th><th scope=col>snow</th><th scope=col>⋯</th><th scope=col>sunrise</th><th scope=col>sunset</th><th scope=col>Energy Produced (Wh)</th><th scope=col>Energy Produced (kWh)</th><th scope=col>daylight</th><th scope=col>solarenergy_kWh</th><th scope=col>type</th><th scope=col>.pred</th><th scope=col>.pred_lower</th><th scope=col>.pred_upper</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2022-04-15</td><td>67.2</td><td>36.8</td><td>55.6</td><td>45.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-15 06:23:41</td><td>2022-04-15 19:41:59</td><td>  6865</td><td>  6.865</td><td>798.3000</td><td>6.9444444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-16</td><td>69.3</td><td>45.5</td><td>58.2</td><td>49.1</td><td>0.031</td><td>100</td><td> 12.50</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-04-16 06:22:09</td><td>2022-04-16 19:43:02</td><td> 48182</td><td> 48.182</td><td>800.8833</td><td>3.7500000</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-17</td><td>47.3</td><td>34.1</td><td>42.5</td><td>55.2</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-17 06:20:39</td><td>2022-04-17 19:44:05</td><td> 77459</td><td> 77.459</td><td>803.4333</td><td>5.1944444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-18</td><td>48.5</td><td>27.8</td><td>38.3</td><td>66.2</td><td>0.580</td><td>100</td><td> 33.33</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-04-18 06:19:09</td><td>2022-04-18 19:45:07</td><td> 35148</td><td> 35.148</td><td>805.9667</td><td>2.3611111</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-19</td><td>48.7</td><td>37.3</td><td>41.6</td><td>68.7</td><td>0.582</td><td>100</td><td> 33.33</td><td>rain,snow,ice</td><td>0.3</td><td>⋯</td><td>2022-04-19 06:17:41</td><td>2022-04-19 19:46:10</td><td> 59849</td><td> 59.849</td><td>808.4833</td><td>4.3333333</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-20</td><td>57.0</td><td>38.0</td><td>46.6</td><td>45.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-20 06:16:13</td><td>2022-04-20 19:47:13</td><td> 95562</td><td> 95.562</td><td>811.0000</td><td>7.1944444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-21</td><td>61.0</td><td>37.8</td><td>50.8</td><td>58.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-04-21 06:14:45</td><td>2022-04-21 19:48:16</td><td> 41926</td><td> 41.926</td><td>813.5167</td><td>3.0555556</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-22</td><td>67.3</td><td>41.2</td><td>55.0</td><td>53.5</td><td>0.007</td><td>100</td><td>  4.17</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-04-22 06:13:19</td><td>2022-04-22 19:49:19</td><td> 87198</td><td> 87.198</td><td>816.0000</td><td>6.8611111</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-23</td><td>61.4</td><td>48.5</td><td>54.8</td><td>44.7</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-23 06:11:54</td><td>2022-04-23 19:50:21</td><td> 48172</td><td> 48.172</td><td>818.4500</td><td>3.5833333</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-24</td><td>70.7</td><td>47.0</td><td>57.7</td><td>53.6</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-24 06:10:29</td><td>2022-04-24 19:51:24</td><td> 86429</td><td> 86.429</td><td>820.9167</td><td>6.7777778</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-25</td><td>59.4</td><td>43.4</td><td>51.8</td><td>63.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-25 06:09:06</td><td>2022-04-25 19:52:27</td><td> 46024</td><td> 46.024</td><td>823.3500</td><td>3.4166667</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-26</td><td>61.5</td><td>50.9</td><td>55.6</td><td>77.4</td><td>0.113</td><td>100</td><td> 37.50</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-04-26 06:07:43</td><td>2022-04-26 19:53:29</td><td> 27975</td><td> 27.975</td><td>825.7667</td><td>2.3055556</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-27</td><td>52.8</td><td>39.6</td><td>46.6</td><td>54.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-04-27 06:06:22</td><td>2022-04-27 19:54:32</td><td> 47856</td><td> 47.856</td><td>828.1667</td><td>3.6111111</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-28</td><td>53.9</td><td>36.6</td><td>44.3</td><td>36.4</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-28 06:05:02</td><td>2022-04-28 19:55:35</td><td> 98083</td><td> 98.083</td><td>830.5500</td><td>7.4444444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-29</td><td>61.3</td><td>32.9</td><td>48.2</td><td>27.5</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-29 06:03:42</td><td>2022-04-29 19:56:37</td><td>101514</td><td>101.514</td><td>832.9167</td><td>7.6111111</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-04-30</td><td>67.3</td><td>34.2</td><td>51.8</td><td>30.8</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-04-30 06:02:24</td><td>2022-04-30 19:57:39</td><td> 95528</td><td> 95.528</td><td>835.2500</td><td>7.5000000</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-01</td><td>69.8</td><td>35.5</td><td>53.3</td><td>56.0</td><td>0.009</td><td>100</td><td>  8.33</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-01 06:01:07</td><td>2022-05-01 19:58:42</td><td> 67771</td><td> 67.771</td><td>837.5833</td><td>5.4722222</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-02</td><td>63.6</td><td>51.4</td><td>56.6</td><td>80.5</td><td>0.123</td><td>100</td><td> 16.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-02 05:59:52</td><td>2022-05-02 19:59:44</td><td> 43823</td><td> 43.823</td><td>839.8667</td><td>3.5277778</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-03</td><td>69.5</td><td>48.0</td><td>58.5</td><td>73.6</td><td>0.000</td><td>  0</td><td>  0.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-03 05:58:37</td><td>2022-05-03 20:00:46</td><td> 74164</td><td> 74.164</td><td>842.1500</td><td>5.6388889</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-04</td><td>62.4</td><td>49.3</td><td>54.5</td><td>86.7</td><td>0.094</td><td>100</td><td> 33.33</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-04 05:57:24</td><td>2022-05-04 20:01:47</td><td> 28102</td><td> 28.102</td><td>844.3833</td><td>2.0833333</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-05</td><td>71.8</td><td>51.9</td><td>62.6</td><td>55.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-05-05 05:56:12</td><td>2022-05-05 20:02:49</td><td> 88225</td><td> 88.225</td><td>846.6167</td><td>6.9722222</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-06</td><td>61.7</td><td>47.6</td><td>55.0</td><td>85.1</td><td>0.912</td><td>100</td><td> 66.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-06 05:55:01</td><td>2022-05-06 20:03:50</td><td> 11759</td><td> 11.759</td><td>848.8167</td><td>0.9444444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-07</td><td>50.0</td><td>43.8</td><td>47.0</td><td>88.8</td><td>1.119</td><td>100</td><td>100.00</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-07 05:53:52</td><td>2022-05-07 20:04:51</td><td> 12844</td><td> 12.844</td><td>850.9833</td><td>0.9444444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-08</td><td>60.9</td><td>41.6</td><td>50.8</td><td>52.1</td><td>0.056</td><td>100</td><td> 16.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-08 05:52:44</td><td>2022-05-08 20:05:52</td><td> 84668</td><td> 84.668</td><td>853.1333</td><td>6.3611111</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-09</td><td>71.2</td><td>43.0</td><td>57.8</td><td>30.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-05-09 05:51:38</td><td>2022-05-09 20:06:52</td><td>101568</td><td>101.568</td><td>855.2333</td><td>8.0277778</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-10</td><td>72.5</td><td>46.5</td><td>60.2</td><td>28.1</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-05-10 05:50:33</td><td>2022-05-10 20:07:53</td><td>100930</td><td>100.930</td><td>857.3333</td><td>7.9444444</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-11</td><td>74.6</td><td>48.1</td><td>62.3</td><td>44.9</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-05-11 05:49:29</td><td>2022-05-11 20:08:52</td><td> 89071</td><td> 89.071</td><td>859.3833</td><td>7.2777778</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-12</td><td>77.6</td><td>50.7</td><td>66.1</td><td>69.3</td><td>0.000</td><td>  0</td><td>  0.00</td><td>NA           </td><td>0.0</td><td>⋯</td><td>2022-05-12 05:48:27</td><td>2022-05-12 20:09:52</td><td> 77673</td><td> 77.673</td><td>861.4167</td><td>6.2500000</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-13</td><td>74.5</td><td>60.8</td><td>66.0</td><td>87.1</td><td>0.047</td><td>100</td><td> 12.50</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-13 05:47:27</td><td>2022-05-13 20:10:51</td><td> 34268</td><td> 34.268</td><td>863.4000</td><td>2.8333333</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>2022-05-14</td><td>70.8</td><td>62.8</td><td>66.6</td><td>90.1</td><td>0.180</td><td>100</td><td> 41.67</td><td>rain         </td><td>0.0</td><td>⋯</td><td>2022-05-14 05:46:28</td><td>2022-05-14 20:11:49</td><td> 29334</td><td> 29.334</td><td>865.3500</td><td>2.2222222</td><td>Historical</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋱</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2023-08-31</td><td>76.4</td><td>60.0</td><td>66.9</td><td>56.9</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-08-31 06:28:19</td><td>2023-08-31 19:36:42</td><td>82003.00</td><td>82.00300</td><td>788.3833</td><td>6.083333</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-01</td><td>75.1</td><td>50.6</td><td>63.6</td><td>64.5</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-01 06:29:17</td><td>2023-09-01 19:35:06</td><td>81127.00</td><td>81.12700</td><td>785.8167</td><td>6.194444</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-02</td><td>80.4</td><td>51.2</td><td>66.9</td><td>70.9</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-02 06:30:16</td><td>2023-09-02 19:33:29</td><td>79051.00</td><td>79.05100</td><td>783.2167</td><td>5.916667</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-03</td><td>88.0</td><td>61.7</td><td>74.7</td><td>70.3</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-03 06:31:14</td><td>2023-09-03 19:31:52</td><td>73460.00</td><td>73.46000</td><td>780.6333</td><td>5.611111</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-04</td><td>90.6</td><td>68.6</td><td>79.3</td><td>71.3</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-04 06:32:12</td><td>2023-09-04 19:30:14</td><td>70303.00</td><td>70.30300</td><td>778.0333</td><td>5.777778</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-05</td><td>89.9</td><td>71.2</td><td>80.1</td><td>72.4</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-05 06:33:11</td><td>2023-09-05 19:28:36</td><td>65123.00</td><td>65.12300</td><td>775.4167</td><td>5.305556</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-06</td><td>89.7</td><td>69.5</td><td>80.1</td><td>72.1</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-06 06:34:09</td><td>2023-09-06 19:26:57</td><td>66952.00</td><td>66.95200</td><td>772.8000</td><td>5.000000</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-07</td><td>90.6</td><td>67.7</td><td>75.3</td><td>81.3</td><td>0.341</td><td>100.0</td><td>25.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-07 06:35:07</td><td>2023-09-07 19:25:18</td><td>37343.00</td><td>37.34300</td><td>770.1833</td><td>3.083333</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-08</td><td>87.0</td><td>66.2</td><td>73.9</td><td>82.1</td><td>0.390</td><td>100.0</td><td>29.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-08 06:36:06</td><td>2023-09-08 19:23:39</td><td>65651.00</td><td>65.65100</td><td>767.5500</td><td>5.388889</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-09</td><td>84.7</td><td>65.6</td><td>72.8</td><td>84.3</td><td>0.502</td><td>100.0</td><td>25.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-09 06:37:04</td><td>2023-09-09 19:21:59</td><td>53224.00</td><td>53.22400</td><td>764.9167</td><td>4.416667</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-10</td><td>77.3</td><td>65.3</td><td>69.6</td><td>90.4</td><td>0.697</td><td>100.0</td><td>45.83</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-10 06:38:02</td><td>2023-09-10 19:20:19</td><td>27303.00</td><td>27.30300</td><td>762.2833</td><td>2.222222</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-11</td><td>79.8</td><td>67.5</td><td>71.8</td><td>86.6</td><td>0.144</td><td>100.0</td><td>29.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-11 06:39:01</td><td>2023-09-11 19:18:39</td><td>47781.00</td><td>47.78100</td><td>759.6333</td><td>3.194444</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-12</td><td>81.6</td><td>60.8</td><td>70.5</td><td>76.6</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-12 06:39:59</td><td>2023-09-12 19:16:58</td><td>67464.00</td><td>67.46400</td><td>756.9833</td><td>5.527778</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-13</td><td>78.6</td><td>62.3</td><td>70.3</td><td>78.9</td><td>0.307</td><td>100.0</td><td>20.83</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-13 06:40:57</td><td>2023-09-13 19:15:18</td><td>54764.00</td><td>54.76400</td><td>754.3500</td><td>3.777778</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-14</td><td>73.3</td><td>58.5</td><td>64.8</td><td>66.7</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-14 06:41:56</td><td>2023-09-14 19:13:37</td><td>75767.00</td><td>75.76700</td><td>751.6833</td><td>5.361111</td><td>Historical</td><td>      NA</td><td>      NA</td><td>      NA</td></tr>
	<tr><td>2023-09-16</td><td>74.5</td><td>51.8</td><td>62.2</td><td>63.9</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-16 06:43:53</td><td>2023-09-16 19:10:14</td><td>73070.48</td><td>73.07048</td><td>746.3500</td><td>5.666667</td><td>Forecast  </td><td>73.07048</td><td>72.41853</td><td>73.72243</td></tr>
	<tr><td>2023-09-17</td><td>71.0</td><td>49.0</td><td>60.0</td><td>78.4</td><td>0.393</td><td> 76.0</td><td>29.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-17 06:44:51</td><td>2023-09-17 19:08:33</td><td>38684.37</td><td>38.68437</td><td>743.7000</td><td>3.000000</td><td>Forecast  </td><td>38.68437</td><td>38.33922</td><td>39.02952</td></tr>
	<tr><td>2023-09-18</td><td>71.0</td><td>56.9</td><td>63.1</td><td>83.8</td><td>0.530</td><td> 69.0</td><td>37.50</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-18 06:45:50</td><td>2023-09-18 19:06:52</td><td>48355.46</td><td>48.35546</td><td>741.0333</td><td>3.750000</td><td>Forecast  </td><td>48.35546</td><td>47.92403</td><td>48.78690</td></tr>
	<tr><td>2023-09-19</td><td>70.4</td><td>55.8</td><td>61.9</td><td>76.5</td><td>0.012</td><td> 36.0</td><td> 4.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-19 06:46:49</td><td>2023-09-19 19:05:10</td><td>72712.29</td><td>72.71229</td><td>738.3500</td><td>5.638889</td><td>Forecast  </td><td>72.71229</td><td>72.06354</td><td>73.36104</td></tr>
	<tr><td>2023-09-20</td><td>71.9</td><td>51.3</td><td>61.0</td><td>74.1</td><td>0.854</td><td>  0.0</td><td> 4.17</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-20 06:47:47</td><td>2023-09-20 19:03:29</td><td>73786.86</td><td>73.78686</td><td>735.7000</td><td>5.722222</td><td>Forecast  </td><td>73.78686</td><td>73.12851</td><td>74.44520</td></tr>
	<tr><td>2023-09-21</td><td>73.1</td><td>53.3</td><td>62.3</td><td>73.0</td><td>0.000</td><td>  0.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-21 06:48:46</td><td>2023-09-21 19:01:47</td><td>71279.53</td><td>71.27953</td><td>733.0167</td><td>5.527778</td><td>Forecast  </td><td>71.27953</td><td>70.64357</td><td>71.91550</td></tr>
	<tr><td>2023-09-22</td><td>71.5</td><td>54.4</td><td>62.2</td><td>76.5</td><td>0.000</td><td>  3.0</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-22 06:49:46</td><td>2023-09-22 19:00:06</td><td>47280.90</td><td>47.28090</td><td>730.3333</td><td>3.666667</td><td>Forecast  </td><td>47.28090</td><td>46.85905</td><td>47.70275</td></tr>
	<tr><td>2023-09-23</td><td>70.8</td><td>55.7</td><td>62.7</td><td>78.9</td><td>0.024</td><td> 15.0</td><td> 8.33</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-23 06:50:45</td><td>2023-09-23 18:58:25</td><td>51579.16</td><td>51.57916</td><td>727.6667</td><td>4.000000</td><td>Forecast  </td><td>51.57916</td><td>51.11896</td><td>52.03936</td></tr>
	<tr><td>2023-09-24</td><td>73.8</td><td>56.4</td><td>63.6</td><td>80.0</td><td>0.040</td><td> 33.3</td><td> 8.33</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-24 06:51:44</td><td>2023-09-24 18:56:43</td><td>63757.57</td><td>63.75757</td><td>724.9833</td><td>4.944444</td><td>Forecast  </td><td>63.75757</td><td>63.18872</td><td>64.32643</td></tr>
	<tr><td>2023-09-25</td><td>75.3</td><td>53.3</td><td>63.0</td><td>69.9</td><td>0.000</td><td> 33.3</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-25 06:52:44</td><td>2023-09-25 18:55:02</td><td>69846.78</td><td>69.84678</td><td>722.3000</td><td>5.416667</td><td>Forecast  </td><td>69.84678</td><td>69.22359</td><td>70.46997</td></tr>
	<tr><td>2023-09-26</td><td>67.2</td><td>55.5</td><td>61.3</td><td>79.0</td><td>0.000</td><td> 52.4</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-26 06:53:43</td><td>2023-09-26 18:53:21</td><td>25073.20</td><td>25.07320</td><td>719.6333</td><td>1.944444</td><td>Forecast  </td><td>25.07320</td><td>24.84950</td><td>25.29691</td></tr>
	<tr><td>2023-09-27</td><td>76.6</td><td>61.4</td><td>66.8</td><td>85.1</td><td>0.000</td><td> 38.1</td><td> 0.00</td><td>NA  </td><td>0</td><td>⋯</td><td>2023-09-27 06:54:43</td><td>2023-09-27 18:51:41</td><td>41549.88</td><td>41.54988</td><td>716.9667</td><td>3.222222</td><td>Forecast  </td><td>41.54988</td><td>41.17916</td><td>41.92060</td></tr>
	<tr><td>2023-09-28</td><td>70.2</td><td>62.7</td><td>65.6</td><td>93.2</td><td>0.000</td><td> 47.6</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-28 06:55:43</td><td>2023-09-28 18:50:00</td><td>21133.13</td><td>21.13313</td><td>714.2833</td><td>1.638889</td><td>Forecast  </td><td>21.13313</td><td>20.94457</td><td>21.32168</td></tr>
	<tr><td>2023-09-29</td><td>69.0</td><td>52.2</td><td>61.9</td><td>74.1</td><td>0.000</td><td> 52.4</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-29 06:56:43</td><td>2023-09-29 18:48:20</td><td>47997.27</td><td>47.99727</td><td>711.6167</td><td>3.722222</td><td>Forecast  </td><td>47.99727</td><td>47.56903</td><td>48.42552</td></tr>
	<tr><td>2023-09-30</td><td>66.5</td><td>49.2</td><td>56.6</td><td>74.2</td><td>0.000</td><td> 57.1</td><td> 0.00</td><td>rain</td><td>0</td><td>⋯</td><td>2023-09-30 06:57:44</td><td>2023-09-30 18:46:40</td><td>65190.33</td><td>65.19033</td><td>708.9333</td><td>5.055556</td><td>Forecast  </td><td>65.19033</td><td>64.60869</td><td>65.77197</td></tr>
</tbody>
</table>




    
![png](output_16_1.png)
    



```R

```


```R

```


```R

```
