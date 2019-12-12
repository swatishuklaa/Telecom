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
 DataQualityReport$UniqueRecords[i]<-length(unique(Telecom[,i]))
  DataQualityReport$DataAvailable[i]<-sum(!is.na(Telecom[,i]))
  DataQualityReport$AvailablePercent[i]<- round(100*(DataQualityReport$DataAvailable[i]/DataQualityReport$NoOfRecords[i]),2)
  DataQualityReport$Missing[i]<- sum(is.na(Telecom[,i]))
  DataQualityReport$MissingPercent[i]<- round(100*(DataQualityReport$Missing[i]/DataQualityReport$NoOfRecords[i]),2)
  
  if(DataQualityReport$DataType[i]== "numeric" ||  DataQualityReport$DataType[i] =="integer"){
    DataQualityReport$Minimum[i] <- min(Telecom[,i],na.rm = TRUE)
    DataQualityReport$Maximum[i] <- max(Telecom[,i],na.rm = TRUE)
    DataQualityReport$Mean[i]  <- mean(Telecom[,i],na.rm = TRUE)
    
  }
  else{
    DataQualityReport$Minimum[i] <- 0
    DataQualityReport$Maximum[i] <- 0
    DataQualityReport$Mean[i] <-0
  }
}
   
#Creating an empty quant data frame with column names given

quant<-data.frame(matrix(ncol = 8,nrow = 0))
colnames(quant)<-c("5thPercentile","10thPercentile","25thPercentile","50thPercentile",
                   "75thPercentile","90thPercentile","95thPercentile","99thPercentile")

# Calculating quantile for each variable then transforming it assigning to quant data frame
for(i in 1:length(Telecom)){
  if(DataQualityReport$DataType[i]== "numeric" ||  DataQualityReport$DataType[i] =="integer"){
    quant[i,] <- t(apply(Telecom[i],2,quantile,
                         probs=c(0.05,0.10,0.25,0.50,0.75,0.90,0.95,0.99),
                         na.rm = TRUE))
  }
  else{
    quant[i,]<-0
  }
}

class(quant)

#combining the DataQualityReport and quant dataframne column wise to get final result

DataQualityReport<- cbind(DataQualityReport, quant)

#Removing TEmporary variable created during data quality report
rm(quant)
rm(i)

class(DataQualityReport)
 

