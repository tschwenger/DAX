

# Overview of X functions

## Key point to understand in utilizing x functions (examples: sumx, minx, countx etc...)

* Use case is for a one to many relationship

* Use the RELATEDTABLE function in conjuction with an X function to define the relationship

* tables have to be related (in case of a roleplaying dimension this would need to be specified with the USERELATIONSHIP function)

Let me expand on this concept as this took me a while to figure out. Think about summarizing a metrics for a fact table, in this case one
uses a regular sum function from a related dimension table. This is due to the the many to one relationship. 

In the case of using the an x function, it is the opposite. One is summarizing a many to one so one is iterating over another data set.

* example: find out the average quantity purchased by product in your product dimension. 

	Average Order = 
	AVERAGEX(
		RELATEDTABLE('Sale'),
			'Sale'[SALES AMOUNT])
    
#### Note: this could be created as a measure or a calculated field

* the key point is to use the relatedtable function
