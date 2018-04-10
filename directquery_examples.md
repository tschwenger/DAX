// helpful query syntax cheatsheet for query development



// using add columns with values function
// will get you all the values in the column with the measure values
//syntax:
	EVALUATE
	ADDCOLUMNS(
		VALUES()  //table and column name
		,"cube value" //name of column you are creating
		, // measure
		,"calculate" // name of column
		, //addition measure
		)
//add an order by clause is optional	
	
	
//=================================

// using adcolumns, filter and summarize
// addcolumns will add the aggregations you want in your table created in the summarize function
// filter will filter those values down in the table you create in the summarize function
//summarize specifies the table you want to summarize and order by



//sytax
	EVALUATE
	ADDCOLUMNS(
		filter(														//using filter to filter down the result set
			SUMMARIZE('Fact table'				// table to summarize
				,'Date'[Fiscal Year Number]							// group by condition for summarize function
				,'Date'[Fiscal Week Number]							// additional group by condition for summarize function
				,'Account-Subaccount'[Type]					// additional group by condition for summarize function
				)
				,'Date'[Fiscal Year Number]=2017)					// filter condition in the filter function
					,"cube value"									// column name added to the add columns function
					,round([Revenue Weekly],0)					//column calculation in the addcolumns function
					,"calculate"
					,round([Revenue Weekly calculate no filter],0)
					,"pp weekly "
					,round([PP Weekly ],0)
					,"pp weekly  not using unnesserary filter function"
					,round([pp  calculate no filter],0)
	)
//optional group by context can be added



//just using the filter and summarize functions
// equivalent to the query above


	EVALUATE
	FILTER(															//filtering down the summarize function
	
		SUMMARIZE('Fact table'									// table to operate on 
			,'Date'[Fiscal Year Number]							// group by condition	
			,'Date'[Fiscal Week Number]							// group by condition
			,'Account-Subaccount'[Type]							// group by condition
				,"cube value"									//name of column to summarize		
				,round([Revenue Weekly],0)						// measure to aggregate based on the conditions above
				,"calculate"
				,round([Revenue Weekly calculate no filter],0)
				,"pp weekly "
				,round([PP Weekly ],0)
				,"pp weekly  not using unnesserary filter function"
				,round([pp calculate no filter],0))
	,'Date'[Fiscal Year Number]=2017)							// filter condition for the filter funcaiton			
	order by 'Date'[Fiscal Week Number]								// optional order by condition



// addcolumns with the summarize
// no filter condition



	EVALUATE
	ADDCOLUMNS(													//adds columns
	
		SUMMARIZE('Fact table'
			,'Date'[Fiscal Year Number]
			,'Date'[Fiscal Week Number]
			,'Account-Subaccount'[ Type]
			)
				,"cube value"
				,round([Revenue Weekly],0)
				,"calculate"
				,round([Revenue Weekly calculate no filter],0)
				,"pp weekly "
				,round([Weekly pp],0)
				,"pp weekly  not using unnesserary filter function"
				,round([pp calculate no filter],0)
	)
	order by 'Date'[Fiscal Week Number],'Account-Subaccount'[Tuition Type]


//ditching the add columns function
//equivalent to the above function


	EVALUATE	
		SUMMARIZE('Fact table'
			,'Date'[Fiscal Year Number]
			,'Date'[Fiscal Week Number]
			,'Account-Subaccount'[ Type]			
				,"cube value"
				,round([ Revenue Weekly],0)
				,"calculate"
				,round([ Revenue Weekly calculate no filter],0)
				,"pp weekly "
				,round([PP Weekly ],0)
				,"pp weekly  not using unnesserary filter function"
				,round([pp  calculate no filter],0)
				)
	order by 'Date'[Fiscal Week Number],'Account-Subaccount'[Tuition Type]



//using summarizecolumns function
// brings back a single result

	SUMMARIZECOLUMNS(
	EVALUATE SUMMARIZECOLUMNS(

	FILTER(VALUES('Org'[ Name]),'Org'[Name]="R2d2"),

	FILTER(SUMMARIZE('Date', 'Date'[Fiscal Year], 'Date'[Fiscal Quarter Of Year], 'Date'[Fiscal Period]), ('Date'[Fiscal Year] = "FY2017" && 'Date'[Fiscal Quarter Of Year] = "Q1" && 'Date'[Fiscal Period] = "FY2017 P01")),

	"column name",CALCULATE([measure],'Type'[Key]=1,'Scenario'[ScenarioKey]=1))


//equivalent to the above

	EVALUATE
	filter(	
	SUMMARIZECOLUMNS(	
		'Organization'[District Name]
		,'Date'[Fiscal Year]
		, 'Date'[Fiscal Quarter Of Year]
		, 'Date'[Fiscal Period]
		,"GL Selection v LY Pct"
		,CALCULATE([GL Balance Selection v LY Pct])
	),'Organization'[District Name]="R2 D2"&& 'Date'[Fiscal Year] = "FY2017" && 'Date'[Fiscal Quarter Of Year] = "Q1" && 'Date'[Fiscal Period] = "FY2017 P01"
	)
