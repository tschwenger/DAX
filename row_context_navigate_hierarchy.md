# Navigate a hierarchy by date with a funky constant measure

Problem: rate problem with two measures except both measures are dynamic based on where they are in the hierarchy

capacity: want the capacity at the daily level, but weekly level is the sum of five business days and the month/period is the sum of the weeks underneath

amount: the amount is a fraction of what it is on the daily level, but can be summed based on the normal behavior at week level and higher in a date hierarchy

Design

* create a variable for the amount 

* create a variable for the capacity (doesn't need to be, but for my task it was necessary)

* Use the switch function to design cases for the each level of the date hierarchy

* start at the date level and work your way outward

* use hasonevalue to check for date level

* use isfiltered function to check for every other level of the hierarchy

* use values to reference the filter context of the report level

* use sumx to sum the values of the filter context

## Solution:

    Occupancy:=
    var 
    Capacity=CALCULATE(SUM('Cc'[Capacity]))

      var
    DailyLevelsum=CALCULATE([Sum]*5)

    return

    SWITCH(TRUE,
	    HASONEVALUE('Date'[Full Date]),DIVIDE(DailyLevelsum,Capacity,BLANK()),
	    ISFILTERED('Date'[Fiscal Week]),DIVIDE([Sum],Capacity,BLANK()),
	    ISFILTERED('Date'[Fiscal Period]),DIVIDE([Sum],SUMX(VALUES('Date'[Fiscal Week]),Capacity),BLANK()),
	    ISFILTERED('Date'[Fiscal Quarter]),DIVIDE([Sum],SUMX(VALUES('Date'[Fiscal Period]),Capacity),BLANK()),
	    ISFILTERED('Date'[Fiscal Year]),DIVIDE([Sum],SUMX(VALUES('Date'[Fiscal Quarter]),Capacity),BLANK()),BLANK())
