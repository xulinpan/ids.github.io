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
##  1) root 3500 495 no (0.85857143 0.14142857)  
##    2) total_day_minutes< 265.4 3294 368 no (0.88828172 0.11171828)  
##      4) number_customer_service_calls< 3.5 3031 241 no (0.92048829 0.07951171)  
##        8) international_plan=no 2753 137 no (0.95023611 0.04976389)  
##         16) total_day_minutes< 221.85 2297  61 no (0.97344362 0.02655638) *
##         17) total_day_minutes>=221.85 456  76 no (0.83333333 0.16666667)  
##           34) total_eve_minutes< 265.95 412  41 no (0.90048544 0.09951456) *
##           35) total_eve_minutes>=265.95 44   9 yes (0.20454545 0.79545455)  
##             70) voice_mail_plan=yes 7   0 no (1.00000000 0.00000000) *
##             71) voice_mail_plan=no 37   2 yes (0.05405405 0.94594595) *
##        9) international_plan=yes 278 104 no (0.62589928 0.37410072)  
##         18) total_intl_calls>=2.5 223  49 no (0.78026906 0.21973094)  
##           36) total_intl_minutes< 13.05 180   6 no (0.96666667 0.03333333) *
##           37) total_intl_minutes>=13.05 43   0 yes (0.00000000 1.00000000) *
##         19) total_intl_calls< 2.5 55   0 yes (0.00000000 1.00000000) *
##      5) number_customer_service_calls>=3.5 263 127 no (0.51711027 0.48288973)  
##       10) total_day_minutes>=160.25 162  38 no (0.76543210 0.23456790)  
##         20) total_eve_minutes>=146.8 137  24 no (0.82481752 0.17518248) *
##         21) total_eve_minutes< 146.8 25  11 yes (0.44000000 0.56000000)  
##           42) total_day_minutes>=196.45 15   4 no (0.73333333 0.26666667) *
##           43) total_day_minutes< 196.45 10   0 yes (0.00000000 1.00000000) *
##       11) total_day_minutes< 160.25 101  12 yes (0.11881188 0.88118812) *
##    3) total_day_minutes>=265.4 206  79 yes (0.38349515 0.61650485)  
##      6) voice_mail_plan=yes 45   3 no (0.93333333 0.06666667) *
##      7) voice_mail_plan=no 161  37 yes (0.22981366 0.77018634)  
##       14) total_eve_minutes< 163.95 45  16 no (0.64444444 0.35555556)  
##         28) total_day_minutes< 311.2 38   9 no (0.76315789 0.23684211) *
##         29) total_day_minutes>=311.2 7   0 yes (0.00000000 1.00000000) *
##       15) total_eve_minutes>=163.95 116   8 yes (0.06896552 0.93103448) *
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
##    8   11   12   13   14   16   18   21   25   26   28   33   36   39   45   46 
##   no  yes   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
##   47   49   51   53   55   58   60   63   72   73   79   81   82   92  100  107 
##   no  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
##  115  118  124  127  129  130  136  139  140  143  144  146  147  148  151  154 
##   no  yes   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no 
##  157  158  162  166  171  177  178  183  185  188  189  190  192  195  199  200 
##  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  201  205  208  212  219  222  227  235  236  238  239  241  243  248  250  261 
##   no   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no 
##  263  270  271  272  278  281  283  290  302  304  308  319  320  328  339  341 
##   no   no   no   no   no   no   no  yes  yes   no  yes   no  yes   no  yes   no 
##  342  357  359  365  370  372  375  381  383  386  390  393  401  406  408  411 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
##  420  423  428  432  435  436  437  440  441  444  448  449  451  452  453  456 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  458  481  484  487  488  490  493  495  498  502  506  507  508  510  514  523 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
##  525  526  528  529  537  543  544  547  548  549  552  556  557  563  565  567 
##   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no   no 
##  584  586  590  591  595  598  602  604  605  606  607  608  614  618  619  625 
##   no   no   no   no   no   no  yes   no   no  yes   no   no  yes   no   no   no 
##  628  629  633  635  636  645  646  650  661  662  664  667  668  670  671  672 
##   no   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no 
##  673  674  675  677  679  687  692  694  695  697  698  699  701  703  706  707 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
##  709  711  712  713  718  721  723  729  730  732  733  735  737  740  741  744 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  750  752  753  754  755  758  759  766  770  772  779  781  791  792  794  796 
##   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no   no 
##  797  799  802  811  812  823  825  826  834  836  837  847  855  857  859  860 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
##  864  870  873  881  890  893  897  899  904  905  906  908  910  911  913  916 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  924  925  927  929  930  931  933  942  948  949  950  951  954  956  959  967 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  976  984  985  988  989  990  996 1002 1003 1005 1017 1018 1019 1020 1027 1029 
##  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 1031 1032 1040 1041 1043 1048 1049 1056 1062 1063 1064 1066 1068 1073 1075 1077 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1079 1080 1082 1085 1089 1094 1095 1100 1108 1112 1118 1121 1122 1125 1128 1131 
##  yes   no   no   no   no   no   no  yes   no   no   no   no  yes   no   no   no 
## 1139 1144 1148 1155 1158 1161 1164 1166 1169 1171 1173 1175 1176 1181 1183 1185 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 1191 1192 1196 1204 1208 1209 1215 1220 1233 1235 1241 1244 1245 1247 1250 1255 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1258 1261 1262 1264 1266 1267 1268 1271 1272 1273 1274 1285 1286 1292 1293 1294 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 1298 1301 1305 1308 1309 1310 1312 1313 1322 1323 1328 1340 1341 1345 1346 1351 
##   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no  yes  yes 
## 1352 1355 1359 1363 1366 1367 1375 1376 1379 1380 1381 1383 1384 1385 1387 1394 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1405 1408 1412 1416 1420 1423 1424 1425 1430 1436 1446 1448 1451 1453 1457 1461 
##   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 1464 1466 1468 1476 1477 1480 1482 1486 1489 1490 1494 1497 1500 1501 1502 1505 
##   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 1510 1511 1514 1515 1516 1519 1521 1524 1528 1529 1532 1533 1534 1535 1541 1544 
##   no  yes   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no 
## 1547 1548 1549 1555 1557 1558 1562 1565 1566 1577 1582 1583 1584 1588 1590 1592 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1594 1601 1607 1615 1616 1624 1625 1626 1628 1629 1635 1640 1643 1644 1648 1651 
##  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 1652 1653 1655 1657 1658 1661 1665 1666 1668 1669 1671 1678 1681 1691 1693 1694 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 1695 1701 1703 1715 1718 1723 1724 1726 1727 1729 1744 1745 1749 1753 1757 1759 
##  yes   no  yes   no   no   no   no   no   no  yes   no   no   no  yes   no   no 
## 1764 1765 1766 1770 1771 1782 1785 1787 1791 1794 1795 1797 1801 1807 1815 1821 
##   no   no  yes   no   no   no  yes   no   no   no  yes   no   no   no   no   no 
## 1822 1824 1829 1833 1834 1839 1840 1841 1845 1851 1854 1855 1857 1860 1863 1864 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 1866 1869 1872 1873 1875 1881 1885 1889 1891 1892 1894 1896 1897 1899 1901 1904 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no  yes   no  yes 
## 1905 1916 1917 1925 1930 1931 1933 1944 1947 1948 1949 1958 1960 1973 1974 1976 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1977 1981 1984 1990 1993 1994 1998 2003 2005 2007 2008 2013 2016 2021 2025 2028 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2029 2030 2037 2041 2042 2045 2047 2048 2049 2051 2053 2056 2060 2063 2065 2067 
##  yes  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 2071 2075 2077 2079 2080 2081 2084 2086 2088 2095 2100 2102 2103 2104 2114 2120 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no  yes  yes   no 
## 2124 2126 2132 2133 2134 2135 2136 2145 2146 2147 2149 2153 2159 2161 2164 2169 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes  yes  yes   no 
## 2172 2176 2177 2179 2182 2184 2192 2193 2202 2203 2211 2214 2216 2217 2218 2222 
##   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no 
## 2223 2228 2230 2240 2241 2243 2246 2248 2249 2253 2254 2258 2261 2262 2269 2270 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2273 2275 2277 2283 2284 2287 2289 2290 2291 2294 2296 2298 2300 2302 2305 2307 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2312 2314 2322 2324 2329 2331 2333 2334 2338 2340 2341 2356 2365 2366 2367 2371 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2372 2373 2382 2388 2393 2395 2396 2397 2399 2400 2401 2402 2404 2405 2406 2407 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 2410 2416 2417 2418 2420 2423 2424 2425 2426 2428 2431 2433 2437 2439 2440 2443 
##   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 2449 2450 2452 2453 2456 2459 2461 2464 2469 2470 2480 2485 2489 2490 2496 2498 
##   no   no  yes   no   no   no   no   no  yes   no  yes   no   no   no   no   no 
## 2499 2502 2504 2506 2511 2512 2516 2517 2519 2526 2527 2532 2533 2536 2537 2539 
##   no   no   no   no   no   no  yes   no   no   no  yes   no   no   no  yes   no 
## 2541 2542 2543 2545 2547 2548 2552 2554 2557 2559 2560 2565 2566 2572 2579 2581 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2585 2592 2595 2597 2602 2611 2613 2615 2616 2620 2622 2626 2630 2634 2635 2641 
##   no  yes  yes   no   no   no   no  yes   no  yes   no   no   no   no   no   no 
## 2643 2644 2647 2648 2662 2663 2671 2674 2678 2691 2692 2696 2700 2702 2706 2707 
##   no   no   no  yes   no  yes   no   no   no   no   no   no   no   no   no   no 
## 2717 2722 2730 2735 2743 2755 2759 2762 2764 2766 2768 2769 2771 2779 2784 2787 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 2791 2796 2806 2807 2808 2810 2817 2820 2821 2823 2826 2829 2830 2834 2839 2841 
##   no   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no   no 
## 2844 2845 2846 2847 2851 2852 2861 2865 2875 2878 2879 2884 2885 2889 2891 2897 
##   no   no   no   no   no   no   no   no  yes   no   no   no  yes   no   no   no 
## 2903 2907 2910 2923 2937 2940 2941 2943 2944 2945 2949 2950 2963 2966 2973 2974 
##   no   no   no   no  yes   no   no  yes   no   no   no   no   no   no   no   no 
## 2975 2977 2981 2983 2985 2990 2991 2996 2997 2998 2999 3000 3002 3008 3013 3019 
##   no   no  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 3021 3022 3024 3026 3027 3029 3031 3032 3037 3039 3040 3046 3047 3048 3050 3052 
##   no   no  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3054 3057 3058 3061 3069 3070 3071 3075 3076 3078 3079 3086 3092 3094 3097 3100 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 3105 3108 3109 3110 3116 3118 3123 3127 3129 3135 3137 3138 3140 3141 3145 3151 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no 
## 3154 3155 3159 3165 3166 3168 3171 3173 3183 3187 3188 3189 3191 3197 3199 3209 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3211 3212 3217 3221 3224 3227 3228 3233 3242 3243 3246 3247 3253 3254 3261 3266 
##   no   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no  yes 
## 3268 3269 3274 3276 3278 3282 3283 3284 3288 3290 3292 3294 3295 3299 3302 3304 
##   no  yes   no   no   no   no   no   no   no   no  yes   no   no   no  yes   no 
## 3311 3316 3319 3321 3326 3330 3333 3336 3338 3340 3343 3347 3349 3355 3357 3358 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 3363 3364 3368 3371 3373 3377 3378 3384 3385 3387 3397 3400 3404 3405 3406 3413 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 3418 3425 3426 3428 3429 3433 3446 3451 3452 3456 3462 3465 3467 3468 3475 3476 
##   no   no   no   no  yes   no   no   no   no   no  yes   no   no   no   no   no 
## 3477 3483 3484 3485 3486 3490 3496 3497 3500 3502 3508 3509 3511 3519 3525 3534 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3537 3542 3543 3544 3545 3551 3552 3553 3558 3562 3564 3565 3570 3581 3584 3586 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 3589 3590 3591 3592 3593 3599 3600 3602 3606 3608 3616 3618 3620 3622 3623 3628 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3633 3640 3645 3647 3651 3656 3658 3659 3664 3665 3666 3667 3668 3673 3678 3679 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3696 3701 3703 3704 3705 3708 3712 3716 3718 3720 3722 3724 3726 3727 3731 3732 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 3733 3735 3741 3748 3750 3754 3757 3758 3759 3760 3761 3762 3764 3765 3767 3768 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3770 3773 3776 3777 3778 3780 3786 3791 3792 3793 3798 3800 3804 3807 3810 3812 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3817 3822 3827 3828 3835 3836 3843 3851 3860 3864 3872 3876 3878 3885 3888 3889 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3899 3901 3904 3911 3914 3915 3917 3921 3935 3936 3942 3945 3957 3960 3968 3969 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3977 3981 3982 3984 3985 3989 3994 3998 4000 4001 4009 4010 4016 4018 4019 4021 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no 
## 4022 4026 4027 4030 4031 4032 4035 4038 4039 4042 4043 4044 4047 4051 4056 4059 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4060 4062 4069 4070 4084 4085 4093 4097 4100 4101 4103 4106 4107 4109 4113 4114 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4116 4120 4126 4127 4128 4130 4135 4136 4147 4148 4149 4159 4161 4164 4168 4171 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 4173 4175 4176 4180 4183 4185 4187 4192 4199 4204 4208 4209 4215 4218 4220 4221 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4225 4229 4231 4250 4261 4269 4280 4285 4286 4290 4291 4293 4300 4301 4305 4311 
##   no   no  yes   no   no  yes   no   no  yes   no   no  yes   no   no   no   no 
## 4323 4324 4325 4330 4334 4335 4339 4340 4341 4345 4346 4350 4351 4352 4353 4354 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 4360 4362 4363 4366 4373 4384 4385 4388 4397 4398 4401 4405 4413 4414 4416 4417 
##   no   no   no   no   no  yes   no   no   no   no   no  yes   no   no   no   no 
## 4421 4422 4426 4430 4431 4433 4434 4435 4436 4438 4441 4443 4447 4451 4452 4454 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4456 4463 4464 4466 4469 4470 4471 4477 4479 4483 4486 4487 4489 4494 4495 4497 
##   no   no   no   no   no   no   no  yes   no   no   no  yes  yes   no   no   no 
## 4499 4500 4501 4514 4518 4522 4527 4531 4536 4538 4540 4542 4543 4544 4545 4546 
##   no   no   no  yes   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4547 4556 4560 4563 4567 4572 4576 4583 4586 4588 4589 4595 4597 4600 4602 4605 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no 
## 4607 4608 4611 4612 4621 4623 4624 4626 4627 4632 4645 4648 4653 4659 4662 4664 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 4665 4666 4672 4676 4678 4679 4680 4683 4684 4686 4688 4695 4697 4700 4704 4713 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 4714 4715 4722 4724 4725 4729 4730 4743 4745 4747 4749 4753 4754 4762 4764 4766 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4770 4775 4779 4783 4784 4785 4787 4791 4792 4795 4799 4801 4804 4806 4809 4817 
##   no   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 4818 4819 4825 4828 4832 4837 4840 4854 4857 4862 4869 4875 4878 4879 4881 4885 
##   no   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no 
## 4889 4895 4896 4899 4900 4905 4910 4915 4917 4923 4928 4935 4937 4943 4948 4949 
##   no   no  yes  yes   no   no   no  yes   no   no   no   no   no   no   no  yes 
## 4950 4951 4954 4962 4963 4967 4972 4983 4988 4992 4995 4996 
##   no   no  yes   no   no   no   no   no   no  yes   no   no 
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
##   no  1270   18
##   yes   76  136
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
##         OOB estimate of  error rate: 4%
## Confusion matrix:
##       no yes class.error
## no  2983  22 0.007321131
## yes  118 377 0.238383838
```

```r
#predicting the data based on the model developed
ran_pred <- predict(random_model,testData)
ran_pred
```

```
##    8   11   12   13   14   16   18   21   25   26   28   33   36   39   45   46 
##   no  yes   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
##   47   49   51   53   55   58   60   63   72   73   79   81   82   92  100  107 
##   no  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
##  115  118  124  127  129  130  136  139  140  143  144  146  147  148  151  154 
##   no  yes   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no 
##  157  158  162  166  171  177  178  183  185  188  189  190  192  195  199  200 
##  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes   no 
##  201  205  208  212  219  222  227  235  236  238  239  241  243  248  250  261 
##   no   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no 
##  263  270  271  272  278  281  283  290  302  304  308  319  320  328  339  341 
##   no   no   no   no   no   no   no  yes  yes   no  yes   no  yes   no  yes   no 
##  342  357  359  365  370  372  375  381  383  386  390  393  401  406  408  411 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
##  420  423  428  432  435  436  437  440  441  444  448  449  451  452  453  456 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
##  458  481  484  487  488  490  493  495  498  502  506  507  508  510  514  523 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
##  525  526  528  529  537  543  544  547  548  549  552  556  557  563  565  567 
##   no   no   no   no   no  yes   no  yes  yes   no   no   no   no   no   no   no 
##  584  586  590  591  595  598  602  604  605  606  607  608  614  618  619  625 
##   no   no   no   no   no   no  yes   no   no  yes   no   no  yes   no   no   no 
##  628  629  633  635  636  645  646  650  661  662  664  667  668  670  671  672 
##   no   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no   no 
##  673  674  675  677  679  687  692  694  695  697  698  699  701  703  706  707 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  709  711  712  713  718  721  723  729  730  732  733  735  737  740  741  744 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
##  750  752  753  754  755  758  759  766  770  772  779  781  791  792  794  796 
##   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no   no 
##  797  799  802  811  812  823  825  826  834  836  837  847  855  857  859  860 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
##  864  870  873  881  890  893  897  899  904  905  906  908  910  911  913  916 
##   no   no   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no 
##  924  925  927  929  930  931  933  942  948  949  950  951  954  956  959  967 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  976  984  985  988  989  990  996 1002 1003 1005 1017 1018 1019 1020 1027 1029 
##  yes   no   no  yes   no  yes   no   no   no   no   no   no   no   no   no   no 
## 1031 1032 1040 1041 1043 1048 1049 1056 1062 1063 1064 1066 1068 1073 1075 1077 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1079 1080 1082 1085 1089 1094 1095 1100 1108 1112 1118 1121 1122 1125 1128 1131 
##  yes   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 1139 1144 1148 1155 1158 1161 1164 1166 1169 1171 1173 1175 1176 1181 1183 1185 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 1191 1192 1196 1204 1208 1209 1215 1220 1233 1235 1241 1244 1245 1247 1250 1255 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 1258 1261 1262 1264 1266 1267 1268 1271 1272 1273 1274 1285 1286 1292 1293 1294 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 1298 1301 1305 1308 1309 1310 1312 1313 1322 1323 1328 1340 1341 1345 1346 1351 
##   no   no   no   no   no   no   no   no   no  yes   no  yes   no  yes  yes  yes 
## 1352 1355 1359 1363 1366 1367 1375 1376 1379 1380 1381 1383 1384 1385 1387 1394 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1405 1408 1412 1416 1420 1423 1424 1425 1430 1436 1446 1448 1451 1453 1457 1461 
##   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 1464 1466 1468 1476 1477 1480 1482 1486 1489 1490 1494 1497 1500 1501 1502 1505 
##   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 1510 1511 1514 1515 1516 1519 1521 1524 1528 1529 1532 1533 1534 1535 1541 1544 
##   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no 
## 1547 1548 1549 1555 1557 1558 1562 1565 1566 1577 1582 1583 1584 1588 1590 1592 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1594 1601 1607 1615 1616 1624 1625 1626 1628 1629 1635 1640 1643 1644 1648 1651 
##  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 1652 1653 1655 1657 1658 1661 1665 1666 1668 1669 1671 1678 1681 1691 1693 1694 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 1695 1701 1703 1715 1718 1723 1724 1726 1727 1729 1744 1745 1749 1753 1757 1759 
##  yes   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 1764 1765 1766 1770 1771 1782 1785 1787 1791 1794 1795 1797 1801 1807 1815 1821 
##   no  yes  yes   no   no   no  yes   no   no   no  yes   no   no   no   no   no 
## 1822 1824 1829 1833 1834 1839 1840 1841 1845 1851 1854 1855 1857 1860 1863 1864 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1866 1869 1872 1873 1875 1881 1885 1889 1891 1892 1894 1896 1897 1899 1901 1904 
##  yes   no   no   no   no  yes   no   no   no   no  yes   no   no  yes   no  yes 
## 1905 1916 1917 1925 1930 1931 1933 1944 1947 1948 1949 1958 1960 1973 1974 1976 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1977 1981 1984 1990 1993 1994 1998 2003 2005 2007 2008 2013 2016 2021 2025 2028 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2029 2030 2037 2041 2042 2045 2047 2048 2049 2051 2053 2056 2060 2063 2065 2067 
##  yes  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 2071 2075 2077 2079 2080 2081 2084 2086 2088 2095 2100 2102 2103 2104 2114 2120 
##   no   no  yes   no   no   no   no   no   no   no  yes   no   no  yes  yes  yes 
## 2124 2126 2132 2133 2134 2135 2136 2145 2146 2147 2149 2153 2159 2161 2164 2169 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes  yes  yes   no 
## 2172 2176 2177 2179 2182 2184 2192 2193 2202 2203 2211 2214 2216 2217 2218 2222 
##   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no 
## 2223 2228 2230 2240 2241 2243 2246 2248 2249 2253 2254 2258 2261 2262 2269 2270 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2273 2275 2277 2283 2284 2287 2289 2290 2291 2294 2296 2298 2300 2302 2305 2307 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 2312 2314 2322 2324 2329 2331 2333 2334 2338 2340 2341 2356 2365 2366 2367 2371 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2372 2373 2382 2388 2393 2395 2396 2397 2399 2400 2401 2402 2404 2405 2406 2407 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 2410 2416 2417 2418 2420 2423 2424 2425 2426 2428 2431 2433 2437 2439 2440 2443 
##   no  yes   no   no  yes   no   no   no   no   no   no  yes   no   no   no   no 
## 2449 2450 2452 2453 2456 2459 2461 2464 2469 2470 2480 2485 2489 2490 2496 2498 
##   no   no  yes   no   no   no   no   no  yes   no  yes   no   no   no   no   no 
## 2499 2502 2504 2506 2511 2512 2516 2517 2519 2526 2527 2532 2533 2536 2537 2539 
##   no   no   no   no   no   no  yes   no   no   no  yes   no   no   no  yes   no 
## 2541 2542 2543 2545 2547 2548 2552 2554 2557 2559 2560 2565 2566 2572 2579 2581 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2585 2592 2595 2597 2602 2611 2613 2615 2616 2620 2622 2626 2630 2634 2635 2641 
##   no  yes  yes   no   no   no   no  yes   no  yes   no   no   no   no   no   no 
## 2643 2644 2647 2648 2662 2663 2671 2674 2678 2691 2692 2696 2700 2702 2706 2707 
##   no   no   no  yes   no  yes   no   no  yes   no   no   no   no   no   no   no 
## 2717 2722 2730 2735 2743 2755 2759 2762 2764 2766 2768 2769 2771 2779 2784 2787 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 2791 2796 2806 2807 2808 2810 2817 2820 2821 2823 2826 2829 2830 2834 2839 2841 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 2844 2845 2846 2847 2851 2852 2861 2865 2875 2878 2879 2884 2885 2889 2891 2897 
##   no   no   no   no   no   no   no   no  yes   no   no   no  yes   no   no   no 
## 2903 2907 2910 2923 2937 2940 2941 2943 2944 2945 2949 2950 2963 2966 2973 2974 
##   no   no   no   no  yes   no   no  yes   no   no   no   no   no   no   no   no 
## 2975 2977 2981 2983 2985 2990 2991 2996 2997 2998 2999 3000 3002 3008 3013 3019 
##   no   no  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 3021 3022 3024 3026 3027 3029 3031 3032 3037 3039 3040 3046 3047 3048 3050 3052 
##   no   no  yes   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3054 3057 3058 3061 3069 3070 3071 3075 3076 3078 3079 3086 3092 3094 3097 3100 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 3105 3108 3109 3110 3116 3118 3123 3127 3129 3135 3137 3138 3140 3141 3145 3151 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no 
## 3154 3155 3159 3165 3166 3168 3171 3173 3183 3187 3188 3189 3191 3197 3199 3209 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 3211 3212 3217 3221 3224 3227 3228 3233 3242 3243 3246 3247 3253 3254 3261 3266 
##   no   no   no   no   no   no   no   no  yes   no   no  yes   no   no   no  yes 
## 3268 3269 3274 3276 3278 3282 3283 3284 3288 3290 3292 3294 3295 3299 3302 3304 
##   no  yes   no   no   no   no   no   no   no   no  yes   no   no   no  yes   no 
## 3311 3316 3319 3321 3326 3330 3333 3336 3338 3340 3343 3347 3349 3355 3357 3358 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 3363 3364 3368 3371 3373 3377 3378 3384 3385 3387 3397 3400 3404 3405 3406 3413 
##   no   no   no   no  yes  yes   no   no   no   no   no   no   no   no   no   no 
## 3418 3425 3426 3428 3429 3433 3446 3451 3452 3456 3462 3465 3467 3468 3475 3476 
##   no   no   no   no  yes   no   no   no   no   no  yes   no   no   no   no   no 
## 3477 3483 3484 3485 3486 3490 3496 3497 3500 3502 3508 3509 3511 3519 3525 3534 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3537 3542 3543 3544 3545 3551 3552 3553 3558 3562 3564 3565 3570 3581 3584 3586 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 3589 3590 3591 3592 3593 3599 3600 3602 3606 3608 3616 3618 3620 3622 3623 3628 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3633 3640 3645 3647 3651 3656 3658 3659 3664 3665 3666 3667 3668 3673 3678 3679 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 3696 3701 3703 3704 3705 3708 3712 3716 3718 3720 3722 3724 3726 3727 3731 3732 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 3733 3735 3741 3748 3750 3754 3757 3758 3759 3760 3761 3762 3764 3765 3767 3768 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3770 3773 3776 3777 3778 3780 3786 3791 3792 3793 3798 3800 3804 3807 3810 3812 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3817 3822 3827 3828 3835 3836 3843 3851 3860 3864 3872 3876 3878 3885 3888 3889 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3899 3901 3904 3911 3914 3915 3917 3921 3935 3936 3942 3945 3957 3960 3968 3969 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3977 3981 3982 3984 3985 3989 3994 3998 4000 4001 4009 4010 4016 4018 4019 4021 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no 
## 4022 4026 4027 4030 4031 4032 4035 4038 4039 4042 4043 4044 4047 4051 4056 4059 
##  yes   no   no   no  yes   no   no   no   no  yes   no   no   no   no   no   no 
## 4060 4062 4069 4070 4084 4085 4093 4097 4100 4101 4103 4106 4107 4109 4113 4114 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4116 4120 4126 4127 4128 4130 4135 4136 4147 4148 4149 4159 4161 4164 4168 4171 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 4173 4175 4176 4180 4183 4185 4187 4192 4199 4204 4208 4209 4215 4218 4220 4221 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4225 4229 4231 4250 4261 4269 4280 4285 4286 4290 4291 4293 4300 4301 4305 4311 
##   no   no  yes   no   no  yes   no   no  yes   no   no  yes   no   no   no   no 
## 4323 4324 4325 4330 4334 4335 4339 4340 4341 4345 4346 4350 4351 4352 4353 4354 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 4360 4362 4363 4366 4373 4384 4385 4388 4397 4398 4401 4405 4413 4414 4416 4417 
##   no   no   no   no   no  yes   no   no   no   no   no  yes   no   no   no   no 
## 4421 4422 4426 4430 4431 4433 4434 4435 4436 4438 4441 4443 4447 4451 4452 4454 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4456 4463 4464 4466 4469 4470 4471 4477 4479 4483 4486 4487 4489 4494 4495 4497 
##   no   no   no  yes   no   no   no  yes   no   no   no   no  yes   no   no   no 
## 4499 4500 4501 4514 4518 4522 4527 4531 4536 4538 4540 4542 4543 4544 4545 4546 
##   no   no   no  yes   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4547 4556 4560 4563 4567 4572 4576 4583 4586 4588 4589 4595 4597 4600 4602 4605 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no 
## 4607 4608 4611 4612 4621 4623 4624 4626 4627 4632 4645 4648 4653 4659 4662 4664 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 4665 4666 4672 4676 4678 4679 4680 4683 4684 4686 4688 4695 4697 4700 4704 4713 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 4714 4715 4722 4724 4725 4729 4730 4743 4745 4747 4749 4753 4754 4762 4764 4766 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4770 4775 4779 4783 4784 4785 4787 4791 4792 4795 4799 4801 4804 4806 4809 4817 
##   no   no  yes   no   no   no   no   no   no   no  yes   no  yes   no   no   no 
## 4818 4819 4825 4828 4832 4837 4840 4854 4857 4862 4869 4875 4878 4879 4881 4885 
##   no   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no 
## 4889 4895 4896 4899 4900 4905 4910 4915 4917 4923 4928 4935 4937 4943 4948 4949 
##   no   no  yes  yes   no   no   no  yes   no   no   no   no   no   no   no  yes 
## 4950 4951 4954 4962 4963 4967 4972 4983 4988 4992 4995 4996 
##   no   no  yes   no   no   no   no   no   no  yes   no   no 
## Levels: no yes
```

```r
ran_pred1 <-data.frame(ran_pred)

table(testData$churn ,ran_pred) # confusion matrix
```

```
##      ran_pred
##         no  yes
##   no  1279    9
##   yes   54  158
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
## international_plan            71.6932332 81.3987711            87.014378
## voice_mail_plan               20.9515776 24.5785130            24.984931
## number_vmail_messages         19.9255993 22.9657358            22.899887
## total_day_minutes             32.6244696 33.7920933            40.799852
## total_day_calls               -0.8835058 -1.1140371            -1.253076
## total_day_charge              33.6441459 35.2598871            43.093310
## total_eve_minutes             24.6458363 25.5232923            29.080855
## total_eve_calls               -3.1220490  0.3106296            -2.720857
## total_eve_charge              25.0020458 24.6405063            29.900750
## total_night_minutes           18.3185206  7.6449983            19.522090
## total_night_calls             -1.1011505 -2.3065358            -1.977344
## total_night_charge            18.5054981  6.0111139            19.508196
## total_intl_minutes            23.5544749 16.5305444            26.548789
## total_intl_calls              33.2159496 45.9183681            46.910670
## total_intl_charge             23.9064600 19.0399018            28.318997
## number_customer_service_calls 70.5102544 91.5325919            93.490019
##                               MeanDecreaseGini
## international_plan                    72.72070
## voice_mail_plan                       19.63045
## number_vmail_messages                 23.67561
## total_day_minutes                    123.05950
## total_day_calls                       23.96466
## total_day_charge                     128.20254
## total_eve_minutes                     60.46191
## total_eve_calls                       23.08926
## total_eve_charge                      58.72516
## total_night_minutes                   34.68757
## total_night_calls                     25.36447
## total_night_charge                    34.60278
## total_intl_minutes                    36.21966
## total_intl_calls                      50.10928
## total_intl_charge                     37.56120
## number_customer_service_calls         95.47742
```

```r
# Variables that result in nodes with higher purity have a higher decrease in Gini coefficient.
varImpPlot(random_model)
```

<img src="07-decisiontree_files/figure-html/project-2.png" width="672" />

```r
#total day charges have higher decrease in gini index so its more significant than other
```

