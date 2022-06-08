# 决策树及其组合方法

+ Decision Tree: A decision tree is a decision support tool that uses a tree-like graph or model of decisions and their possible consequences, including chance event outcomes, resource costs, and utility. 

+ Decision trees are produced by algorithms that identify various ways of splitting a data set into branch-like segments. These segments form an inverted decision tree that originates with a root node at the top of the tree. 

## 决策树


## 实例


```r
#####
# importing churn data set from .csv file
data1 <-read.csv("./data/chun_data.csv",header= TRUE,sep=",")

#as the first three columns of the data arent important for the prediction we drop those columns from the data frame 
# drop the variables from the data set
data1<-data1[-c(1,2,3)]

#data exploration

#gives the dimensions of the data set
dim(data1)
```

```
## [1] 5000   17
```

```r
#gives the names of the variables in the data set
names(data1)
```

```
##  [1] "international_plan"            "voice_mail_plan"              
##  [3] "number_vmail_messages"         "total_day_minutes"            
##  [5] "total_day_calls"               "total_day_charge"             
##  [7] "total_eve_minutes"             "total_eve_calls"              
##  [9] "total_eve_charge"              "total_night_minutes"          
## [11] "total_night_calls"             "total_night_charge"           
## [13] "total_intl_minutes"            "total_intl_calls"             
## [15] "total_intl_charge"             "number_customer_service_calls"
## [17] "churn"
```

```r
#displays top 10 observations of the data set
head(data1)
```

```
##   international_plan voice_mail_plan number_vmail_messages total_day_minutes
## 1                 no             yes                    25             265.1
## 2                 no             yes                    26             161.6
## 3                 no              no                     0             243.4
## 4                yes              no                     0             299.4
## 5                yes              no                     0             166.7
## 6                yes              no                     0             223.4
##   total_day_calls total_day_charge total_eve_minutes total_eve_calls
## 1             110            45.07             197.4              99
## 2             123            27.47             195.5             103
## 3             114            41.38             121.2             110
## 4              71            50.90              61.9              88
## 5             113            28.34             148.3             122
## 6              98            37.98             220.6             101
##   total_eve_charge total_night_minutes total_night_calls total_night_charge
## 1            16.78               244.7                91              11.01
## 2            16.62               254.4               103              11.45
## 3            10.30               162.6               104               7.32
## 4             5.26               196.9                89               8.86
## 5            12.61               186.9               121               8.41
## 6            18.75               203.9               118               9.18
##   total_intl_minutes total_intl_calls total_intl_charge
## 1               10.0                3              2.70
## 2               13.7                3              3.70
## 3               12.2                5              3.29
## 4                6.6                7              1.78
## 5               10.1                3              2.73
## 6                6.3                6              1.70
##   number_customer_service_calls churn
## 1                             1    no
## 2                             1    no
## 3                             0    no
## 4                             2    no
## 5                             3    no
## 6                             0    no
```

```r
#displays bottom top 10 observations of the data set
tail(data1)
```

```
##      international_plan voice_mail_plan number_vmail_messages total_day_minutes
## 4995                 no              no                     0             170.7
## 4996                 no             yes                    40             235.7
## 4997                 no              no                     0             184.2
## 4998                 no              no                     0             140.6
## 4999                 no              no                     0             188.8
## 5000                 no             yes                    34             129.4
##      total_day_calls total_day_charge total_eve_minutes total_eve_calls
## 4995             101            29.02             193.1             126
## 4996             127            40.07             223.0             126
## 4997              90            31.31             256.8              73
## 4998              89            23.90             172.8             128
## 4999              67            32.10             171.7              92
## 5000             102            22.00             267.1             104
##      total_eve_charge total_night_minutes total_night_calls total_night_charge
## 4995            16.41               129.1               104               5.81
## 4996            18.96               297.5               116              13.39
## 4997            21.83               213.6               113               9.61
## 4998            14.69               212.4                97               9.56
## 4999            14.59               224.4                89              10.10
## 5000            22.70               154.8               100               6.97
##      total_intl_minutes total_intl_calls total_intl_charge
## 4995                6.9                7              1.86
## 4996                9.9                5              2.67
## 4997               14.7                2              3.97
## 4998               13.6                4              3.67
## 4999                8.5                6              2.30
## 5000                9.3               16              2.51
##      number_customer_service_calls churn
## 4995                             1    no
## 4996                             2    no
## 4997                             3   yes
## 4998                             1    no
## 4999                             0    no
## 5000                             0    no
```

```r
#displays structure of the variables in the data set
str(data1)
```

```
## 'data.frame':	5000 obs. of  17 variables:
##  $ international_plan           : chr  "no" "no" "no" "yes" ...
##  $ voice_mail_plan              : chr  "yes" "yes" "no" "no" ...
##  $ number_vmail_messages        : int  25 26 0 0 0 0 24 0 0 37 ...
##  $ total_day_minutes            : num  265 162 243 299 167 ...
##  $ total_day_calls              : int  110 123 114 71 113 98 88 79 97 84 ...
##  $ total_day_charge             : num  45.1 27.5 41.4 50.9 28.3 ...
##  $ total_eve_minutes            : num  197.4 195.5 121.2 61.9 148.3 ...
##  $ total_eve_calls              : int  99 103 110 88 122 101 108 94 80 111 ...
##  $ total_eve_charge             : num  16.78 16.62 10.3 5.26 12.61 ...
##  $ total_night_minutes          : num  245 254 163 197 187 ...
##  $ total_night_calls            : int  91 103 104 89 121 118 118 96 90 97 ...
##  $ total_night_charge           : num  11.01 11.45 7.32 8.86 8.41 ...
##  $ total_intl_minutes           : num  10 13.7 12.2 6.6 10.1 6.3 7.5 7.1 8.7 11.2 ...
##  $ total_intl_calls             : int  3 3 5 7 3 6 7 6 4 5 ...
##  $ total_intl_charge            : num  2.7 3.7 3.29 1.78 2.73 1.7 2.03 1.92 2.35 3.02 ...
##  $ number_customer_service_calls: int  1 1 0 2 3 0 3 0 1 0 ...
##  $ churn                        : chr  "no" "no" "no" "no" ...
```

```r
#gives the summary of the variables of the data set 
summary(data1)
```

```
##  international_plan voice_mail_plan    number_vmail_messages total_day_minutes
##  Length:5000        Length:5000        Min.   : 0.000        Min.   :  0.0    
##  Class :character   Class :character   1st Qu.: 0.000        1st Qu.:143.7    
##  Mode  :character   Mode  :character   Median : 0.000        Median :180.1    
##                                        Mean   : 7.755        Mean   :180.3    
##                                        3rd Qu.:17.000        3rd Qu.:216.2    
##                                        Max.   :52.000        Max.   :351.5    
##  total_day_calls total_day_charge total_eve_minutes total_eve_calls
##  Min.   :  0     Min.   : 0.00    Min.   :  0.0     Min.   :  0.0  
##  1st Qu.: 87     1st Qu.:24.43    1st Qu.:166.4     1st Qu.: 87.0  
##  Median :100     Median :30.62    Median :201.0     Median :100.0  
##  Mean   :100     Mean   :30.65    Mean   :200.6     Mean   :100.2  
##  3rd Qu.:113     3rd Qu.:36.75    3rd Qu.:234.1     3rd Qu.:114.0  
##  Max.   :165     Max.   :59.76    Max.   :363.7     Max.   :170.0  
##  total_eve_charge total_night_minutes total_night_calls total_night_charge
##  Min.   : 0.00    Min.   :  0.0       Min.   :  0.00    Min.   : 0.000    
##  1st Qu.:14.14    1st Qu.:166.9       1st Qu.: 87.00    1st Qu.: 7.510    
##  Median :17.09    Median :200.4       Median :100.00    Median : 9.020    
##  Mean   :17.05    Mean   :200.4       Mean   : 99.92    Mean   : 9.018    
##  3rd Qu.:19.90    3rd Qu.:234.7       3rd Qu.:113.00    3rd Qu.:10.560    
##  Max.   :30.91    Max.   :395.0       Max.   :175.00    Max.   :17.770    
##  total_intl_minutes total_intl_calls total_intl_charge
##  Min.   : 0.00      Min.   : 0.000   Min.   :0.000    
##  1st Qu.: 8.50      1st Qu.: 3.000   1st Qu.:2.300    
##  Median :10.30      Median : 4.000   Median :2.780    
##  Mean   :10.26      Mean   : 4.435   Mean   :2.771    
##  3rd Qu.:12.00      3rd Qu.: 6.000   3rd Qu.:3.240    
##  Max.   :20.00      Max.   :20.000   Max.   :5.400    
##  number_customer_service_calls    churn          
##  Min.   :0.00                  Length:5000       
##  1st Qu.:1.00                  Class :character  
##  Median :1.00                  Mode  :character  
##  Mean   :1.57                                    
##  3rd Qu.:2.00                                    
##  Max.   :9.00
```

```r
#library for the sample.split function
library(caTools)

#splitting the data into training and testing data 
# sample the input data with 70% for training and 30% for testing
data1$churn <- factor(data1$churn)
sample <- sample.split(data1$churn,SplitRatio=0.70)

trainData <- subset(data1,sample==TRUE)

testData <- subset(data1,sample==FALSE)

#rpart --- recursive partitioning decision tree
library(rpart)

#TO BUILD DECISON TREE BY USING RPART PACKAGE(recursive partitioning decision tree)
churn_model<- rpart(churn ~ .,data=trainData)
churn_model
```

```
## n= 3500 
## 
## node), split, n, loss, yval, (yprob)
##       * denotes terminal node
## 
##   1) root 3500 495 no (0.85857143 0.14142857)  
##     2) total_day_minutes< 264.45 3275 362 no (0.88946565 0.11053435)  
##       4) number_customer_service_calls< 3.5 3012 235 no (0.92197875 0.07802125)  
##         8) international_plan=no 2735 125 no (0.95429616 0.04570384)  
##          16) total_day_minutes< 221.85 2289  54 no (0.97640891 0.02359109) *
##          17) total_day_minutes>=221.85 446  71 no (0.84080717 0.15919283)  
##            34) total_eve_minutes< 259.8 396  40 no (0.89898990 0.10101010)  
##              68) total_eve_minutes< 205.6 250   9 no (0.96400000 0.03600000) *
##              69) total_eve_minutes>=205.6 146  31 no (0.78767123 0.21232877)  
##               138) total_day_minutes< 244.55 94   8 no (0.91489362 0.08510638) *
##               139) total_day_minutes>=244.55 52  23 no (0.55769231 0.44230769)  
##                 278) total_night_minutes< 223.4 32   5 no (0.84375000 0.15625000) *
##                 279) total_night_minutes>=223.4 20   2 yes (0.10000000 0.90000000) *
##            35) total_eve_minutes>=259.8 50  19 yes (0.38000000 0.62000000)  
##              70) voice_mail_plan=yes 12   0 no (1.00000000 0.00000000) *
##              71) voice_mail_plan=no 38   7 yes (0.18421053 0.81578947) *
##         9) international_plan=yes 277 110 no (0.60288809 0.39711191)  
##          18) total_intl_calls>=2.5 223  56 no (0.74887892 0.25112108)  
##            36) total_intl_minutes< 13.05 174   7 no (0.95977011 0.04022989) *
##            37) total_intl_minutes>=13.05 49   0 yes (0.00000000 1.00000000) *
##          19) total_intl_calls< 2.5 54   0 yes (0.00000000 1.00000000) *
##       5) number_customer_service_calls>=3.5 263 127 no (0.51711027 0.48288973)  
##        10) total_day_minutes>=160.65 158  34 no (0.78481013 0.21518987)  
##          20) total_eve_minutes>=141.45 142  23 no (0.83802817 0.16197183) *
##          21) total_eve_minutes< 141.45 16   5 yes (0.31250000 0.68750000) *
##        11) total_day_minutes< 160.65 105  12 yes (0.11428571 0.88571429) *
##     3) total_day_minutes>=264.45 225  92 yes (0.40888889 0.59111111)  
##       6) voice_mail_plan=yes 48   2 no (0.95833333 0.04166667) *
##       7) voice_mail_plan=no 177  46 yes (0.25988701 0.74011299)  
##        14) total_eve_minutes< 138.25 26   2 no (0.92307692 0.07692308) *
##        15) total_eve_minutes>=138.25 151  22 yes (0.14569536 0.85430464)  
##          30) total_night_minutes< 114.5 8   1 no (0.87500000 0.12500000) *
##          31) total_night_minutes>=114.5 143  15 yes (0.10489510 0.89510490) *
```

```r
#to display it in diagram

library(rattle)
```

```
## Loading required package: tibble
```

```
## Loading required package: bitops
```

```
## Rattle: A free graphical interface for data science with R.
## Version 5.5.1 Copyright (c) 2006-2021 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.
```

```r
library(rpart.plot)

fancyRpartPlot(churn_model)
```

<img src="07-decisiontree_files/figure-html/project-1.png" width="672" />

```r
#predicting the model data based on the model built from training data set
pred<- predict(churn_model,testData,type = "class" )
pred
```

```
##    2    3    8   27   28   29   33   34   44   45   47   49   52   53   60   61 
##   no   no   no   no   no   no   no  yes   no   no   no  yes   no   no   no   no 
##   68   75   79   81   83   87   94   99  100  102  104  107  108  112  113  119 
##   no   no   no   no   no  yes   no   no  yes   no   no   no   no   no   no   no 
##  122  125  128  130  131  136  139  140  144  146  147  150  153  157  164  167 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
##  172  176  178  179  183  184  185  188  189  196  198  200  204  206  207  208 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  210  211  212  213  214  217  220  223  226  227  231  241  243  244  249  254 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  257  259  266  269  273  277  278  280  290  292  293  294  295  300  302  303 
##   no  yes   no   no   no   no   no   no  yes   no   no  yes   no   no  yes   no 
##  305  308  312  313  315  323  324  329  333  342  343  350  352  354  357  358 
##   no  yes   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no 
##  366  368  369  372  374  376  383  390  392  398  399  400  409  415  420  424 
##  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  425  426  434  435  441  449  450  451  453  464  465  467  468  469  470  473 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  475  476  479  481  486  487  494  497  498  501  502  504  511  518  520  521 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  531  532  535  537  545  548  551  552  553  556  558  570  572  573  575  582 
##   no   no   no   no   no  yes   no   no  yes   no   no  yes   no   no  yes   no 
##  583  587  588  592  596  599  601  602  603  605  606  607  615  619  621  623 
##   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no   no 
##  626  632  634  635  642  643  645  652  660  663  667  671  678  686  688  689 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  692  694  697  702  703  706  709  710  712  720  721  722  723  724  725  730 
##   no   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no 
##  734  736  737  741  748  749  762  763  774  777  781  783  789  793  800  805 
##   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no   no   no 
##  810  812  813  814  816  817  819  820  822  827  830  831  832  835  836  839 
##   no   no   no   no  yes   no   no   no   no   no   no  yes   no   no   no   no 
##  841  842  847  854  856  857  859  860  861  862  864  865  866  867  870  871 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
##  875  880  882  887  893  896  898  902  907  910  911  916  919  932  936  944 
##  yes   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
##  945  955  958  960  962  965  970  974  980  983  988  989  991  992  993  994 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  995  997  999 1004 1011 1015 1018 1021 1022 1024 1031 1032 1038 1039 1040 1043 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 1044 1045 1051 1054 1055 1057 1059 1063 1069 1070 1071 1073 1077 1084 1086 1087 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1095 1096 1100 1101 1102 1105 1107 1108 1113 1116 1132 1133 1135 1137 1138 1140 
##   no  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1144 1150 1155 1159 1162 1163 1164 1174 1180 1183 1184 1192 1196 1199 1200 1203 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 1205 1206 1210 1212 1214 1215 1218 1222 1230 1233 1236 1241 1245 1246 1251 1252 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1255 1259 1268 1270 1275 1280 1281 1285 1288 1292 1299 1304 1305 1306 1308 1321 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 1331 1339 1343 1344 1348 1354 1357 1372 1378 1379 1386 1387 1388 1389 1392 1398 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1399 1404 1405 1409 1410 1411 1419 1428 1430 1432 1438 1441 1443 1444 1447 1449 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1453 1454 1462 1468 1469 1483 1484 1495 1500 1503 1505 1506 1509 1512 1513 1516 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 1522 1533 1535 1536 1539 1540 1550 1554 1558 1561 1562 1564 1568 1569 1577 1578 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 1580 1587 1592 1601 1606 1611 1623 1627 1634 1642 1645 1648 1650 1656 1660 1662 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 1663 1665 1667 1669 1673 1678 1681 1683 1684 1685 1686 1691 1692 1693 1699 1701 
##  yes   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no   no 
## 1702 1708 1709 1711 1720 1721 1731 1733 1734 1735 1746 1752 1755 1758 1768 1771 
##  yes  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 1772 1783 1787 1791 1792 1793 1794 1797 1800 1801 1806 1812 1822 1823 1824 1826 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1828 1832 1837 1839 1845 1846 1847 1848 1849 1854 1856 1860 1861 1866 1867 1875 
##   no  yes   no   no  yes  yes   no   no   no   no   no   no   no  yes   no   no 
## 1880 1881 1889 1893 1894 1899 1908 1917 1919 1925 1929 1931 1932 1938 1939 1940 
##   no   no   no  yes  yes  yes   no   no   no   no   no   no   no   no   no   no 
## 1951 1953 1955 1956 1957 1959 1967 1968 1969 1971 1974 1975 1978 1979 1980 1982 
##  yes   no  yes   no   no   no   no   no   no   no   no  yes  yes  yes   no   no 
## 1983 1984 1991 1994 1995 1996 1998 2009 2010 2014 2026 2028 2031 2032 2036 2038 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 2040 2043 2049 2050 2052 2053 2062 2069 2070 2072 2074 2081 2083 2084 2085 2086 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 2089 2090 2093 2097 2098 2106 2107 2109 2110 2118 2119 2120 2125 2126 2129 2131 
##   no   no   no   no   no   no  yes   no   no   no  yes  yes   no   no   no   no 
## 2133 2134 2140 2142 2143 2145 2147 2148 2154 2158 2161 2163 2164 2165 2166 2175 
##   no   no   no   no   no   no   no  yes   no   no  yes   no  yes  yes   no   no 
## 2181 2182 2187 2188 2194 2197 2202 2203 2215 2221 2229 2232 2234 2235 2238 2240 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2242 2243 2245 2246 2247 2251 2255 2264 2266 2267 2268 2269 2273 2274 2279 2284 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes 
## 2294 2296 2298 2307 2315 2325 2330 2349 2350 2352 2354 2363 2379 2381 2382 2387 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no  yes 
## 2389 2392 2395 2396 2400 2403 2404 2405 2406 2415 2418 2419 2426 2430 2434 2437 
##  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 2440 2442 2443 2451 2452 2454 2455 2456 2460 2471 2474 2476 2480 2484 2488 2489 
##   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no 
## 2494 2495 2499 2501 2503 2505 2506 2508 2510 2511 2512 2516 2518 2519 2523 2524 
##  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 2525 2526 2531 2532 2536 2538 2539 2541 2542 2544 2545 2552 2555 2556 2558 2562 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2566 2568 2580 2590 2593 2598 2601 2603 2604 2607 2612 2616 2618 2623 2624 2625 
##   no  yes   no   no   no   no   no  yes   no  yes   no   no   no  yes   no   no 
## 2627 2628 2630 2635 2641 2642 2644 2649 2657 2661 2664 2682 2683 2690 2707 2711 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 2713 2715 2737 2738 2740 2748 2749 2752 2755 2756 2758 2759 2761 2766 2767 2769 
##   no   no   no   no   no  yes   no   no   no   no   no   no  yes   no   no   no 
## 2771 2772 2777 2779 2781 2786 2787 2788 2792 2797 2798 2802 2803 2808 2810 2813 
##   no   no   no   no   no   no  yes   no   no   no   no  yes   no   no   no   no 
## 2815 2819 2824 2838 2839 2842 2843 2849 2854 2860 2867 2869 2871 2879 2882 2883 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes 
## 2885 2888 2889 2898 2899 2901 2903 2904 2908 2914 2915 2917 2918 2922 2924 2925 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 2926 2930 2932 2935 2938 2941 2942 2943 2944 2948 2949 2950 2951 2961 2964 2965 
##   no   no   no   no   no   no   no  yes   no  yes   no   no   no  yes   no  yes 
## 2970 2972 2973 2977 2979 2987 2990 2993 2994 2998 3000 3001 3005 3006 3010 3013 
##   no  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3018 3025 3026 3029 3033 3036 3037 3041 3043 3052 3060 3061 3063 3064 3065 3066 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no  yes   no 
## 3067 3072 3074 3078 3079 3080 3081 3082 3087 3089 3099 3101 3108 3114 3117 3119 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no   no 
## 3120 3123 3125 3133 3134 3135 3139 3145 3146 3147 3148 3155 3157 3159 3161 3163 
##   no   no   no  yes   no   no   no  yes   no   no  yes   no   no   no   no   no 
## 3164 3166 3168 3169 3170 3175 3176 3180 3183 3195 3197 3198 3203 3208 3217 3219 
##   no   no   no  yes  yes   no   no   no   no   no   no   no   no   no   no   no 
## 3221 3222 3223 3226 3229 3232 3233 3236 3239 3240 3245 3252 3253 3259 3262 3265 
##   no   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no 
## 3266 3269 3276 3277 3279 3280 3284 3287 3293 3297 3302 3303 3306 3307 3312 3314 
##  yes  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 3315 3316 3321 3322 3324 3331 3332 3333 3336 3341 3347 3348 3350 3356 3363 3365 
##   no   no  yes   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 3367 3368 3384 3386 3387 3388 3391 3392 3400 3402 3403 3406 3408 3409 3410 3416 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 3417 3418 3420 3423 3427 3428 3431 3440 3441 3442 3445 3446 3450 3459 3460 3465 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3469 3474 3476 3479 3480 3486 3487 3488 3489 3491 3492 3496 3497 3500 3502 3507 
##   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no   no   no 
## 3510 3512 3513 3518 3520 3524 3525 3529 3530 3540 3541 3542 3546 3554 3556 3557 
##  yes   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes   no 
## 3558 3569 3571 3572 3573 3576 3585 3595 3601 3608 3609 3610 3614 3620 3622 3623 
##   no   no   no   no   no   no   no   no   no   no  yes  yes  yes   no   no   no 
## 3624 3626 3627 3628 3635 3639 3644 3649 3652 3663 3664 3666 3667 3668 3671 3674 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3677 3679 3683 3685 3691 3692 3702 3704 3706 3711 3712 3716 3725 3730 3731 3740 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3744 3750 3751 3761 3766 3768 3774 3780 3783 3788 3790 3793 3795 3800 3803 3804 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3806 3809 3819 3820 3821 3822 3823 3824 3825 3828 3833 3836 3841 3842 3844 3845 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3850 3853 3855 3857 3858 3859 3869 3873 3874 3876 3881 3884 3885 3890 3895 3904 
##   no  yes  yes   no   no   no  yes   no   no   no   no   no  yes   no   no   no 
## 3905 3914 3919 3921 3926 3934 3936 3937 3940 3941 3942 3943 3944 3948 3949 3959 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3962 3963 3969 3977 3978 3983 3985 3986 3988 3994 3996 3997 3998 4005 4014 4015 
##   no   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no 
## 4022 4031 4032 4047 4048 4055 4056 4057 4059 4062 4072 4073 4075 4076 4082 4084 
##  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4086 4089 4091 4096 4098 4099 4100 4106 4110 4114 4116 4124 4126 4127 4128 4131 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 4132 4145 4157 4161 4162 4164 4165 4167 4169 4174 4178 4181 4182 4186 4193 4195 
##  yes   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 4196 4198 4199 4202 4211 4213 4215 4217 4220 4221 4223 4224 4226 4229 4230 4232 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no 
## 4236 4237 4239 4240 4241 4243 4248 4250 4251 4254 4256 4258 4259 4265 4267 4268 
##   no  yes   no   no   no   no   no   no   no  yes   no   no  yes  yes   no   no 
## 4270 4271 4276 4278 4279 4284 4286 4288 4293 4295 4296 4305 4306 4311 4312 4315 
##   no   no   no   no   no  yes  yes   no  yes   no   no   no   no   no   no   no 
## 4318 4323 4325 4326 4330 4343 4345 4347 4349 4353 4356 4362 4365 4368 4369 4370 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4371 4375 4376 4381 4388 4397 4398 4403 4412 4415 4421 4422 4424 4426 4427 4429 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 4430 4433 4435 4437 4440 4441 4445 4447 4449 4452 4455 4457 4459 4462 4463 4468 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4469 4473 4476 4479 4484 4485 4488 4489 4496 4497 4499 4501 4502 4505 4508 4509 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no  yes   no 
## 4512 4516 4518 4524 4525 4530 4532 4536 4538 4547 4551 4553 4557 4561 4562 4565 
##   no  yes   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 4566 4568 4575 4576 4578 4585 4594 4595 4603 4620 4621 4624 4625 4626 4628 4638 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 4639 4640 4643 4645 4649 4650 4653 4655 4656 4657 4661 4662 4667 4668 4680 4681 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4687 4694 4695 4696 4697 4705 4707 4708 4710 4711 4715 4719 4720 4723 4726 4727 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4732 4733 4737 4738 4740 4744 4751 4754 4757 4758 4759 4762 4771 4777 4779 4781 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no  yes  yes   no 
## 4782 4788 4789 4792 4796 4801 4803 4804 4805 4807 4808 4809 4813 4814 4815 4816 
##   no   no  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 4818 4821 4822 4823 4826 4827 4831 4832 4834 4840 4842 4843 4844 4845 4848 4853 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 4858 4860 4862 4863 4864 4865 4868 4869 4870 4873 4880 4887 4888 4890 4892 4897 
##   no   no  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 4906 4907 4908 4912 4913 4915 4918 4921 4923 4925 4932 4933 4939 4941 4942 4943 
##   no   no   no   no   no  yes  yes   no   no  yes   no   no   no   no   no   no 
## 4944 4945 4951 4955 4957 4964 4978 4984 4991 4992 4997 5000 
##  yes   no   no   no   no   no   no   no  yes  yes   no   no 
## Levels: no yes
```

```r
pred1<- data.frame(pred)
#View(pred1)

#Confusion matrix is a technique for summarizing the performance of a classification algorithm 
table(testData$churn ,pred)#create confusion matrix to see how mnay cus are correctly predicted and incorrectly predicted
```

```
##      pred
##         no  yes
##   no  1266   22
##   yes   66  146
```

```r
#####
#####RANDOM FOREST MODEL
#####


library(randomForest)
```

```
## randomForest 4.7-1.1
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:rattle':
## 
##     importance
```

```r
#to build the random forest model 
#trainData$churn = as.character(trainData$churn)
random_model <- randomForest(churn~.,data=trainData, importance = T)
random_model
```

```
## 
## Call:
##  randomForest(formula = churn ~ ., data = trainData, importance = T) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 4
## 
##         OOB estimate of  error rate: 3.8%
## Confusion matrix:
##       no yes class.error
## no  2983  22 0.007321131
## yes  111 384 0.224242424
```

```r
#predicting the data based on the model developed
ran_pred <- predict(random_model,testData)
ran_pred
```

```
##    2    3    8   27   28   29   33   34   44   45   47   49   52   53   60   61 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
##   68   75   79   81   83   87   94   99  100  102  104  107  108  112  113  119 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no  yes   no 
##  122  125  128  130  131  136  139  140  144  146  147  150  153  157  164  167 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
##  172  176  178  179  183  184  185  188  189  196  198  200  204  206  207  208 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  210  211  212  213  214  217  220  223  226  227  231  241  243  244  249  254 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  257  259  266  269  273  277  278  280  290  292  293  294  295  300  302  303 
##   no  yes   no   no   no   no   no   no  yes   no   no  yes   no   no  yes   no 
##  305  308  312  313  315  323  324  329  333  342  343  350  352  354  357  358 
##   no  yes   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no 
##  366  368  369  372  374  376  383  390  392  398  399  400  409  415  420  424 
##  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  425  426  434  435  441  449  450  451  453  464  465  467  468  469  470  473 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  475  476  479  481  486  487  494  497  498  501  502  504  511  518  520  521 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  531  532  535  537  545  548  551  552  553  556  558  570  572  573  575  582 
##   no   no   no   no   no  yes  yes   no  yes   no   no  yes   no   no  yes   no 
##  583  587  588  592  596  599  601  602  603  605  606  607  615  619  621  623 
##   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no   no 
##  626  632  634  635  642  643  645  652  660  663  667  671  678  686  688  689 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  692  694  697  702  703  706  709  710  712  720  721  722  723  724  725  730 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
##  734  736  737  741  748  749  762  763  774  777  781  783  789  793  800  805 
##   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no   no   no 
##  810  812  813  814  816  817  819  820  822  827  830  831  832  835  836  839 
##   no   no   no   no  yes   no   no   no   no   no   no  yes   no   no   no   no 
##  841  842  847  854  856  857  859  860  861  862  864  865  866  867  870  871 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
##  875  880  882  887  893  896  898  902  907  910  911  916  919  932  936  944 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
##  945  955  958  960  962  965  970  974  980  983  988  989  991  992  993  994 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  995  997  999 1004 1011 1015 1018 1021 1022 1024 1031 1032 1038 1039 1040 1043 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 1044 1045 1051 1054 1055 1057 1059 1063 1069 1070 1071 1073 1077 1084 1086 1087 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1095 1096 1100 1101 1102 1105 1107 1108 1113 1116 1132 1133 1135 1137 1138 1140 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 1144 1150 1155 1159 1162 1163 1164 1174 1180 1183 1184 1192 1196 1199 1200 1203 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 1205 1206 1210 1212 1214 1215 1218 1222 1230 1233 1236 1241 1245 1246 1251 1252 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1255 1259 1268 1270 1275 1280 1281 1285 1288 1292 1299 1304 1305 1306 1308 1321 
##   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no   no 
## 1331 1339 1343 1344 1348 1354 1357 1372 1378 1379 1386 1387 1388 1389 1392 1398 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1399 1404 1405 1409 1410 1411 1419 1428 1430 1432 1438 1441 1443 1444 1447 1449 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1453 1454 1462 1468 1469 1483 1484 1495 1500 1503 1505 1506 1509 1512 1513 1516 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 1522 1533 1535 1536 1539 1540 1550 1554 1558 1561 1562 1564 1568 1569 1577 1578 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 1580 1587 1592 1601 1606 1611 1623 1627 1634 1642 1645 1648 1650 1656 1660 1662 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1663 1665 1667 1669 1673 1678 1681 1683 1684 1685 1686 1691 1692 1693 1699 1701 
##  yes   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no   no 
## 1702 1708 1709 1711 1720 1721 1731 1733 1734 1735 1746 1752 1755 1758 1768 1771 
##  yes  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 1772 1783 1787 1791 1792 1793 1794 1797 1800 1801 1806 1812 1822 1823 1824 1826 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1828 1832 1837 1839 1845 1846 1847 1848 1849 1854 1856 1860 1861 1866 1867 1875 
##   no   no   no   no  yes  yes   no   no   no   no   no   no   no  yes   no   no 
## 1880 1881 1889 1893 1894 1899 1908 1917 1919 1925 1929 1931 1932 1938 1939 1940 
##   no   no   no  yes  yes  yes   no   no   no   no   no   no   no   no   no   no 
## 1951 1953 1955 1956 1957 1959 1967 1968 1969 1971 1974 1975 1978 1979 1980 1982 
##  yes   no  yes   no   no   no   no   no   no   no   no  yes  yes  yes   no   no 
## 1983 1984 1991 1994 1995 1996 1998 2009 2010 2014 2026 2028 2031 2032 2036 2038 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 2040 2043 2049 2050 2052 2053 2062 2069 2070 2072 2074 2081 2083 2084 2085 2086 
##   no   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no 
## 2089 2090 2093 2097 2098 2106 2107 2109 2110 2118 2119 2120 2125 2126 2129 2131 
##   no   no   no   no   no   no  yes   no   no   no  yes  yes   no   no   no   no 
## 2133 2134 2140 2142 2143 2145 2147 2148 2154 2158 2161 2163 2164 2165 2166 2175 
##   no   no   no   no   no   no   no  yes   no   no  yes   no   no  yes   no   no 
## 2181 2182 2187 2188 2194 2197 2202 2203 2215 2221 2229 2232 2234 2235 2238 2240 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2242 2243 2245 2246 2247 2251 2255 2264 2266 2267 2268 2269 2273 2274 2279 2284 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 2294 2296 2298 2307 2315 2325 2330 2349 2350 2352 2354 2363 2379 2381 2382 2387 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no  yes 
## 2389 2392 2395 2396 2400 2403 2404 2405 2406 2415 2418 2419 2426 2430 2434 2437 
##  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 2440 2442 2443 2451 2452 2454 2455 2456 2460 2471 2474 2476 2480 2484 2488 2489 
##   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no 
## 2494 2495 2499 2501 2503 2505 2506 2508 2510 2511 2512 2516 2518 2519 2523 2524 
##  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 2525 2526 2531 2532 2536 2538 2539 2541 2542 2544 2545 2552 2555 2556 2558 2562 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2566 2568 2580 2590 2593 2598 2601 2603 2604 2607 2612 2616 2618 2623 2624 2625 
##   no   no   no   no   no   no   no  yes   no  yes   no   no   no  yes   no   no 
## 2627 2628 2630 2635 2641 2642 2644 2649 2657 2661 2664 2682 2683 2690 2707 2711 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no  yes 
## 2713 2715 2737 2738 2740 2748 2749 2752 2755 2756 2758 2759 2761 2766 2767 2769 
##   no   no   no   no   no  yes   no   no   no   no   no   no  yes   no   no   no 
## 2771 2772 2777 2779 2781 2786 2787 2788 2792 2797 2798 2802 2803 2808 2810 2813 
##   no   no   no   no   no   no  yes   no   no   no   no  yes   no   no   no   no 
## 2815 2819 2824 2838 2839 2842 2843 2849 2854 2860 2867 2869 2871 2879 2882 2883 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes 
## 2885 2888 2889 2898 2899 2901 2903 2904 2908 2914 2915 2917 2918 2922 2924 2925 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 2926 2930 2932 2935 2938 2941 2942 2943 2944 2948 2949 2950 2951 2961 2964 2965 
##   no   no   no   no   no   no   no  yes   no  yes   no   no   no  yes   no  yes 
## 2970 2972 2973 2977 2979 2987 2990 2993 2994 2998 3000 3001 3005 3006 3010 3013 
##   no  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3018 3025 3026 3029 3033 3036 3037 3041 3043 3052 3060 3061 3063 3064 3065 3066 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no  yes   no 
## 3067 3072 3074 3078 3079 3080 3081 3082 3087 3089 3099 3101 3108 3114 3117 3119 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no   no 
## 3120 3123 3125 3133 3134 3135 3139 3145 3146 3147 3148 3155 3157 3159 3161 3163 
##   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no   no 
## 3164 3166 3168 3169 3170 3175 3176 3180 3183 3195 3197 3198 3203 3208 3217 3219 
##   no   no   no  yes  yes   no   no   no   no   no   no   no   no   no   no   no 
## 3221 3222 3223 3226 3229 3232 3233 3236 3239 3240 3245 3252 3253 3259 3262 3265 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 3266 3269 3276 3277 3279 3280 3284 3287 3293 3297 3302 3303 3306 3307 3312 3314 
##  yes  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 3315 3316 3321 3322 3324 3331 3332 3333 3336 3341 3347 3348 3350 3356 3363 3365 
##   no   no  yes   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 3367 3368 3384 3386 3387 3388 3391 3392 3400 3402 3403 3406 3408 3409 3410 3416 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3417 3418 3420 3423 3427 3428 3431 3440 3441 3442 3445 3446 3450 3459 3460 3465 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 3469 3474 3476 3479 3480 3486 3487 3488 3489 3491 3492 3496 3497 3500 3502 3507 
##   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no   no   no 
## 3510 3512 3513 3518 3520 3524 3525 3529 3530 3540 3541 3542 3546 3554 3556 3557 
##  yes   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes   no 
## 3558 3569 3571 3572 3573 3576 3585 3595 3601 3608 3609 3610 3614 3620 3622 3623 
##   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no 
## 3624 3626 3627 3628 3635 3639 3644 3649 3652 3663 3664 3666 3667 3668 3671 3674 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3677 3679 3683 3685 3691 3692 3702 3704 3706 3711 3712 3716 3725 3730 3731 3740 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3744 3750 3751 3761 3766 3768 3774 3780 3783 3788 3790 3793 3795 3800 3803 3804 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3806 3809 3819 3820 3821 3822 3823 3824 3825 3828 3833 3836 3841 3842 3844 3845 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3850 3853 3855 3857 3858 3859 3869 3873 3874 3876 3881 3884 3885 3890 3895 3904 
##   no   no  yes   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 3905 3914 3919 3921 3926 3934 3936 3937 3940 3941 3942 3943 3944 3948 3949 3959 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3962 3963 3969 3977 3978 3983 3985 3986 3988 3994 3996 3997 3998 4005 4014 4015 
##   no   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no 
## 4022 4031 4032 4047 4048 4055 4056 4057 4059 4062 4072 4073 4075 4076 4082 4084 
##  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4086 4089 4091 4096 4098 4099 4100 4106 4110 4114 4116 4124 4126 4127 4128 4131 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 4132 4145 4157 4161 4162 4164 4165 4167 4169 4174 4178 4181 4182 4186 4193 4195 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4196 4198 4199 4202 4211 4213 4215 4217 4220 4221 4223 4224 4226 4229 4230 4232 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes  yes  yes   no 
## 4236 4237 4239 4240 4241 4243 4248 4250 4251 4254 4256 4258 4259 4265 4267 4268 
##   no   no   no   no   no   no   no   no   no  yes   no   no  yes  yes   no   no 
## 4270 4271 4276 4278 4279 4284 4286 4288 4293 4295 4296 4305 4306 4311 4312 4315 
##   no   no   no   no   no  yes  yes   no  yes   no   no   no   no   no   no   no 
## 4318 4323 4325 4326 4330 4343 4345 4347 4349 4353 4356 4362 4365 4368 4369 4370 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 4371 4375 4376 4381 4388 4397 4398 4403 4412 4415 4421 4422 4424 4426 4427 4429 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 4430 4433 4435 4437 4440 4441 4445 4447 4449 4452 4455 4457 4459 4462 4463 4468 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4469 4473 4476 4479 4484 4485 4488 4489 4496 4497 4499 4501 4502 4505 4508 4509 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no  yes   no 
## 4512 4516 4518 4524 4525 4530 4532 4536 4538 4547 4551 4553 4557 4561 4562 4565 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 4566 4568 4575 4576 4578 4585 4594 4595 4603 4620 4621 4624 4625 4626 4628 4638 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 4639 4640 4643 4645 4649 4650 4653 4655 4656 4657 4661 4662 4667 4668 4680 4681 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4687 4694 4695 4696 4697 4705 4707 4708 4710 4711 4715 4719 4720 4723 4726 4727 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4732 4733 4737 4738 4740 4744 4751 4754 4757 4758 4759 4762 4771 4777 4779 4781 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no  yes  yes   no 
## 4782 4788 4789 4792 4796 4801 4803 4804 4805 4807 4808 4809 4813 4814 4815 4816 
##   no   no  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 4818 4821 4822 4823 4826 4827 4831 4832 4834 4840 4842 4843 4844 4845 4848 4853 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 4858 4860 4862 4863 4864 4865 4868 4869 4870 4873 4880 4887 4888 4890 4892 4897 
##   no   no  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 4906 4907 4908 4912 4913 4915 4918 4921 4923 4925 4932 4933 4939 4941 4942 4943 
##   no   no   no   no   no  yes  yes   no   no  yes   no   no   no   no   no   no 
## 4944 4945 4951 4955 4957 4964 4978 4984 4991 4992 4997 5000 
##  yes   no   no   no   no   no   no   no  yes  yes   no   no 
## Levels: no yes
```

```r
ran_pred1 <-data.frame(ran_pred)

table(testData$churn ,ran_pred) # confusion matrix
```

```
##      ran_pred
##         no  yes
##   no  1280    8
##   yes   64  148
```

```r
#accuracy rate 
(1276+ 173)/(1276+12+39+173) # 97 % of accuracy : higher than decision tree , where accuracy rate is 95 % 
```

```
## [1] 0.966
```

```r
#most commonly mean decrease gini is considered
importance(random_model)
```

```
##                                       no        yes MeanDecreaseAccuracy
## international_plan            75.9276391 87.7569214           94.2923001
## voice_mail_plan               22.0419178 24.1896849           25.0883355
## number_vmail_messages         19.5950841 22.2511923           22.8621802
## total_day_minutes             35.0027779 37.0510828           45.5920890
## total_day_calls                2.8089050 -0.2721065            2.3804446
## total_day_charge              32.4957507 34.8419651           40.6883574
## total_eve_minutes             21.9363498 22.1352263           25.6449142
## total_eve_calls               -0.2381585  0.7881132            0.1373284
## total_eve_charge              21.8687942 22.3997798           25.6167519
## total_night_minutes           19.0254813  9.3682074           20.3892225
## total_night_calls             -0.4736799 -0.1900218           -0.5189115
## total_night_charge            19.2378059  7.4666204           20.4443894
## total_intl_minutes            22.3929840 20.0203768           28.1047881
## total_intl_calls              32.1051729 47.3568155           46.9145410
## total_intl_charge             22.1147970 19.2889055           26.6142573
## number_customer_service_calls 80.9048634 99.1496488          109.2607217
##                               MeanDecreaseGini
## international_plan                    83.21250
## voice_mail_plan                       20.76575
## number_vmail_messages                 24.44583
## total_day_minutes                    127.94021
## total_day_calls                       24.48775
## total_day_charge                     117.93104
## total_eve_minutes                     55.87562
## total_eve_calls                       22.88268
## total_eve_charge                      57.35853
## total_night_minutes                   36.32438
## total_night_calls                     22.94186
## total_night_charge                    36.74443
## total_intl_minutes                    37.69195
## total_intl_calls                      48.11172
## total_intl_charge                     35.87547
## number_customer_service_calls         97.20376
```

```r
# Variables that result in nodes with higher purity have a higher decrease in Gini coefficient.
varImpPlot(random_model)
```

<img src="07-decisiontree_files/figure-html/project-2.png" width="672" />

```r
#total day charges have higher decrease in gini index so its more significant than other
```

