# Hourly Electricity Production & Consumption


```R

library(tidyverse)
library(lubridate)
library(hms)
library(googlesheets4)
#library(ggpubr)
#library(tidymodels)
#library(httr)
```

    ── [1mAttaching core tidyverse packages[22m ──────────────────────── tidyverse 2.0.0 ──
    [32m✔[39m [34mdplyr    [39m 1.1.3     [32m✔[39m [34mreadr    [39m 2.1.4
    [32m✔[39m [34mforcats  [39m 1.0.0     [32m✔[39m [34mstringr  [39m 1.5.0
    [32m✔[39m [34mggplot2  [39m 3.4.3     [32m✔[39m [34mtibble   [39m 3.2.1
    [32m✔[39m [34mlubridate[39m 1.9.2     [32m✔[39m [34mtidyr    [39m 1.3.0
    [32m✔[39m [34mpurrr    [39m 1.0.1     
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    [36mℹ[39m Use the conflicted package ([3m[34m<http://conflicted.r-lib.org/>[39m[23m) to force all conflicts to become errors
    
    Attaching package: ‘hms’
    
    
    The following object is masked from ‘package:lubridate’:
    
        hms
    
    



```R

gs4_deauth()
ppl_15mins <- read_sheet("https://docs.google.com/spreadsheets/d/1uv5SBYklQ-bArCnWPVG1hOvKu3fpgD1sgU8bTZ8sR5o/edit?usp=sharing")

ppl_15mins 

```

    [32m✔[39m Reading from [36mppl_hourly_usage_230901_230915[39m.
    
    [32m✔[39m Range [33m1[39m.
    



<table class="dataframe">
<caption>A tibble: 48 × 103</caption>
<thead>
	<tr><th scope=col>Account Number</th><th scope=col>Meter Number</th><th scope=col>Date</th><th scope=col>Read Type</th><th scope=col>Min</th><th scope=col>Max</th><th scope=col>Total</th><th scope=col>12:00 AM</th><th scope=col>12:15 AM</th><th scope=col>12:30 AM</th><th scope=col>⋯</th><th scope=col>9:30 PM</th><th scope=col>9:45 PM</th><th scope=col>10:00 PM</th><th scope=col>10:15 PM</th><th scope=col>10:30 PM</th><th scope=col>10:45 PM</th><th scope=col>11:00 PM</th><th scope=col>11:15 PM</th><th scope=col>11:30 PM</th><th scope=col>11:45 PM</th></tr>
	<tr><th scope=col>&lt;list&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net            </td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>2.66</td><td>2.66</td><td>2.56</td><td>⋯</td><td>0.19</td><td>0.22</td><td>0.21</td><td>0.20</td><td>0.22</td><td>0.19</td><td>0.19</td><td>0.19</td><td>0.18</td><td>0.20</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>Delivered Usage kWh</td><td> 0.00</td><td>2.66</td><td> 19.65</td><td>2.66</td><td>2.66</td><td>2.56</td><td>⋯</td><td>0.19</td><td>0.22</td><td>0.21</td><td>0.20</td><td>0.22</td><td>0.19</td><td>0.19</td><td>0.19</td><td>0.18</td><td>0.20</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>Received Usage kWh </td><td> 0.00</td><td>2.17</td><td> 69.23</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-14</td><td>kWh Net            </td><td>-2.20</td><td>2.74</td><td>-20.52</td><td>0.12</td><td>0.11</td><td>0.10</td><td>⋯</td><td>2.69</td><td>2.64</td><td>2.74</td><td>2.70</td><td>2.67</td><td>2.67</td><td>2.66</td><td>2.63</td><td>2.66</td><td>2.65</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-14</td><td>Delivered Usage kWh</td><td> 0.00</td><td>2.74</td><td> 38.32</td><td>0.12</td><td>0.11</td><td>0.10</td><td>⋯</td><td>2.69</td><td>2.64</td><td>2.74</td><td>2.70</td><td>2.67</td><td>2.67</td><td>2.66</td><td>2.63</td><td>2.66</td><td>2.65</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-14</td><td>Received Usage kWh </td><td> 0.00</td><td>2.20</td><td> 58.84</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-13</td><td>kWh Net            </td><td>-2.28</td><td>0.67</td><td>-29.73</td><td>0.22</td><td>0.15</td><td>0.34</td><td>⋯</td><td>0.27</td><td>0.67</td><td>0.25</td><td>0.24</td><td>0.18</td><td>0.31</td><td>0.11</td><td>0.11</td><td>0.10</td><td>0.26</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-13</td><td>Delivered Usage kWh</td><td> 0.00</td><td>0.67</td><td> 14.73</td><td>0.22</td><td>0.15</td><td>0.34</td><td>⋯</td><td>0.27</td><td>0.67</td><td>0.25</td><td>0.24</td><td>0.18</td><td>0.31</td><td>0.11</td><td>0.11</td><td>0.10</td><td>0.26</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-13</td><td>Received Usage kWh </td><td> 0.00</td><td>2.28</td><td> 44.46</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-12</td><td>kWh Net            </td><td>-2.08</td><td>2.78</td><td>-21.08</td><td>2.67</td><td>2.78</td><td>2.65</td><td>⋯</td><td>0.23</td><td>0.65</td><td>0.26</td><td>0.51</td><td>0.37</td><td>0.42</td><td>0.24</td><td>0.47</td><td>0.21</td><td>0.46</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-12</td><td>Delivered Usage kWh</td><td> 0.00</td><td>2.78</td><td> 29.32</td><td>2.67</td><td>2.78</td><td>2.65</td><td>⋯</td><td>0.23</td><td>0.65</td><td>0.26</td><td>0.51</td><td>0.37</td><td>0.42</td><td>0.24</td><td>0.47</td><td>0.21</td><td>0.46</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-12</td><td>Received Usage kWh </td><td> 0.00</td><td>2.08</td><td> 50.40</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-11</td><td>kWh Net            </td><td>-1.72</td><td>2.94</td><td> 14.02</td><td>0.27</td><td>0.25</td><td>0.23</td><td>⋯</td><td>2.65</td><td>2.94</td><td>2.73</td><td>2.67</td><td>2.85</td><td>2.70</td><td>2.69</td><td>2.74</td><td>2.79</td><td>2.66</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-11</td><td>Delivered Usage kWh</td><td> 0.00</td><td>2.94</td><td> 44.96</td><td>0.27</td><td>0.25</td><td>0.23</td><td>⋯</td><td>2.65</td><td>2.94</td><td>2.73</td><td>2.67</td><td>2.85</td><td>2.70</td><td>2.69</td><td>2.74</td><td>2.79</td><td>2.66</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-11</td><td>Received Usage kWh </td><td> 0.00</td><td>1.72</td><td> 30.94</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-10</td><td>kWh Net            </td><td>-1.31</td><td>0.90</td><td> -1.10</td><td>0.26</td><td>0.24</td><td>0.18</td><td>⋯</td><td>0.25</td><td>0.26</td><td>0.39</td><td>0.22</td><td>0.24</td><td>0.42</td><td>0.25</td><td>0.26</td><td>0.45</td><td>0.26</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-10</td><td>Delivered Usage kWh</td><td> 0.00</td><td>0.91</td><td> 18.06</td><td>0.26</td><td>0.24</td><td>0.18</td><td>⋯</td><td>0.25</td><td>0.26</td><td>0.39</td><td>0.22</td><td>0.24</td><td>0.42</td><td>0.25</td><td>0.26</td><td>0.45</td><td>0.26</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-10</td><td>Received Usage kWh </td><td> 0.00</td><td>1.31</td><td> 19.16</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-09</td><td>kWh Net            </td><td>-2.18</td><td>2.90</td><td>  3.17</td><td>2.84</td><td>2.90</td><td>2.56</td><td>⋯</td><td>0.99</td><td>0.78</td><td>0.91</td><td>0.95</td><td>0.77</td><td>0.93</td><td>0.60</td><td>0.27</td><td>0.47</td><td>0.26</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-09</td><td>Delivered Usage kWh</td><td> 0.00</td><td>2.90</td><td> 46.62</td><td>2.84</td><td>2.90</td><td>2.56</td><td>⋯</td><td>0.99</td><td>0.78</td><td>0.91</td><td>0.95</td><td>0.77</td><td>0.93</td><td>0.60</td><td>0.27</td><td>0.47</td><td>0.26</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-09</td><td>Received Usage kWh </td><td> 0.00</td><td>2.18</td><td> 43.45</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-08</td><td>kWh Net            </td><td>-1.92</td><td>3.02</td><td> -9.01</td><td>0.48</td><td>0.19</td><td>0.42</td><td>⋯</td><td>2.38</td><td>2.97</td><td>2.74</td><td>2.79</td><td>2.94</td><td>2.72</td><td>3.02</td><td>2.75</td><td>2.93</td><td>2.92</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-08</td><td>Delivered Usage kWh</td><td> 0.00</td><td>3.02</td><td> 39.58</td><td>0.48</td><td>0.19</td><td>0.42</td><td>⋯</td><td>2.38</td><td>2.97</td><td>2.74</td><td>2.79</td><td>2.94</td><td>2.72</td><td>3.02</td><td>2.75</td><td>2.93</td><td>2.92</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-08</td><td>Received Usage kWh </td><td> 0.00</td><td>1.94</td><td> 48.59</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-07</td><td>kWh Net            </td><td>-1.92</td><td>0.73</td><td>  4.63</td><td>0.55</td><td>0.18</td><td>0.49</td><td>⋯</td><td>0.40</td><td>0.57</td><td>0.23</td><td>0.46</td><td>0.21</td><td>0.49</td><td>0.22</td><td>0.18</td><td>0.39</td><td>0.25</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-07</td><td>Delivered Usage kWh</td><td> 0.01</td><td>0.73</td><td> 27.28</td><td>0.55</td><td>0.18</td><td>0.49</td><td>⋯</td><td>0.40</td><td>0.57</td><td>0.23</td><td>0.46</td><td>0.21</td><td>0.49</td><td>0.22</td><td>0.18</td><td>0.39</td><td>0.25</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-07</td><td>Received Usage kWh </td><td> 0.00</td><td>1.92</td><td> 22.65</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-06</td><td>kWh Net            </td><td>-1.76</td><td>1.24</td><td>-21.32</td><td>0.20</td><td>0.31</td><td>0.07</td><td>⋯</td><td>1.05</td><td>0.30</td><td>0.54</td><td>0.33</td><td>0.44</td><td>0.55</td><td>0.23</td><td>0.55</td><td>0.41</td><td>0.24</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-06</td><td>Delivered Usage kWh</td><td> 0.00</td><td>1.24</td><td> 20.79</td><td>0.20</td><td>0.31</td><td>0.07</td><td>⋯</td><td>1.05</td><td>0.30</td><td>0.54</td><td>0.33</td><td>0.44</td><td>0.55</td><td>0.23</td><td>0.55</td><td>0.41</td><td>0.24</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-06</td><td>Received Usage kWh </td><td> 0.00</td><td>1.76</td><td> 42.12</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-05</td><td>kWh Net            </td><td>-2.00</td><td>0.92</td><td>-25.74</td><td>0.28</td><td>0.30</td><td>0.09</td><td>⋯</td><td>0.20</td><td>0.58</td><td>0.20</td><td>0.38</td><td>0.28</td><td>0.33</td><td>0.24</td><td>0.33</td><td>0.22</td><td>0.31</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-05</td><td>Delivered Usage kWh</td><td> 0.00</td><td>0.92</td><td> 16.85</td><td>0.28</td><td>0.30</td><td>0.09</td><td>⋯</td><td>0.20</td><td>0.58</td><td>0.20</td><td>0.38</td><td>0.28</td><td>0.33</td><td>0.24</td><td>0.33</td><td>0.22</td><td>0.31</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-05</td><td>Received Usage kWh </td><td> 0.00</td><td>2.00</td><td> 42.58</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-04</td><td>kWh Net            </td><td>-2.02</td><td>3.29</td><td> 11.70</td><td>0.16</td><td>0.11</td><td>0.30</td><td>⋯</td><td>2.74</td><td>2.56</td><td>0.82</td><td>0.35</td><td>0.11</td><td>0.54</td><td>0.19</td><td>0.11</td><td>0.32</td><td>0.24</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-04</td><td>Delivered Usage kWh</td><td> 0.00</td><td>3.29</td><td> 54.87</td><td>0.16</td><td>0.11</td><td>0.30</td><td>⋯</td><td>2.74</td><td>2.56</td><td>0.82</td><td>0.35</td><td>0.11</td><td>0.54</td><td>0.19</td><td>0.11</td><td>0.32</td><td>0.24</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-04</td><td>Received Usage kWh </td><td> 0.00</td><td>2.02</td><td> 43.17</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-03</td><td>kWh Net            </td><td>-1.88</td><td>0.75</td><td>-36.53</td><td>0.15</td><td>0.14</td><td>0.10</td><td>⋯</td><td>0.10</td><td>0.54</td><td>0.09</td><td>0.15</td><td>0.24</td><td>0.21</td><td>0.38</td><td>0.08</td><td>0.12</td><td>0.30</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-03</td><td>Delivered Usage kWh</td><td> 0.00</td><td>0.75</td><td> 11.23</td><td>0.15</td><td>0.14</td><td>0.10</td><td>⋯</td><td>0.10</td><td>0.54</td><td>0.09</td><td>0.15</td><td>0.24</td><td>0.21</td><td>0.38</td><td>0.08</td><td>0.12</td><td>0.30</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-03</td><td>Received Usage kWh </td><td> 0.00</td><td>1.88</td><td> 47.76</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-02</td><td>kWh Net            </td><td>-2.29</td><td>2.86</td><td>-27.02</td><td>0.14</td><td>0.08</td><td>0.06</td><td>⋯</td><td>2.65</td><td>2.74</td><td>2.57</td><td>2.56</td><td>2.54</td><td>0.13</td><td>0.11</td><td>0.13</td><td>0.16</td><td>0.16</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-02</td><td>Delivered Usage kWh</td><td> 0.00</td><td>2.86</td><td> 37.40</td><td>0.14</td><td>0.08</td><td>0.06</td><td>⋯</td><td>2.65</td><td>2.74</td><td>2.57</td><td>2.56</td><td>2.54</td><td>0.13</td><td>0.11</td><td>0.13</td><td>0.16</td><td>0.16</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-02</td><td>Received Usage kWh </td><td> 0.00</td><td>2.29</td><td> 64.42</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net            </td><td>-2.40</td><td>0.65</td><td>-62.45</td><td>0.11</td><td>0.09</td><td>0.09</td><td>⋯</td><td>0.31</td><td>0.22</td><td>0.23</td><td>0.15</td><td>0.11</td><td>0.11</td><td>0.11</td><td>0.12</td><td>0.11</td><td>0.13</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>Delivered Usage kWh</td><td> 0.00</td><td>0.65</td><td>  7.80</td><td>0.11</td><td>0.09</td><td>0.09</td><td>⋯</td><td>0.31</td><td>0.22</td><td>0.23</td><td>0.15</td><td>0.11</td><td>0.11</td><td>0.11</td><td>0.12</td><td>0.11</td><td>0.13</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>Received Usage kWh </td><td> 0.00</td><td>2.40</td><td> 70.25</td><td>0.00</td><td>0.00</td><td>0.00</td><td>⋯</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td><td>0.00</td></tr>
	<tr><td>NULL</td><td>       NA</td><td>NA</td><td>NA                 </td><td>   NA</td><td>  NA</td><td>    NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>⋯</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td></tr>
	<tr><td>NULL</td><td>       NA</td><td>NA</td><td>NA                 </td><td>   NA</td><td>  NA</td><td>    NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>⋯</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td></tr>
	<tr><td>The information contained in this file is intended for the confidential use by the customer and third parties authorized by the customer to receive the information. Any unauthorized use is prohibited.</td><td>       NA</td><td>NA</td><td>NA                 </td><td>   NA</td><td>  NA</td><td>    NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>⋯</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td><td>  NA</td></tr>
</tbody>
</table>




```R
hourly_ppl_pivot <- ppl_15mins %>% 
  rename(date = Date) %>% 
  pivot_longer(!c("Account Number", "Meter Number", date, "Read Type", Min, Max, Total), names_to = "time", values_to = "kWh") 

#rm(hourly_pivot)

hourly_ppl_pivot <- hourly_ppl_pivot %>% 
  mutate(time = parse_time(time, '%H:%M %p'), month = month(date, label=TRUE), year = year(date), yday = yday(date), wday = wday(date, label=TRUE))

(hourly_ppl_net <- hourly_ppl_pivot %>% 
  filter(`Read Type` == "kWh Net"))
```


<table class="dataframe">
<caption>A tibble: 1440 × 13</caption>
<thead>
	<tr><th scope=col>Account Number</th><th scope=col>Meter Number</th><th scope=col>date</th><th scope=col>Read Type</th><th scope=col>Min</th><th scope=col>Max</th><th scope=col>Total</th><th scope=col>time</th><th scope=col>kWh</th><th scope=col>month</th><th scope=col>year</th><th scope=col>yday</th><th scope=col>wday</th></tr>
	<tr><th scope=col>&lt;list&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;time&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;ord&gt;</th></tr>
</thead>
<tbody>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>00:00:00</td><td> 2.66</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>00:15:00</td><td> 2.66</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>00:30:00</td><td> 2.56</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>00:45:00</td><td> 2.54</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>01:00:00</td><td> 2.56</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>01:15:00</td><td> 1.18</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>01:30:00</td><td> 0.09</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>01:45:00</td><td> 0.10</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>02:00:00</td><td> 0.09</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>02:15:00</td><td> 0.12</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>02:30:00</td><td> 0.13</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>02:45:00</td><td> 0.12</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>03:00:00</td><td> 0.12</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>03:15:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>03:30:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>03:45:00</td><td> 0.10</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>04:00:00</td><td> 0.08</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>04:15:00</td><td> 0.10</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>04:30:00</td><td> 0.09</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>04:45:00</td><td> 0.09</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>05:00:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>05:15:00</td><td> 0.09</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>05:30:00</td><td> 0.10</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>05:45:00</td><td> 0.12</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>06:00:00</td><td> 0.10</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>06:15:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>06:30:00</td><td> 0.10</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>06:45:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>07:00:00</td><td>-0.04</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-15</td><td>kWh Net</td><td>-2.17</td><td>2.66</td><td>-49.57</td><td>07:15:00</td><td>-0.46</td><td>Sep</td><td>2023</td><td>258</td><td>Fri</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>16:30:00</td><td>-1.41</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>16:45:00</td><td>-1.26</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>17:00:00</td><td>-0.99</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>17:15:00</td><td>-0.91</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>17:30:00</td><td>-0.67</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>17:45:00</td><td>-0.43</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>18:00:00</td><td>-0.37</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>18:15:00</td><td>-0.33</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>18:30:00</td><td>-0.19</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>18:45:00</td><td>-0.06</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>19:00:00</td><td> 0.34</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>19:15:00</td><td> 0.48</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>19:30:00</td><td> 0.65</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>19:45:00</td><td> 0.50</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>20:00:00</td><td> 0.41</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>20:15:00</td><td> 0.31</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>20:30:00</td><td> 0.31</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>20:45:00</td><td> 0.27</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>21:00:00</td><td> 0.20</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>21:15:00</td><td> 0.27</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>21:30:00</td><td> 0.31</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>21:45:00</td><td> 0.22</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>22:00:00</td><td> 0.23</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>22:15:00</td><td> 0.15</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>22:30:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>22:45:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>23:00:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>23:15:00</td><td> 0.12</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>23:30:00</td><td> 0.11</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
	<tr><td>8892170024</td><td>300676415</td><td>2023-09-01</td><td>kWh Net</td><td>-2.4</td><td>0.65</td><td>-62.45</td><td>23:45:00</td><td> 0.13</td><td>Sep</td><td>2023</td><td>244</td><td>Fri</td></tr>
</tbody>
</table>




```R

solar_import <- read_sheet("https://docs.google.com/spreadsheets/d/1Hp63wXcI_eh3I_KhpQws6LvfHKKaORv-b6eC6Fsmijs/edit?usp=sharing")

(hourly_production <- solar_import %>% 
  rename(datetime = `Date/Time`, energy_produced_Wh = `Energy Produced (Wh)`) %>% 
  mutate(datetime = as.POSIXct(datetime,format="%m/%d/%Y %H:%M",tz=Sys.timezone())) %>% 
  mutate(date = date(datetime), time = as_hms(format(datetime, format = "%H:%M:%S")), month = month(datetime, label=TRUE), year = year(datetime), day = day(datetime), yday = yday(datetime), monthday = format(datetime, "%m-%d"), wday = wday(datetime, label=TRUE), equinox_day = (yday + 10) %% 365, equinox_group = floor((equinox_day+15)/30)*30)
)

```

    [32m✔[39m Reading from [36m2801736_custom_report_15mins_230901_230915[39m.
    
    [32m✔[39m Range [33m2801736_custom_report_15mins_230901_230915[39m.
    



<table class="dataframe">
<caption>A tibble: 1440 × 12</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>energy_produced_Wh</th><th scope=col>date</th><th scope=col>time</th><th scope=col>month</th><th scope=col>year</th><th scope=col>day</th><th scope=col>yday</th><th scope=col>monthday</th><th scope=col>wday</th><th scope=col>equinox_day</th><th scope=col>equinox_group</th></tr>
	<tr><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;time&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;ord&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2023-09-01 00:00:00</td><td>  0</td><td>2023-09-01</td><td>00:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 00:15:00</td><td>  0</td><td>2023-09-01</td><td>00:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 00:30:00</td><td>  0</td><td>2023-09-01</td><td>00:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 00:45:00</td><td>  0</td><td>2023-09-01</td><td>00:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 01:00:00</td><td>  0</td><td>2023-09-01</td><td>01:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 01:15:00</td><td>  0</td><td>2023-09-01</td><td>01:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 01:30:00</td><td>  0</td><td>2023-09-01</td><td>01:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 01:45:00</td><td>  0</td><td>2023-09-01</td><td>01:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 02:00:00</td><td>  0</td><td>2023-09-01</td><td>02:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 02:15:00</td><td>  0</td><td>2023-09-01</td><td>02:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 02:30:00</td><td>  0</td><td>2023-09-01</td><td>02:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 02:45:00</td><td>  0</td><td>2023-09-01</td><td>02:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 03:00:00</td><td>  0</td><td>2023-09-01</td><td>03:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 03:15:00</td><td>  0</td><td>2023-09-01</td><td>03:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 03:30:00</td><td>  0</td><td>2023-09-01</td><td>03:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 03:45:00</td><td>  0</td><td>2023-09-01</td><td>03:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 04:00:00</td><td>  0</td><td>2023-09-01</td><td>04:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 04:15:00</td><td>  0</td><td>2023-09-01</td><td>04:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 04:30:00</td><td>  0</td><td>2023-09-01</td><td>04:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 04:45:00</td><td>  0</td><td>2023-09-01</td><td>04:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 05:00:00</td><td>  0</td><td>2023-09-01</td><td>05:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 05:15:00</td><td>  0</td><td>2023-09-01</td><td>05:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 05:30:00</td><td>  0</td><td>2023-09-01</td><td>05:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 05:45:00</td><td>  0</td><td>2023-09-01</td><td>05:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 06:00:00</td><td>  0</td><td>2023-09-01</td><td>06:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 06:15:00</td><td>  2</td><td>2023-09-01</td><td>06:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 06:30:00</td><td> 64</td><td>2023-09-01</td><td>06:30:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 06:45:00</td><td>290</td><td>2023-09-01</td><td>06:45:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 07:00:00</td><td>497</td><td>2023-09-01</td><td>07:00:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>2023-09-01 07:15:00</td><td>643</td><td>2023-09-01</td><td>07:15:00</td><td>Sep</td><td>2023</td><td>1</td><td>244</td><td>09-01</td><td>Fri</td><td>254</td><td>240</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2023-09-15 16:30:00</td><td>1582</td><td>2023-09-15</td><td>16:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 16:45:00</td><td>1520</td><td>2023-09-15</td><td>16:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 17:00:00</td><td>1449</td><td>2023-09-15</td><td>17:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 17:15:00</td><td>1362</td><td>2023-09-15</td><td>17:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 17:30:00</td><td>1256</td><td>2023-09-15</td><td>17:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 17:45:00</td><td>1141</td><td>2023-09-15</td><td>17:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 18:00:00</td><td> 990</td><td>2023-09-15</td><td>18:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 18:15:00</td><td> 822</td><td>2023-09-15</td><td>18:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 18:30:00</td><td> 599</td><td>2023-09-15</td><td>18:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 18:45:00</td><td> 267</td><td>2023-09-15</td><td>18:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 19:00:00</td><td>  17</td><td>2023-09-15</td><td>19:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 19:15:00</td><td>   0</td><td>2023-09-15</td><td>19:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 19:30:00</td><td>   0</td><td>2023-09-15</td><td>19:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 19:45:00</td><td>   0</td><td>2023-09-15</td><td>19:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 20:00:00</td><td>   0</td><td>2023-09-15</td><td>20:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 20:15:00</td><td>   0</td><td>2023-09-15</td><td>20:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 20:30:00</td><td>   0</td><td>2023-09-15</td><td>20:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 20:45:00</td><td>   0</td><td>2023-09-15</td><td>20:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 21:00:00</td><td>   0</td><td>2023-09-15</td><td>21:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 21:15:00</td><td>   0</td><td>2023-09-15</td><td>21:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 21:30:00</td><td>   0</td><td>2023-09-15</td><td>21:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 21:45:00</td><td>   0</td><td>2023-09-15</td><td>21:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 22:00:00</td><td>   0</td><td>2023-09-15</td><td>22:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 22:15:00</td><td>   0</td><td>2023-09-15</td><td>22:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 22:30:00</td><td>   0</td><td>2023-09-15</td><td>22:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 22:45:00</td><td>   0</td><td>2023-09-15</td><td>22:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 23:00:00</td><td>   0</td><td>2023-09-15</td><td>23:00:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 23:15:00</td><td>   0</td><td>2023-09-15</td><td>23:15:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 23:30:00</td><td>   0</td><td>2023-09-15</td><td>23:30:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
	<tr><td>2023-09-15 23:45:00</td><td>   0</td><td>2023-09-15</td><td>23:45:00</td><td>Sep</td><td>2023</td><td>15</td><td>258</td><td>09-15</td><td>Fri</td><td>268</td><td>270</td></tr>
</tbody>
</table>




```R
ggplot(hourly_production, aes(datetime, energy_produced_Wh)) +
  geom_point()

ggplot(hourly_production, aes(time, energy_produced_Wh)) +
  theme(axis.text.x = element_text(angle = 90)) +
  geom_point()
```


    
![png](output_5_0.png)
    



    
![png](output_5_1.png)
    



```R
ggplot(hourly_production, aes(datetime, energy_produced_Wh)) +
  geom_point()

ggplot(hourly_production, aes(time, energy_produced_Wh)) +
  theme(axis.text.x = element_text(angle = 90)) +
  geom_point()
```

Net + Produced = Consumed


```R
(hourly_electricity <- hourly_ppl_net %>% 
    inner_join(hourly_production, by = join_by(date,time))  %>% 
    mutate(consumed_kWh = kWh + energy_produced_Wh/1000, produced_kWh = energy_produced_Wh/1000)  %>% 
    rename(net_kWh = kWh) %>% 
    select(datetime, date, time, net_kWh, produced_kWh, consumed_kWh))

```


<table class="dataframe">
<caption>A tibble: 1440 × 6</caption>
<thead>
	<tr><th scope=col>datetime</th><th scope=col>date</th><th scope=col>time</th><th scope=col>net_kWh</th><th scope=col>produced_kWh</th><th scope=col>consumed_kWh</th></tr>
	<tr><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;dttm&gt;</th><th scope=col>&lt;time&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2023-09-15 00:00:00</td><td>2023-09-15</td><td>00:00:00</td><td> 2.66</td><td>0.000</td><td>2.660</td></tr>
	<tr><td>2023-09-15 00:15:00</td><td>2023-09-15</td><td>00:15:00</td><td> 2.66</td><td>0.000</td><td>2.660</td></tr>
	<tr><td>2023-09-15 00:30:00</td><td>2023-09-15</td><td>00:30:00</td><td> 2.56</td><td>0.000</td><td>2.560</td></tr>
	<tr><td>2023-09-15 00:45:00</td><td>2023-09-15</td><td>00:45:00</td><td> 2.54</td><td>0.000</td><td>2.540</td></tr>
	<tr><td>2023-09-15 01:00:00</td><td>2023-09-15</td><td>01:00:00</td><td> 2.56</td><td>0.000</td><td>2.560</td></tr>
	<tr><td>2023-09-15 01:15:00</td><td>2023-09-15</td><td>01:15:00</td><td> 1.18</td><td>0.000</td><td>1.180</td></tr>
	<tr><td>2023-09-15 01:30:00</td><td>2023-09-15</td><td>01:30:00</td><td> 0.09</td><td>0.000</td><td>0.090</td></tr>
	<tr><td>2023-09-15 01:45:00</td><td>2023-09-15</td><td>01:45:00</td><td> 0.10</td><td>0.000</td><td>0.100</td></tr>
	<tr><td>2023-09-15 02:00:00</td><td>2023-09-15</td><td>02:00:00</td><td> 0.09</td><td>0.000</td><td>0.090</td></tr>
	<tr><td>2023-09-15 02:15:00</td><td>2023-09-15</td><td>02:15:00</td><td> 0.12</td><td>0.000</td><td>0.120</td></tr>
	<tr><td>2023-09-15 02:30:00</td><td>2023-09-15</td><td>02:30:00</td><td> 0.13</td><td>0.000</td><td>0.130</td></tr>
	<tr><td>2023-09-15 02:45:00</td><td>2023-09-15</td><td>02:45:00</td><td> 0.12</td><td>0.000</td><td>0.120</td></tr>
	<tr><td>2023-09-15 03:00:00</td><td>2023-09-15</td><td>03:00:00</td><td> 0.12</td><td>0.000</td><td>0.120</td></tr>
	<tr><td>2023-09-15 03:15:00</td><td>2023-09-15</td><td>03:15:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-15 03:30:00</td><td>2023-09-15</td><td>03:30:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-15 03:45:00</td><td>2023-09-15</td><td>03:45:00</td><td> 0.10</td><td>0.000</td><td>0.100</td></tr>
	<tr><td>2023-09-15 04:00:00</td><td>2023-09-15</td><td>04:00:00</td><td> 0.08</td><td>0.000</td><td>0.080</td></tr>
	<tr><td>2023-09-15 04:15:00</td><td>2023-09-15</td><td>04:15:00</td><td> 0.10</td><td>0.000</td><td>0.100</td></tr>
	<tr><td>2023-09-15 04:30:00</td><td>2023-09-15</td><td>04:30:00</td><td> 0.09</td><td>0.000</td><td>0.090</td></tr>
	<tr><td>2023-09-15 04:45:00</td><td>2023-09-15</td><td>04:45:00</td><td> 0.09</td><td>0.000</td><td>0.090</td></tr>
	<tr><td>2023-09-15 05:00:00</td><td>2023-09-15</td><td>05:00:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-15 05:15:00</td><td>2023-09-15</td><td>05:15:00</td><td> 0.09</td><td>0.000</td><td>0.090</td></tr>
	<tr><td>2023-09-15 05:30:00</td><td>2023-09-15</td><td>05:30:00</td><td> 0.10</td><td>0.000</td><td>0.100</td></tr>
	<tr><td>2023-09-15 05:45:00</td><td>2023-09-15</td><td>05:45:00</td><td> 0.12</td><td>0.000</td><td>0.120</td></tr>
	<tr><td>2023-09-15 06:00:00</td><td>2023-09-15</td><td>06:00:00</td><td> 0.10</td><td>0.000</td><td>0.100</td></tr>
	<tr><td>2023-09-15 06:15:00</td><td>2023-09-15</td><td>06:15:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-15 06:30:00</td><td>2023-09-15</td><td>06:30:00</td><td> 0.10</td><td>0.000</td><td>0.100</td></tr>
	<tr><td>2023-09-15 06:45:00</td><td>2023-09-15</td><td>06:45:00</td><td> 0.11</td><td>0.009</td><td>0.119</td></tr>
	<tr><td>2023-09-15 07:00:00</td><td>2023-09-15</td><td>07:00:00</td><td>-0.04</td><td>0.212</td><td>0.172</td></tr>
	<tr><td>2023-09-15 07:15:00</td><td>2023-09-15</td><td>07:15:00</td><td>-0.46</td><td>0.596</td><td>0.136</td></tr>
	<tr><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td><td>⋮</td></tr>
	<tr><td>2023-09-01 16:30:00</td><td>2023-09-01</td><td>16:30:00</td><td>-1.41</td><td>1.622</td><td>0.212</td></tr>
	<tr><td>2023-09-01 16:45:00</td><td>2023-09-01</td><td>16:45:00</td><td>-1.26</td><td>1.561</td><td>0.301</td></tr>
	<tr><td>2023-09-01 17:00:00</td><td>2023-09-01</td><td>17:00:00</td><td>-0.99</td><td>1.493</td><td>0.503</td></tr>
	<tr><td>2023-09-01 17:15:00</td><td>2023-09-01</td><td>17:15:00</td><td>-0.91</td><td>1.389</td><td>0.479</td></tr>
	<tr><td>2023-09-01 17:30:00</td><td>2023-09-01</td><td>17:30:00</td><td>-0.67</td><td>1.299</td><td>0.629</td></tr>
	<tr><td>2023-09-01 17:45:00</td><td>2023-09-01</td><td>17:45:00</td><td>-0.43</td><td>1.192</td><td>0.762</td></tr>
	<tr><td>2023-09-01 18:00:00</td><td>2023-09-01</td><td>18:00:00</td><td>-0.37</td><td>1.002</td><td>0.632</td></tr>
	<tr><td>2023-09-01 18:15:00</td><td>2023-09-01</td><td>18:15:00</td><td>-0.33</td><td>0.904</td><td>0.574</td></tr>
	<tr><td>2023-09-01 18:30:00</td><td>2023-09-01</td><td>18:30:00</td><td>-0.19</td><td>0.765</td><td>0.575</td></tr>
	<tr><td>2023-09-01 18:45:00</td><td>2023-09-01</td><td>18:45:00</td><td>-0.06</td><td>0.568</td><td>0.508</td></tr>
	<tr><td>2023-09-01 19:00:00</td><td>2023-09-01</td><td>19:00:00</td><td> 0.34</td><td>0.345</td><td>0.685</td></tr>
	<tr><td>2023-09-01 19:15:00</td><td>2023-09-01</td><td>19:15:00</td><td> 0.48</td><td>0.077</td><td>0.557</td></tr>
	<tr><td>2023-09-01 19:30:00</td><td>2023-09-01</td><td>19:30:00</td><td> 0.65</td><td>0.005</td><td>0.655</td></tr>
	<tr><td>2023-09-01 19:45:00</td><td>2023-09-01</td><td>19:45:00</td><td> 0.50</td><td>0.000</td><td>0.500</td></tr>
	<tr><td>2023-09-01 20:00:00</td><td>2023-09-01</td><td>20:00:00</td><td> 0.41</td><td>0.000</td><td>0.410</td></tr>
	<tr><td>2023-09-01 20:15:00</td><td>2023-09-01</td><td>20:15:00</td><td> 0.31</td><td>0.000</td><td>0.310</td></tr>
	<tr><td>2023-09-01 20:30:00</td><td>2023-09-01</td><td>20:30:00</td><td> 0.31</td><td>0.000</td><td>0.310</td></tr>
	<tr><td>2023-09-01 20:45:00</td><td>2023-09-01</td><td>20:45:00</td><td> 0.27</td><td>0.000</td><td>0.270</td></tr>
	<tr><td>2023-09-01 21:00:00</td><td>2023-09-01</td><td>21:00:00</td><td> 0.20</td><td>0.000</td><td>0.200</td></tr>
	<tr><td>2023-09-01 21:15:00</td><td>2023-09-01</td><td>21:15:00</td><td> 0.27</td><td>0.000</td><td>0.270</td></tr>
	<tr><td>2023-09-01 21:30:00</td><td>2023-09-01</td><td>21:30:00</td><td> 0.31</td><td>0.000</td><td>0.310</td></tr>
	<tr><td>2023-09-01 21:45:00</td><td>2023-09-01</td><td>21:45:00</td><td> 0.22</td><td>0.000</td><td>0.220</td></tr>
	<tr><td>2023-09-01 22:00:00</td><td>2023-09-01</td><td>22:00:00</td><td> 0.23</td><td>0.000</td><td>0.230</td></tr>
	<tr><td>2023-09-01 22:15:00</td><td>2023-09-01</td><td>22:15:00</td><td> 0.15</td><td>0.000</td><td>0.150</td></tr>
	<tr><td>2023-09-01 22:30:00</td><td>2023-09-01</td><td>22:30:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-01 22:45:00</td><td>2023-09-01</td><td>22:45:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-01 23:00:00</td><td>2023-09-01</td><td>23:00:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-01 23:15:00</td><td>2023-09-01</td><td>23:15:00</td><td> 0.12</td><td>0.000</td><td>0.120</td></tr>
	<tr><td>2023-09-01 23:30:00</td><td>2023-09-01</td><td>23:30:00</td><td> 0.11</td><td>0.000</td><td>0.110</td></tr>
	<tr><td>2023-09-01 23:45:00</td><td>2023-09-01</td><td>23:45:00</td><td> 0.13</td><td>0.000</td><td>0.130</td></tr>
</tbody>
</table>




```R
ggplot(hourly_electricity, aes(x=time)) +
  geom_point(aes(y=consumed_kWh,color="red")) 

ggplot(hourly_electricity, aes(x=datetime)) +
  geom_point(aes(y=consumed_kWh,color="red")) 

```


    
![png](output_9_0.png)
    



    
![png](output_9_1.png)
    



```R

```


```R

```
