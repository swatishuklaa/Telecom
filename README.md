# Telecom 

#Checking the current working directory and then changing it

getwd()
setwd("C:\\Users\\Dataset")

options(scipen = 999)

#Loading the required library

library(dplyr)
library(lazyeval)
library(rpart)
library(rpart.plot)
library(car)


#Reading the telecomfinal.csv into Telecom data set

Telecom<-read.csv("telecomfinal.csv",stringsAsFactors = FALSE)
str(Telecom)

#----------Creating the Data Quality Report ---------#

# Extracting variable names for each variable into VariableName and then converting it into dataframe

VariableName<-names(Telecom)
DataQualityReport<-as.data.frame(VariableName)
rm(VariableName)

#Extracting the data type and Total no of records for each variable and adding them to dataframe

DataQualityReport$DataType<-sapply(Telecom,class)
DataQualityReport$NoOfRecords<-nrow(Telecom)

#Getting unique records, data availbale, available percent, missing, missing percent, 
# maximum, minimum and mean for each variable  

for(i in 1:ncol(Telecom)){

}
   
