  # ADVANCED DAX ERROR HANDLING

PARAMETER INCONSISTANCY

- GETTING ERRORS THAT DON'T MAKE SENSE. EXAMPLE RETURN TABLE EXPRESSION WHEN YOU WANT A COLUMN. 
EXAMPLE LASTDATE, RETURNS A TABLE, NOT A VALUE.

## PARAMETER INCONSISTENCY

### FILTER VS CALCULATETABLE

* FILTER EXPRESSION IS DIFFERENT

* FILTER EXPECTS A TRUE/FALSE BOOLEAN EXPRESSION

* CALCULATETABLE CAN TAKE BOTH BOOLEAN AND TABLE EXPRESSIONS SO IT CAN RETURN A TABLE EXPRESSION

### MAX VS LASTDATE

MAX 

* RETURNS A SCALAR VALUE

* CAN BE USED ANYWHERE A SCALAR VALUE IS EXPECTED

LASTDATE

* RETURNS A SINGLE COLUMN/SINGLE ROW TABLE

EXAMPLE MAX VS LAST DATE:

	CALCULATE(
		SUM('MSFT Stock Price'[Close]),
		MAX('Date'[Date])) //table filter expression

* THIS WILL FAIL BECAUSE THE FUNCTION EXPECTS A TABLE FILTER EXPRESSION

FIX BY USING THE FILTER FUNCTION:

	CALCULATE(
		SUM('MSFT Stock Price'[Close]),
		FILTER(
			'DATE',
			MAX('Date'[Date])) //table filter expression
	)

NOTE: CTRL +K+C WILL COMMENT DAX CODE

NOTE: CTRL +K+U WILL UNCOMMENT DAX CODE

### EXAMPLE FILTER VS CALCULATETABLE

	Closing Price (EOM) Calculate = 
		CALCULATE (
    		   [Close Price], 
  		     CALCULATETABLE(
      	      LASTNONBLANK('Date'[Date],//MAY 15 2017 
						   
       		     PARALLELPERIOD ( 
					'Date'[Date], 
					0, 
					MONTH )
					[Close Price]),)))

SO WHY DIDN'T WE USE THE FILTER FUNCTION INSTEAD OF THE CALCULATE TABLE?


	Closing Price (EOM) FILTER = 
	CALCULATE (
  	   [Close Price], 
		 FILTER(	
				LASTNONBLANK('Date'[Date],
							[Close Price]),		
				//1=1))
				PARALLELPERIOD ( 
					'Date'[Date], 
					0, 
					MONTH)))

* THIS FAILS, BECAUSE YOU CANNOT HAVE THE TABLE FILTER EXPRESSION
* GOING TO RETURN A LIST OF DATES, WHICH THE FILTER FUNCTION DOESN'T ALLOW
