# This document houses DAX code for sample time intelligence calculations for calculated measures


## YTD total


YTD PROFITS = TOTALYTD([Profit],'Date'[Date])

* use the measure you want to and add the date table
* pretty straight forward calculation

## Last Year's total
 
Last Year YTD Profit = TOTALYTD('Internet Sales'[Profit],DATEADD('Date'[Date],-11,MONTH))

* use the expression you want to measure
* use the dateadd function to link to the date table
* subtract off the 11 periods and set to month

or 
test = TOTALYTD([lead count],DATEADD('date'[Date],-12,month))

test 2 = CALCULATE([lead count ly],DATESYTD('date'[Date]))

test 3 = CALCULATE([lead count],DATEADD(DATESYTD('date'[Date]),-1,year))

test 4 = CALCULATE([lead count],SAMEPERIODLASTYEAR(DATESYTD('date'[Date])))

test 9:=CALCULATE([lead count ly ytd],DATESYTD('date'[Date]))



## Prior Year Amount


Prior Year Profit = CALCULATE('Internet Sales'[Profit], SAMEPERIODLASTYEAR('Date'[Date]))

* use the expression you want to link to the measure
* use the sameperiodlastyear function and link to the date table

## Rolling 12 month amount

Rolling 12 Months Profit = CALCULATE('Internet Sales'[Profit], DATESBETWEEN('Date'[Date],DATEADD(FIRSTDATE('Date'[Date]),-11, MONTH),LASTDATE('Date'[Date])))

* use the measure you want to calculate
* use the datesbetween function and link to the date table
* for the start date, use the dateadd function with the firstdate function to get the start date that is 11 months ago
* us the lastdate function and link to the date table to get the end date

## Calculate the total number of business days

total days pended= CALCULATE(SUM(DATEDIM[WEEKDAY_FLAG]),DATESBETWEEN(DATEDIM[DATE_VALUE],COMPLETEDTICKETS[PendStartDate],COMPLETEDTICKETS[PendEndDate])) 

* set up a weekday or business day flag in the date table and set the data type to numeric
* Sum the total number of business days or weekdays and use the dates between function linking to the date dimension and then use the dates
	that you want to evaluate the difference of
* note, the date table does not need to be related to the table to work


## Calculate the total number of business days

note: in some cases the DATESBETWEEN function will not work and will spew the error: "Invalid Numeric Representation of a Date Value" error. 
In this case use the following formula

Days to complete:=CALCULATE(COUNTROWS('DateDim'),FILTER('DateDim',DateDim[DATE_VALUE]>=WildWorksMetaData[BertRecievedDate] && DateDim[DATE_VALUE]<=WildWorksMetaData[CompletionDate]&&DateDim[Business Day]=1)))

* use the calculate function and count the rows in the datedim
* use the filter function to count the rows that are equal to or greater than the startdate and lesser than or equal to the enddate with the condition of the business day value being true. 



## time intelligence calculation

First Date Calculation (not used by itself, used as a parameter (rolling 12 month function) 
used to nest in more complex calculations

First Purchase Date = FIRSTDATE('Sale'[Invoice Date Key])


## Last Purchase Date 

Last Purchase Day = LASTDATE('Sale'[Invoice Date Key])

===================

## YTD calculation

YTD Sales = TOTALYTD([Total Sales],'Date'[Date])

QTD Sales = TOTALQTD([Total Sales],'Date'[Date])

MTD Sales = TOTALMTD([Total Sales],'Date'[Date])


## Prior Year Sales

Prior Year Sales = 
	CALCULATE([Total Sales],
		SAMEPERIODLASTYEAR('Date'[Date])
		)

YOY Sales

YOY Sales = [Total Sales]-[Prior Year Sales]

YOY Growth

YOY % Growth = DIVIDE([YOY Sales],[Prior Year Sales])


## Prior Month Sales

Prior Month Sales = 
	CALCULATE([Total Sales],
		PARALLELPERIOD('Date'[Date],-1,MONTH)
	)

## Fiscal YTD Sales

Fiscal YTD Sales = 
TOTALYTD([Total Sales],'Date'[Date],"05/31")

Sales on a regional level (for comparision)

Southeast Sales = 
IF([Total Sales]>0 ,
CALCULATE([Total Sales],
	'City'[Sales Territory]="Southeast"),
	BLANK())

All sales 
All Territory Sales = 
CALCULATE([Total Sales],
	ALL('City'[Sales Territory]))

All Territory Sales = 
IF([Total Sales]>0,
CALCULATE([Total Sales],
	ALL('City'[Sales Territory])),
	BLANK())

Territory % of Total = DIVIDE([Total Sales],[All Territory Sales])


average order for a customer (customer table to sales table, one to many) (calculated column)

Average Order = 
AVERAGEX(
	RELATEDTABLE('Sale'),
		'Sale'[SALES AMOUNT])

first purchase date

First Purchase date = MINX(RELATEDTABLE(Sale),'Sale'[Invoice Date Key])
