# Switch function

## The function can make your life way easier

### Use Cases

* Better and cleaner form of an If statement
* Simplify code
* Avoid nesting many if statements
* Better maintainability of code

Note: as I got better at DAX, I become very good at nested functions. However, multiple nested functions are hard to maintain and can be
painful to update, change or reproduce (especially if it's someone elses code). The switch function simplifies that. 

## Sytax

SWITCH( expression , value , result [, value, <result>] … [, <lse] ) 

Example

New Month = 

SWITCH('Date'[Calendar Month Number],

	1,"January",
	
	2,"Feb",
	
	3,"mar",
	
	4,"april",
	
	5,"may",
	
	6,"jun",
	
	7,"jul",
	
	8,"aug",
	
	9,"sep",
	
	10,"oct",
	
	11,"nov",
	
	12,"dec")
  
###  IF versus switch
  
* if statement

Population Groups = 

IF('StatePopulation'[Population Ranking]<10,"Top 10",

IF('StatePopulation'[Population Ranking]<20,"11-20",

	IF('StatePopulation'[Population Ranking]<30,"21-30",
	
		IF('StatePopulation'[Population Ranking]<40,"31-40",
			
			"other"))))	
      
 * Switch Statement

Population groups2 = 

SWITCH(TRUE(),
	
	'StatePopulation'[Population Ranking]<10,"Top 10",
	
	'StatePopulation'[Population Ranking]<10,"Top 20",
	
	'StatePopulation'[Population Ranking]<10,"Top 30",
	
	'StatePopulation'[Population Ranking]<10,"Top 40",
	
	"other")
 
  
