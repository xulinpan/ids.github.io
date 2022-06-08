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
##    2) total_day_minutes< 264.7 3277 359 no (0.89044858 0.10955142)  
##      4) number_customer_service_calls< 3.5 3026 235 no (0.92233972 0.07766028)  
##        8) international_plan=no 2740 137 no (0.95000000 0.05000000)  
##         16) total_day_minutes< 221.8 2300  65 no (0.97173913 0.02826087) *
##         17) total_day_minutes>=221.8 440  72 no (0.83636364 0.16363636)  
##           34) total_eve_minutes< 259.8 388  36 no (0.90721649 0.09278351) *
##           35) total_eve_minutes>=259.8 52  16 yes (0.30769231 0.69230769)  
##             70) voice_mail_plan=yes 9   0 no (1.00000000 0.00000000) *
##             71) voice_mail_plan=no 43   7 yes (0.16279070 0.83720930) *
##        9) international_plan=yes 286  98 no (0.65734266 0.34265734)  
##         18) total_intl_calls>=2.5 238  50 no (0.78991597 0.21008403)  
##           36) total_intl_minutes< 13.05 196   8 no (0.95918367 0.04081633) *
##           37) total_intl_minutes>=13.05 42   0 yes (0.00000000 1.00000000) *
##         19) total_intl_calls< 2.5 48   0 yes (0.00000000 1.00000000) *
##      5) number_customer_service_calls>=3.5 251 124 no (0.50597610 0.49402390)  
##       10) total_day_minutes>=160.25 154  39 no (0.74675325 0.25324675)  
##         20) total_eve_minutes>=141.45 136  27 no (0.80147059 0.19852941)  
##           40) total_day_minutes>=185.7 92   9 no (0.90217391 0.09782609) *
##           41) total_day_minutes< 185.7 44  18 no (0.59090909 0.40909091)  
##             82) total_eve_minutes>=199.9 28   4 no (0.85714286 0.14285714) *
##             83) total_eve_minutes< 199.9 16   2 yes (0.12500000 0.87500000) *
##         21) total_eve_minutes< 141.45 18   6 yes (0.33333333 0.66666667) *
##       11) total_day_minutes< 160.25 97  12 yes (0.12371134 0.87628866) *
##    3) total_day_minutes>=264.7 223  87 yes (0.39013453 0.60986547)  
##      6) voice_mail_plan=yes 55   6 no (0.89090909 0.10909091) *
##      7) voice_mail_plan=no 168  38 yes (0.22619048 0.77380952)  
##       14) total_eve_minutes< 150.35 22   2 no (0.90909091 0.09090909) *
##       15) total_eve_minutes>=150.35 146  18 yes (0.12328767 0.87671233) *
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
##    2    9   11   13   19   35   38   41   47   50   54   56   64   65   71   73 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
##   77   78   83   85   87   88   89   93   94   97  105  107  113  115  120  123 
##  yes  yes   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no 
##  128  140  141  148  150  152  155  160  165  166  167  168  169  172  174  177 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
##  179  183  187  192  193  197  198  202  209  210  212  223  224  231  234  235 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
##  238  242  244  249  252  254  255  257  258  259  260  261  267  268  270  276 
##   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  279  280  290  292  297  298  301  309  310  315  316  317  324  325  327  328 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  335  340  341  342  343  345  346  348  351  352  353  355  359  362  366  371 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no  yes   no 
##  373  375  377  379  381  384  386  393  396  397  399  403  410  411  416  421 
##  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no 
##  437  440  450  451  457  458  459  461  465  467  468  475  477  478  482  485 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  488  489  492  494  495  496  497  498  500  502  506  509  516  522  523  525 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no 
##  528  530  532  533  534  536  537  539  545  547  549  554  555  557  560  563 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  570  571  573  575  581  588  593  595  600  603  606  610  616  618  624  628 
##  yes   no   no  yes   no   no   no   no   no   no  yes   no   no   no   no   no 
##  631  632  636  640  641  653  655  657  658  660  661  662  663  664  670  672 
##   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  674  676  687  688  690  692  695  696  697  698  707  718  721  723  724  727 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
##  735  736  740  744  745  752  753  758  763  764  767  773  777  779  780  781 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes 
##  787  788  790  797  805  807  810  811  819  825  828  829  836  842  843  848 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  850  851  860  864  866  868  870  872  877  878  884  888  892  895  900  902 
##   no   no  yes   no   no   no   no  yes   no  yes   no   no   no  yes   no  yes 
##  904  907  908  912  914  915  918  922  936  941  942  945  946  956  959  963 
##   no   no   no  yes   no  yes   no   no   no   no   no  yes   no   no   no   no 
##  970  973  982  984  985  991  995  998 1002 1003 1004 1014 1019 1022 1029 1032 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1034 1036 1037 1041 1046 1047 1048 1050 1052 1055 1061 1064 1066 1069 1070 1071 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no 
## 1075 1077 1078 1085 1088 1096 1097 1103 1105 1119 1124 1126 1127 1132 1133 1134 
##   no   no  yes   no   no  yes   no  yes   no   no   no   no   no   no   no  yes 
## 1135 1137 1140 1144 1146 1147 1154 1156 1159 1162 1163 1167 1168 1171 1173 1179 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 1184 1188 1191 1197 1204 1205 1209 1210 1213 1215 1217 1218 1235 1237 1241 1246 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no  yes   no 
## 1249 1254 1255 1256 1258 1259 1260 1266 1269 1273 1274 1285 1288 1300 1301 1305 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no  yes   no   no 
## 1308 1312 1317 1323 1325 1331 1340 1345 1346 1347 1348 1352 1356 1358 1360 1373 
##   no   no   no  yes   no   no  yes  yes  yes  yes   no   no   no   no   no   no 
## 1374 1379 1383 1385 1386 1392 1393 1397 1401 1408 1412 1414 1415 1418 1421 1422 
##  yes   no   no   no   no   no  yes   no   no  yes   no   no   no   no  yes   no 
## 1425 1426 1427 1431 1434 1437 1444 1448 1457 1458 1459 1464 1465 1467 1478 1479 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1480 1482 1490 1496 1497 1501 1505 1509 1512 1514 1520 1524 1525 1526 1529 1531 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1533 1534 1536 1537 1538 1539 1540 1546 1548 1549 1551 1556 1558 1560 1562 1568 
##   no  yes   no   no  yes  yes   no   no   no  yes   no   no   no   no   no   no 
## 1577 1582 1583 1584 1585 1587 1588 1589 1592 1593 1599 1603 1607 1618 1621 1625 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1629 1632 1633 1640 1641 1646 1648 1652 1654 1655 1657 1664 1665 1667 1668 1670 
##   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no   no 
## 1671 1681 1688 1689 1691 1695 1701 1704 1706 1707 1714 1715 1727 1731 1733 1738 
##   no   no   no   no   no  yes   no   no   no   no  yes   no   no   no   no   no 
## 1739 1741 1745 1746 1747 1748 1749 1754 1755 1757 1758 1759 1760 1767 1771 1773 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 1774 1776 1784 1787 1790 1793 1795 1797 1799 1804 1816 1817 1824 1827 1829 1832 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no  yes 
## 1833 1834 1836 1839 1840 1843 1844 1845 1846 1851 1853 1855 1857 1865 1866 1867 
##   no   no  yes   no   no  yes   no  yes  yes   no   no   no   no   no  yes   no 
## 1870 1872 1879 1888 1889 1890 1891 1892 1894 1898 1902 1904 1916 1917 1918 1919 
##  yes   no  yes  yes   no   no   no   no   no   no   no  yes   no   no   no   no 
## 1922 1923 1928 1929 1930 1934 1938 1939 1941 1943 1944 1953 1954 1955 1957 1961 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no  yes   no   no 
## 1964 1969 1974 1975 1977 1980 1981 1983 1988 1991 1993 1996 2005 2006 2009 2010 
##   no   no   no  yes   no   no  yes   no   no   no   no   no   no   no   no   no 
## 2014 2017 2018 2019 2031 2032 2038 2039 2043 2050 2051 2054 2060 2064 2066 2070 
##   no   no   no   no   no  yes   no  yes   no   no   no   no  yes   no   no   no 
## 2071 2076 2082 2083 2087 2089 2090 2094 2103 2104 2105 2106 2107 2108 2113 2115 
##   no   no   no   no   no   no   no   no   no  yes   no   no  yes  yes  yes   no 
## 2120 2123 2127 2132 2136 2142 2144 2146 2148 2154 2155 2157 2162 2164 2165 2169 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes  yes   no 
## 2172 2174 2179 2180 2183 2185 2191 2193 2195 2196 2198 2215 2216 2217 2219 2220 
##   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no 
## 2222 2223 2226 2239 2246 2248 2250 2254 2255 2260 2263 2271 2275 2281 2282 2287 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 2300 2301 2306 2310 2312 2314 2318 2325 2326 2327 2331 2335 2340 2344 2345 2346 
##   no   no   no   no   no   no   no  yes  yes   no   no   no   no  yes   no   no 
## 2350 2358 2361 2363 2367 2369 2372 2376 2378 2379 2380 2383 2386 2388 2391 2398 
##   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no  yes 
## 2403 2407 2408 2421 2422 2424 2426 2427 2430 2431 2435 2436 2438 2442 2445 2446 
##  yes   no  yes  yes  yes   no   no   no   no   no   no   no  yes   no   no   no 
## 2449 2451 2455 2462 2463 2467 2468 2470 2472 2477 2482 2484 2487 2488 2490 2493 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2510 2513 2518 2519 2526 2527 2529 2531 2532 2537 2541 2542 2549 2554 2561 2567 
##   no   no   no   no   no  yes   no  yes   no  yes   no   no   no   no   no   no 
## 2572 2573 2577 2578 2579 2580 2589 2591 2592 2597 2598 2604 2616 2625 2628 2632 
##   no  yes   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 2634 2635 2636 2638 2641 2644 2645 2653 2654 2662 2667 2668 2669 2670 2672 2673 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes 
## 2680 2684 2685 2688 2696 2705 2714 2715 2717 2724 2727 2728 2732 2733 2737 2741 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 2745 2748 2752 2753 2754 2755 2766 2767 2768 2769 2771 2773 2774 2783 2790 2801 
##   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes 
## 2803 2813 2817 2818 2823 2831 2835 2837 2844 2853 2854 2857 2865 2867 2868 2876 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2878 2880 2883 2886 2892 2894 2897 2902 2919 2922 2924 2927 2929 2932 2933 2940 
##   no   no  yes   no   no   no   no  yes   no   no   no  yes   no   no   no   no 
## 2945 2950 2952 2953 2954 2956 2957 2969 2970 2976 2978 2979 2982 2991 2999 3003 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes 
## 3008 3010 3011 3012 3015 3022 3024 3027 3031 3033 3042 3046 3050 3051 3056 3058 
##   no   no   no   no   no   no  yes   no   no   no   no  yes   no  yes   no   no 
## 3060 3061 3062 3063 3066 3069 3070 3075 3076 3077 3080 3082 3085 3086 3088 3097 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 3098 3101 3105 3106 3108 3109 3112 3122 3124 3129 3137 3139 3140 3142 3144 3145 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 3148 3154 3155 3157 3160 3161 3166 3168 3169 3171 3172 3174 3176 3178 3181 3182 
##  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 3191 3198 3199 3205 3208 3214 3216 3217 3219 3222 3223 3224 3236 3242 3245 3247 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no  yes   no  yes 
## 3248 3253 3254 3255 3261 3269 3270 3284 3288 3291 3292 3295 3300 3301 3304 3305 
##  yes   no   no   no   no  yes   no   no  yes   no  yes   no   no   no   no   no 
## 3306 3310 3311 3312 3314 3317 3321 3322 3323 3325 3330 3335 3338 3340 3341 3343 
##   no   no   no   no   no   no  yes   no  yes   no   no   no   no   no   no   no 
## 3344 3347 3352 3354 3356 3360 3361 3362 3363 3364 3365 3366 3370 3371 3374 3375 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 3377 3380 3388 3392 3394 3395 3405 3406 3409 3411 3415 3418 3419 3420 3423 3424 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3431 3432 3437 3439 3447 3448 3453 3455 3457 3461 3465 3473 3474 3476 3478 3482 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3486 3491 3493 3494 3500 3502 3503 3508 3514 3516 3517 3519 3529 3534 3537 3545 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3548 3554 3559 3572 3573 3583 3600 3603 3604 3605 3608 3609 3612 3615 3619 3621 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 3622 3623 3624 3629 3634 3635 3640 3645 3648 3653 3655 3656 3659 3661 3662 3663 
##   no   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no 
## 3664 3668 3671 3672 3680 3681 3685 3687 3695 3699 3703 3706 3712 3713 3715 3716 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 3721 3722 3725 3727 3729 3733 3734 3735 3736 3741 3747 3754 3756 3760 3761 3763 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3764 3765 3768 3769 3770 3771 3774 3777 3778 3781 3784 3786 3789 3790 3794 3796 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3798 3800 3801 3803 3805 3814 3815 3817 3836 3839 3845 3852 3855 3856 3861 3862 
##   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no   no   no 
## 3865 3872 3875 3876 3877 3879 3881 3883 3885 3886 3888 3890 3892 3893 3894 3896 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3899 3905 3911 3913 3916 3917 3919 3927 3932 3933 3937 3938 3942 3951 3953 3956 
##   no   no   no   no   no  yes   no   no  yes   no   no   no   no   no   no   no 
## 3959 3960 3966 3970 3973 3975 3980 3984 3985 3989 3990 3991 3992 3997 3999 4001 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4002 4012 4013 4021 4024 4027 4030 4034 4035 4037 4039 4041 4042 4053 4056 4063 
##   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no  yes 
## 4064 4067 4070 4072 4075 4077 4082 4083 4084 4089 4090 4092 4103 4108 4112 4115 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4116 4119 4125 4133 4135 4142 4144 4145 4146 4147 4150 4151 4153 4154 4155 4163 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no  yes 
## 4166 4178 4183 4185 4186 4189 4190 4192 4201 4202 4210 4213 4216 4217 4219 4222 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 4223 4234 4241 4242 4247 4252 4253 4256 4257 4260 4261 4266 4270 4272 4274 4279 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4283 4284 4287 4289 4290 4292 4294 4297 4300 4301 4302 4307 4311 4318 4321 4328 
##   no  yes   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 4329 4335 4337 4339 4340 4346 4351 4354 4364 4368 4370 4373 4374 4377 4382 4386 
##  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 4389 4391 4392 4395 4398 4399 4401 4404 4405 4407 4416 4425 4428 4433 4436 4438 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no  yes 
## 4439 4441 4445 4447 4454 4455 4456 4457 4458 4460 4462 4466 4467 4479 4485 4489 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 4494 4498 4500 4502 4511 4522 4523 4524 4527 4530 4537 4538 4542 4545 4551 4561 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 4562 4566 4567 4571 4576 4581 4583 4584 4597 4598 4601 4603 4607 4608 4616 4619 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4620 4621 4623 4625 4626 4629 4630 4632 4636 4638 4641 4647 4654 4655 4656 4657 
##   no   no   no   no   no  yes   no  yes   no   no   no   no   no   no   no   no 
## 4660 4666 4673 4681 4683 4689 4690 4691 4692 4696 4699 4700 4703 4708 4711 4714 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 4721 4722 4726 4730 4731 4736 4739 4740 4741 4742 4743 4745 4747 4752 4755 4757 
##   no  yes   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 4758 4763 4767 4770 4772 4774 4776 4778 4779 4787 4790 4795 4803 4804 4805 4809 
##   no   no   no   no   no  yes   no   no  yes   no   no   no   no  yes   no   no 
## 4813 4815 4817 4820 4821 4826 4829 4833 4836 4837 4839 4840 4845 4849 4860 4861 
##   no   no   no   no   no  yes   no  yes   no   no   no   no   no   no   no   no 
## 4862 4863 4865 4867 4872 4876 4878 4879 4881 4882 4884 4893 4897 4898 4899 4901 
##  yes  yes   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no 
## 4902 4904 4911 4918 4919 4920 4921 4924 4932 4933 4941 4945 4949 4963 4970 4971 
##   no   no   no  yes  yes   no   no   no   no   no   no   no  yes   no   no   no 
## 4973 4976 4979 4981 4982 4985 4986 4987 4988 4992 4998 5000 
##   no   no   no   no   no   no   no   no   no  yes   no   no 
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
##   no  1265   23
##   yes   60  152
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
##         OOB estimate of  error rate: 4.34%
## Confusion matrix:
##       no yes class.error
## no  2979  26 0.008652246
## yes  126 369 0.254545455
```

```r
#predicting the data based on the model developed
ran_pred <- predict(random_model,testData)
ran_pred
```

```
##    2    9   11   13   19   35   38   41   47   50   54   56   64   65   71   73 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
##   77   78   83   85   87   88   89   93   94   97  105  107  113  115  120  123 
##  yes  yes   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no 
##  128  140  141  148  150  152  155  160  165  166  167  168  169  172  174  177 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
##  179  183  187  192  193  197  198  202  209  210  212  223  224  231  234  235 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
##  238  242  244  249  252  254  255  257  258  259  260  261  267  268  270  276 
##   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  279  280  290  292  297  298  301  309  310  315  316  317  324  325  327  328 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  335  340  341  342  343  345  346  348  351  352  353  355  359  362  366  371 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no  yes   no 
##  373  375  377  379  381  384  386  393  396  397  399  403  410  411  416  421 
##  yes   no   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no 
##  437  440  450  451  457  458  459  461  465  467  468  475  477  478  482  485 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  488  489  492  494  495  496  497  498  500  502  506  509  516  522  523  525 
##   no   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no   no 
##  528  530  532  533  534  536  537  539  545  547  549  554  555  557  560  563 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
##  570  571  573  575  581  588  593  595  600  603  606  610  616  618  624  628 
##  yes   no   no  yes   no   no   no   no   no   no  yes   no   no   no   no   no 
##  631  632  636  640  641  653  655  657  658  660  661  662  663  664  670  672 
##   no   no  yes   no   no   no   no   no   no   no  yes   no   no   no   no   no 
##  674  676  687  688  690  692  695  696  697  698  707  718  721  723  724  727 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  735  736  740  744  745  752  753  758  763  764  767  773  777  779  780  781 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes 
##  787  788  790  797  805  807  810  811  819  825  828  829  836  842  843  848 
##   no  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
##  850  851  860  864  866  868  870  872  877  878  884  888  892  895  900  902 
##   no   no  yes   no   no   no   no  yes   no  yes   no   no   no  yes   no  yes 
##  904  907  908  912  914  915  918  922  936  941  942  945  946  956  959  963 
##   no   no   no  yes   no  yes   no   no   no   no   no  yes   no   no   no   no 
##  970  973  982  984  985  991  995  998 1002 1003 1004 1014 1019 1022 1029 1032 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1034 1036 1037 1041 1046 1047 1048 1050 1052 1055 1061 1064 1066 1069 1070 1071 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1075 1077 1078 1085 1088 1096 1097 1103 1105 1119 1124 1126 1127 1132 1133 1134 
##   no   no  yes   no   no  yes   no  yes   no   no   no   no   no   no   no  yes 
## 1135 1137 1140 1144 1146 1147 1154 1156 1159 1162 1163 1167 1168 1171 1173 1179 
##   no  yes   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 1184 1188 1191 1197 1204 1205 1209 1210 1213 1215 1217 1218 1235 1237 1241 1246 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no   no  yes   no 
## 1249 1254 1255 1256 1258 1259 1260 1266 1269 1273 1274 1285 1288 1300 1301 1305 
##   no   no   no   no   no   no   no   no   no   no  yes  yes   no  yes   no   no 
## 1308 1312 1317 1323 1325 1331 1340 1345 1346 1347 1348 1352 1356 1358 1360 1373 
##   no   no   no  yes   no   no  yes  yes  yes  yes   no   no   no   no   no   no 
## 1374 1379 1383 1385 1386 1392 1393 1397 1401 1408 1412 1414 1415 1418 1421 1422 
##  yes   no   no   no   no   no  yes   no   no  yes   no   no   no   no  yes   no 
## 1425 1426 1427 1431 1434 1437 1444 1448 1457 1458 1459 1464 1465 1467 1478 1479 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1480 1482 1490 1496 1497 1501 1505 1509 1512 1514 1520 1524 1525 1526 1529 1531 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 1533 1534 1536 1537 1538 1539 1540 1546 1548 1549 1551 1556 1558 1560 1562 1568 
##   no  yes   no   no  yes  yes   no   no   no   no   no   no   no   no   no   no 
## 1577 1582 1583 1584 1585 1587 1588 1589 1592 1593 1599 1603 1607 1618 1621 1625 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 1629 1632 1633 1640 1641 1646 1648 1652 1654 1655 1657 1664 1665 1667 1668 1670 
##   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no   no   no 
## 1671 1681 1688 1689 1691 1695 1701 1704 1706 1707 1714 1715 1727 1731 1733 1738 
##   no   no   no   no   no  yes   no   no   no   no  yes   no   no   no   no   no 
## 1739 1741 1745 1746 1747 1748 1749 1754 1755 1757 1758 1759 1760 1767 1771 1773 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 1774 1776 1784 1787 1790 1793 1795 1797 1799 1804 1816 1817 1824 1827 1829 1832 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 1833 1834 1836 1839 1840 1843 1844 1845 1846 1851 1853 1855 1857 1865 1866 1867 
##   no   no   no   no   no  yes   no   no  yes   no   no   no   no   no  yes   no 
## 1870 1872 1879 1888 1889 1890 1891 1892 1894 1898 1902 1904 1916 1917 1918 1919 
##  yes   no  yes  yes   no   no   no   no  yes   no   no  yes   no   no   no   no 
## 1922 1923 1928 1929 1930 1934 1938 1939 1941 1943 1944 1953 1954 1955 1957 1961 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no   no 
## 1964 1969 1974 1975 1977 1980 1981 1983 1988 1991 1993 1996 2005 2006 2009 2010 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 2014 2017 2018 2019 2031 2032 2038 2039 2043 2050 2051 2054 2060 2064 2066 2070 
##   no   no   no   no   no  yes   no  yes   no   no   no   no  yes   no   no  yes 
## 2071 2076 2082 2083 2087 2089 2090 2094 2103 2104 2105 2106 2107 2108 2113 2115 
##   no   no   no   no   no   no   no   no   no  yes   no   no  yes  yes  yes   no 
## 2120 2123 2127 2132 2136 2142 2144 2146 2148 2154 2155 2157 2162 2164 2165 2169 
##  yes   no   no   no   no   no   no   no  yes   no   no   no   no  yes  yes   no 
## 2172 2174 2179 2180 2183 2185 2191 2193 2195 2196 2198 2215 2216 2217 2219 2220 
##   no  yes   no   no   no   no   no   no   no   no   no   no  yes   no  yes   no 
## 2222 2223 2226 2239 2246 2248 2250 2254 2255 2260 2263 2271 2275 2281 2282 2287 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 2300 2301 2306 2310 2312 2314 2318 2325 2326 2327 2331 2335 2340 2344 2345 2346 
##   no   no   no   no   no   no   no  yes  yes   no   no   no   no  yes   no   no 
## 2350 2358 2361 2363 2367 2369 2372 2376 2378 2379 2380 2383 2386 2388 2391 2398 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 2403 2407 2408 2421 2422 2424 2426 2427 2430 2431 2435 2436 2438 2442 2445 2446 
##  yes   no  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no 
## 2449 2451 2455 2462 2463 2467 2468 2470 2472 2477 2482 2484 2487 2488 2490 2493 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 2510 2513 2518 2519 2526 2527 2529 2531 2532 2537 2541 2542 2549 2554 2561 2567 
##   no   no   no   no   no  yes   no   no   no  yes   no   no   no   no   no   no 
## 2572 2573 2577 2578 2579 2580 2589 2591 2592 2597 2598 2604 2616 2625 2628 2632 
##   no  yes   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 2634 2635 2636 2638 2641 2644 2645 2653 2654 2662 2667 2668 2669 2670 2672 2673 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no  yes 
## 2680 2684 2685 2688 2696 2705 2714 2715 2717 2724 2727 2728 2732 2733 2737 2741 
##   no   no   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no 
## 2745 2748 2752 2753 2754 2755 2766 2767 2768 2769 2771 2773 2774 2783 2790 2801 
##   no  yes   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes 
## 2803 2813 2817 2818 2823 2831 2835 2837 2844 2853 2854 2857 2865 2867 2868 2876 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 2878 2880 2883 2886 2892 2894 2897 2902 2919 2922 2924 2927 2929 2932 2933 2940 
##   no   no  yes   no   no   no   no  yes   no   no   no  yes   no   no   no   no 
## 2945 2950 2952 2953 2954 2956 2957 2969 2970 2976 2978 2979 2982 2991 2999 3003 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes 
## 3008 3010 3011 3012 3015 3022 3024 3027 3031 3033 3042 3046 3050 3051 3056 3058 
##   no   no   no   no   no   no  yes   no   no   no   no  yes   no  yes   no   no 
## 3060 3061 3062 3063 3066 3069 3070 3075 3076 3077 3080 3082 3085 3086 3088 3097 
##   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no 
## 3098 3101 3105 3106 3108 3109 3112 3122 3124 3129 3137 3139 3140 3142 3144 3145 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes 
## 3148 3154 3155 3157 3160 3161 3166 3168 3169 3171 3172 3174 3176 3178 3181 3182 
##   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no 
## 3191 3198 3199 3205 3208 3214 3216 3217 3219 3222 3223 3224 3236 3242 3245 3247 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no  yes   no  yes 
## 3248 3253 3254 3255 3261 3269 3270 3284 3288 3291 3292 3295 3300 3301 3304 3305 
##  yes   no   no   no   no  yes   no   no   no   no  yes   no   no   no   no  yes 
## 3306 3310 3311 3312 3314 3317 3321 3322 3323 3325 3330 3335 3338 3340 3341 3343 
##   no   no   no   no   no   no  yes   no  yes   no   no   no   no   no   no   no 
## 3344 3347 3352 3354 3356 3360 3361 3362 3363 3364 3365 3366 3370 3371 3374 3375 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 3377 3380 3388 3392 3394 3395 3405 3406 3409 3411 3415 3418 3419 3420 3423 3424 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3431 3432 3437 3439 3447 3448 3453 3455 3457 3461 3465 3473 3474 3476 3478 3482 
##   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no   no 
## 3486 3491 3493 3494 3500 3502 3503 3508 3514 3516 3517 3519 3529 3534 3537 3545 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3548 3554 3559 3572 3573 3583 3600 3603 3604 3605 3608 3609 3612 3615 3619 3621 
##   no   no   no   no   no   no   no   no   no  yes   no  yes   no   no   no   no 
## 3622 3623 3624 3629 3634 3635 3640 3645 3648 3653 3655 3656 3659 3661 3662 3663 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3664 3668 3671 3672 3680 3681 3685 3687 3695 3699 3703 3706 3712 3713 3715 3716 
##   no   no   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 3721 3722 3725 3727 3729 3733 3734 3735 3736 3741 3747 3754 3756 3760 3761 3763 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3764 3765 3768 3769 3770 3771 3774 3777 3778 3781 3784 3786 3789 3790 3794 3796 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3798 3800 3801 3803 3805 3814 3815 3817 3836 3839 3845 3852 3855 3856 3861 3862 
##   no   no   no   no   no   no   no   no   no   no   no  yes  yes   no   no   no 
## 3865 3872 3875 3876 3877 3879 3881 3883 3885 3886 3888 3890 3892 3893 3894 3896 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 3899 3905 3911 3913 3916 3917 3919 3927 3932 3933 3937 3938 3942 3951 3953 3956 
##   no   no   no   no   no  yes   no   no  yes   no   no   no   no   no   no  yes 
## 3959 3960 3966 3970 3973 3975 3980 3984 3985 3989 3990 3991 3992 3997 3999 4001 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4002 4012 4013 4021 4024 4027 4030 4034 4035 4037 4039 4041 4042 4053 4056 4063 
##   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no   no  yes 
## 4064 4067 4070 4072 4075 4077 4082 4083 4084 4089 4090 4092 4103 4108 4112 4115 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4116 4119 4125 4133 4135 4142 4144 4145 4146 4147 4150 4151 4153 4154 4155 4163 
##   no   no   no   no   no  yes   no   no   no   no   no   no   no  yes   no  yes 
## 4166 4178 4183 4185 4186 4189 4190 4192 4201 4202 4210 4213 4216 4217 4219 4222 
##  yes   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4223 4234 4241 4242 4247 4252 4253 4256 4257 4260 4261 4266 4270 4272 4274 4279 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no   no 
## 4283 4284 4287 4289 4290 4292 4294 4297 4300 4301 4302 4307 4311 4318 4321 4328 
##   no  yes   no   no   no   no   no  yes   no   no   no   no   no   no   no   no 
## 4329 4335 4337 4339 4340 4346 4351 4354 4364 4368 4370 4373 4374 4377 4382 4386 
##  yes   no   no   no   no  yes   no   no   no   no   no   no   no   no   no   no 
## 4389 4391 4392 4395 4398 4399 4401 4404 4405 4407 4416 4425 4428 4433 4436 4438 
##   no   no   no   no   no   no   no   no  yes  yes   no   no   no   no   no  yes 
## 4439 4441 4445 4447 4454 4455 4456 4457 4458 4460 4462 4466 4467 4479 4485 4489 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no  yes 
## 4494 4498 4500 4502 4511 4522 4523 4524 4527 4530 4537 4538 4542 4545 4551 4561 
##   no   no   no   no   no   no   no   no   no   no   no  yes   no   no   no   no 
## 4562 4566 4567 4571 4576 4581 4583 4584 4597 4598 4601 4603 4607 4608 4616 4619 
##   no   no   no   no   no   no   no   no   no  yes   no   no   no   no   no   no 
## 4620 4621 4623 4625 4626 4629 4630 4632 4636 4638 4641 4647 4654 4655 4656 4657 
##   no   no   no   no   no  yes   no   no   no   no  yes   no   no   no   no   no 
## 4660 4666 4673 4681 4683 4689 4690 4691 4692 4696 4699 4700 4703 4708 4711 4714 
##   no   no   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 4721 4722 4726 4730 4731 4736 4739 4740 4741 4742 4743 4745 4747 4752 4755 4757 
##   no  yes   no   no   no  yes   no   no  yes   no   no   no   no   no   no   no 
## 4758 4763 4767 4770 4772 4774 4776 4778 4779 4787 4790 4795 4803 4804 4805 4809 
##   no   no   no   no   no  yes   no   no  yes   no   no   no   no  yes   no   no 
## 4813 4815 4817 4820 4821 4826 4829 4833 4836 4837 4839 4840 4845 4849 4860 4861 
##   no   no   no   no   no  yes   no  yes   no   no   no   no   no   no   no   no 
## 4862 4863 4865 4867 4872 4876 4878 4879 4881 4882 4884 4893 4897 4898 4899 4901 
##  yes  yes   no   no   no   no   no   no   no   no   no   no   no   no  yes   no 
## 4902 4904 4911 4918 4919 4920 4921 4924 4932 4933 4941 4945 4949 4963 4970 4971 
##   no   no   no  yes  yes   no   no   no   no   no   no   no  yes   no   no   no 
## 4973 4976 4979 4981 4982 4985 4986 4987 4988 4992 4998 5000 
##   no   no   no   no   no   no   no   no   no  yes   no   no 
## Levels: no yes
```

```r
ran_pred1 <-data.frame(ran_pred)

table(testData$churn ,ran_pred) # confusion matrix
```

```
##      ran_pred
##         no  yes
##   no  1278   10
##   yes   46  166
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
## international_plan            71.9390672 78.5118830           87.1812848
## voice_mail_plan               21.1620471 24.0302199           24.8578864
## number_vmail_messages         15.8878044 21.3298153           20.3469411
## total_day_minutes             32.2875791 32.2407984           41.3799338
## total_day_calls               -1.3973147 -4.0167222           -3.0408053
## total_day_charge              32.4735513 32.7482818           41.2107028
## total_eve_minutes             23.9684849 23.6084321           28.1522627
## total_eve_calls                0.7679422 -0.4376756            0.5181431
## total_eve_charge              24.0847982 25.0431195           28.7734843
## total_night_minutes           19.3953772  7.5077996           20.8404415
## total_night_calls             -1.0771860 -1.4274444           -1.5108879
## total_night_charge            19.4483858  8.3151465           20.9849154
## total_intl_minutes            24.1705008 16.2238061           27.3140135
## total_intl_calls              35.3535304 43.3584148           46.4208667
## total_intl_charge             24.5737400 18.7334341           28.0382434
## number_customer_service_calls 70.4192652 99.7054629           99.4379502
##                               MeanDecreaseGini
## international_plan                    63.46379
## voice_mail_plan                       21.42020
## number_vmail_messages                 26.00416
## total_day_minutes                    124.52948
## total_day_calls                       22.93931
## total_day_charge                     126.56828
## total_eve_minutes                     58.70415
## total_eve_calls                       24.40578
## total_eve_charge                      60.60495
## total_night_minutes                   38.20237
## total_night_calls                     25.42663
## total_night_charge                    36.39348
## total_intl_minutes                    36.79688
## total_intl_calls                      47.19613
## total_intl_charge                     37.84441
## number_customer_service_calls         99.12227
```

```r
# Variables that result in nodes with higher purity have a higher decrease in Gini coefficient.
varImpPlot(random_model)
```

<img src="07-decisiontree_files/figure-html/project-2.png" width="672" />

```r
#total day charges have higher decrease in gini index so its more significant than other
```

