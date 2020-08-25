# tooth-growth-analysis-of-pigs

In this project ,we are  going to analyze the Toothgrowth dataset provided
 in the r package datasets. 
The dataset contains the data of growth of teeth
 by giving Vitamin c (VC) and orange juice (OJ). 
It also has 3 dosage quantity i-e 0.5mg, 1mg, 2mg.

 It is a project related to statistical inference and exploratory analysis using R language.

1. Loading the dataset and neccessary libraries
library(ggplot2)
data(ToothGrowth)
2. Basic summary and struture of dataset
head(ToothGrowth)
#    len supp dose
    1  4.2   VC  0.5
    2 11.5   VC  0.5
    3  7.3   VC  0.5
    4  5.8   VC  0.5
    5  6.4   VC  0.5
    6 10.0   VC  0.5
str(ToothGrowth)
# 'data.frame':    60 obs. of  3 variables:
#  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
# $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
#  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
summary(ToothGrowth)
#       len        supp         dose      
      Min.   : 4.20   OJ:30   Min.   :0.500  
      1st Qu.:13.07   VC:30   1st Qu.:0.500  
      Median :19.25           Median :1.000  
      Mean   :18.81           Mean   :1.167  
      3rd Qu.:25.27           3rd Qu.:2.000  
      Max.   :33.90           Max.   :2.000
dim(ToothGrowth)
# [1] 60  3
3. Basic exploratory Analysis
g<- ggplot(data=ToothGrowth, aes(x=paste(supp,dose) , y=len, fill=factor(dose)))
g<- g+geom_boxplot() 
g<- g+labs(title = "Tooth growth of 60 guinea pigs by dosage and delivery method of vitamin C", x = "Dose in milligrams/day", y = "Tooth Lengh")
g<- g+ scale_fill_discrete(name="dosage type")
g<- g+theme(plot.title = element_text(hjust = 0.5))
print(g)


4. Data Manipulation to separate data by dosage
data_0.5<-subset(ToothGrowth, dose==0.5)
data_1<-subset(ToothGrowth, dose==1)
data_2<-subset(ToothGrowth, dose==2)
5. Performing T test to accept or reject hypothesis
t1<-t.test(len~supp, data=data_0.5)
t2<-t.test(len~supp, data=data_1)
t3<-t.test(len~supp, data=data_2)
6. Creating DataFrame to analyze the result.
Conclusion<-data.frame("p-value"=c(t1$p.value,t2$p.value,t3$p.value),
                       "Low-Confidence"=c(t1$conf.int[1],t2$conf.int[1],t3$conf.int[1]),
                       "High-Confidence"=c(t1$conf.int[2],t2$conf.int[2],t3$conf.int[2]),
                       "OJ mean" = c(t1$estimate[1],t2$estimate[1],t3$estimate[1]),
                       "VC mean" = c(t1$estimate[2],t2$estimate[2],t3$estimate[2]),
                       row.names = c("Dosage_.5","Dosage_1","Dosage_2"))

Conclusion
##               p.value Low.Confidence High.Confidence OJ.mean VC.mean
      Dosage_.5 0.006358607       1.719057        8.780943   13.23    7.98
      Dosage_1  0.001038376       2.802148        9.057852   22.70   16.77
      Dosage_2  0.963851589      -3.798070        3.638070   26.06   26.14
