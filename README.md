#Purpose
The purpose of this project is to analyze and present the given data sets (Breweries.csv & Beer.csv). The code book that follows will describe the data sets, their variables, the questions, answers to those questions, and code used to obtain the answers.

##Datasets
File Name | Source | Description
----------|--------|------------
Beers.csv | Provided by professor | Dataset contains a list of 2410 US craft beers and specific characteristics
Breweries.csv | Provided by professor | Daaset contains a list of 558 US breweries and their locations

##Code
```r
#How many breweries are present in each state?
as.data.frame(table(Breweries$State))

#Merge beer data with the breweries data. Print the first 6 observations and the last six observations to check the merged file.
colnames(Breweries)[1] <- "Brewery_id" #change column name to use as merge variable
new.df <- merge(Beers, Breweries, by = "Brewery_id") #merge by brewery_id
names(new.df)[2] <- "Beer Name" #clean up column name for beer
names(new.df)[8] <- "Brewery Name" #clean up column name for brewery
head(new.df,6) #show first 6 entries
tail(new.df,6) #show last 6 entries

#Report the number of NA's in each column.
na_count <-sapply(new.df, function(y) sum(length(which(is.na(y))))) #use sapply to count NAs of each column
na_count <- as.data.frame(na_count) #change object to data frame
print(na_count) #show NAs in each column

#Compute the median alcohol content and international bitterness unit for each state. Plot a bar chart to compare.
new.df.med <- aggregate(new.df[,4:5], list(new.df$State), median, na.rm = TRUE) #store medians of ABV & IBU / state in new data frame
#need to plot data

#Which state has the maximum alcoholic (ABV) beer? Which state has the most bitter (IBU) beer?
new.df[which.max(new.df$ABV), c(4,10)]
new.df[which.max(new.df$IBU), c(5,10)]

#Summary statistics for the ABV variable.
summary(new.df$ABV)

#Is there an apparent relationship between the bitterness of the beer and its alcoholic content? Draw a scatter plot.
scatter.smooth(x = new.df$ABV, y = new.df$IBU) #basic plot showing correlation line
cor(new.df$ABV, new.df$IBU, use = "complete.obs") correlation cooefficients 
linearMod <- lm(IBU ~ ABV, data = new.df) #store linear model
summary(linearMod) #display summary of linear model
```
