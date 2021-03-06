# TIME INTELLIGENCE USING CALCULATE

## YTD SALES

SYNTAX

* TWO FILTER CRITERIA, THE MAX YEAR AND THE MAX DATE

* ALL IS USED TO BRING BACK ENTIRE DATE TABLE, IT IS THEN FILTERED TO BRING BACK THE MAX YEAR


	CALCULATE(
		[EXPRESSION],
		FILTER(
			ALL('DATE'),
			DATE[YEAR]=MAX(DATE[YEAR]) &&
			DATE[DATE]<=MAX(DATE[DATE)
			)
		)

EXAMPLE:

	YTD SALES = 
	CALCULATE(
	[Quantity Sold],
	FILTER(ALL('Date'),
		'Date'[Year]=MAX('Date'[Year]) &&
			'Date'[Date]<=MAX('Date'[Date])))

* IF THE ALL FUNCTION IS OMITTED, IT WILL NOT YIELD YTD, BECAUSE THE FILTER CONTEXT WILL ONLY LOOK AT THE FILTER CONTEXT LEVEL


## MTD SALES

SYNTAX

* THREE FILTER CRITERIA, THE MAX YEAR, MAX MONTH, AND THE MAX DATE

	CALCULATE(
		[EXPRESSION],
		FILTER(
			ALL('DATE'),
			DATE[YEAR]=MAX(DATE[YEAR]) &&
			DATE[MONTH]=MAX(DATE[MONTH]) &&
			DATE[DATE]<=MAX(DATE[DATE)
			)
		)

EXAMPLE: 

	MTD SALES = 
	CALCULATE(
		[Quantity Sold],
		FILTER(ALL('Date'),
			'Date'[Year]=MAX('Date'[Year]) &&
			'Date'[Month]=MAX('Date'[Month]) &&
				'Date'[Date]<=MAX('Date'[Date])))

## 12 MONTHS ROLLING TOTAL

USE OF 

NEXTDAY: returns a table that contains a column of all dates from the next day, based on the first date specified in the dates column in the current context

LASTDATE: returns the last date in the current context for the specified column of dates

SAMEPERIODLASTYEAR: returns a table that contains a column of dates shifted one year back in time from the dates in the specified dates column, in the current context

DATESBETWEEN: returns a table that contains a column of dates that begins with the start_date and continues until the end_date

EXAMPLE:

	12 MONT ROLLING SALES = 

	CALCULATE(
	[Total Sales],
	DATESBETWEEN('Date'[Date],
		NEXTDAY(SAMEPERIODLASTYEAR(LASTDATE('Date'[Date]))),
		LASTDATE('Date'[Date]))) 

* DATESBETWEEN RETURNS THE TABLE OF THE VALUES

* START_DATE IN THE DATEBETWEEN IS TRICKY, USE THE NEXTDAY IN CONJUNCTION WITH SAMEPERIODLASTYEAR AND LASTDATE

	* NEXTDAY BRINGS BACK THE LAST DATE FROM THE SAME PERIOD A YEAR AGO

	* NOTE: IF THE SAMEPERIODLASTYEAR IS USED BEFORE THE NEXT DAY, THERE ARE FUNKY RESULTS, BECAUSE YOU WILL BRING BACK BLANKS AT THE END OF THE FILTER CONTEXT OF THE DATE TABLE

* THE END_DATE IS STRAIGHTFORWARD, LASTDATE BRINGS BACK THE LAST DATE WITHIN THE FILTER CONTEXT

## 3 MONTHS ROLLING TOTAL

keys:

* DATEADD FUNCTION: RETURNS a table that contains a column of dates, shifted either forward or backward
	in time by the specified number of intervals from the dates in the current context

EXAMPLE:

	3 Month Rolling Total = 
	CALCULATE(
	[Total Sales],
	DATESBETWEEN('Date'[Date],
		NEXTDAY(DATEADD(LASTDATE('Date'[Date]),-3,MONTH)),
		LASTDATE('Date'[Date]))) 

* USE CALCULATE

* DATEBETWEEN IS USED TO DEFINE THE DATE RANGE

* NEXT DAY IS USED IN THE SAME CONTEXT AS THE 12 MONTH ROLLING AVERAGE

* THE DATEADD FUNCTION IS USED INSTEAD OF THE SAMEPERIODLASTYEAR, BECAUSE IT CAN SPECIFY A CUSTOM INTERVAL
	IN THIS CASE, THREE MONTHS PRIOR (-3)

* LAST DATE SPECIFIES THE LAST DATE IN BOTH DATE CONTEXTS.

## MOVING AVERAGES

THIS IS PRETTY STRAIGHT FORWARD ONCE THE ABOVE ( 3 MONTH ROLLING AVERAGE) IS CALCULATED

jUST USE THE DIVIDE FUNCTION

EXAMPLE:

	3 MONTH MOVING AVERAGE SALES = 
 	DIVIDE([3 Month Rolling Total],3)
