# Mauna Loa carbon dioxide data analysis
## Problem statement 
In 1958, Charles David Keeling (1928-2005) from the Scripps Institution of
Oceanography began recording $CO_2$ concentrations in the atmosphere at an
observatory located at about 3,400 m altitude on the Mauna Loa Volcano
on Hawaii Island.

The location was chosen because it is not influenced by changing CO2
levels due to the local vegetation and because prevailing wind patterns on this
tropical island tend to bring well-mixed air to the site. While the recordings
are made near a volcano (which tends to produce $CO_2$), wind patterns tend
to blow the volcanic $CO_2$ away from the recording site.

Air samples are taken several times a day, and concentrations have been
observed using the same measuring method for over 60 years. In addition,
samples are stored in flasks and periodically reanalyzed for calibration purposes.
The result is a data set with very few interruptions and very few
inhomogeneities.

Let $C_i$ be the average $CO_2$ concentration in month $i$ ($i = 1, 2, ...$, counting
from March 1958). We are looking for a description of the form:
$$C_i = F(t_i) + P_i + R_i$$
where
- $F : t → F(t)$ accounts for the long-term trend;
- $t_i$ is time at the middle of the $i^{th}$ month, measured in fractions of
years after Jan 15, 1958. Specifically, we take $t_i = (i+0.5)/12$ where $i = 0$
corresponds to Jan, 1958, adding $0.5$ is because the first measurement
is halfway through the first month;
- $P_i$ is periodic in i with a fixed period, accounting for the seasonal
pattern;
- $R_i$ is the remaining residual that accounts for all other influences

## Results 
Here is a brief summary, for more results as well as methodology check the [report](https://github.com/zhenyatos/CO2-concentration-forecast/blob/main/report/report.pdf)

After we tried several polynomial trends, the quadratic trend was picked as having the best bias-variance tradeoff, measured using MAPE (Mean Absolute Percentage Error) on train and test datasets. 
Then, after removing the trend, the periodic component was derived using the averaging all values corresponding to the same month. The resulting model has MAPE of 0.21%. 

![co2_prediction](https://github.com/zhenyatos/CO2-concentration-forecast/assets/47058532/94c76510-7806-4376-aa4f-814b5b4235a5)

However, when we performed some statistical tests for measuring the stationarity of the residual $R_i$ component such as computing the ACF (autocorrelation function) and 
performing the [Augmented Dickey-Fuller test](https://en.wikipedia.org/wiki/Augmented_Dickey%E2%80%93Fuller_test), it was apparent that there could still be some external regressor which we weren't 
able to account for. This external regressor might be tied to the global warming, but further investigation is required to confirm this hypothesis. 

![output](https://github.com/zhenyatos/CO2-concentration-forecast/assets/47058532/121c018e-9b54-4af8-8008-1598e0a1cd00)

## References 
[Data](https://gml.noaa.gov/ccgg/trends/data.html) by *Dr. Pieter Tans, NOAA/GML (gml.noaa.gov/ccgg/trends/) and Dr. Ralph Keeling, Scripps Institution of Oceanography (scrippsco2.ucsd.edu/).* 
