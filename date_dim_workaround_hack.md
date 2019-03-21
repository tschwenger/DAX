

# When you don't have a contiguous date table, because your company is clueless

### YTD

          YTD Measure=
          VAR SelectedFiscalYearNumber = SELECTEDVALUE ( 'Date'[Fiscal Year Number] )
          VAR MaxFiscalDayOfYear = MAX ( 'Date'[Fiscal Day Of Year Number] )
          RETURN
              CALCULATE (
                  [Measure],
                  ALL ( 'Date' ),
                  'Date'[Fiscal Year Number] = SelectedFiscalYearNumber,
                  'Date'[Fiscal Day Of Year Number] <= MaxFiscalDayOfYear
              )


### QTD

        QTD Measure=
        VAR SelectedFiscalQuarterNumber = SELECTEDVALUE ( 'Date'[Fiscal Quarter Number] )
        VAR MaxFiscalDayOfQuarter = MAX ( 'Date'[Fiscal Day Of Quarter Number] )
        RETURN
        CALCULATE (
        [Measure],
        ALL ( 'Date' ),
        'Date'[Fiscal Quarter Number] = SelectedFiscalQuarterNumber,
        'Date'[Fiscal Day Of Quarter Number] <= MaxFiscalDayOfQuarter
        )
        

### PTD

        PTD measure=
        
                VAR SelectedFiscalPeriodNumber = SELECTEDVALUE ( 'Date'[Fiscal Period Number] )
        VAR MaxFiscalDayOfPeriod = MAX ( 'Date'[Fiscal Day Of Period Number] )
        RETURN
            CALCULATE (
                [Measure],
                ALL ( 'Date' ),
                'Date'[Fiscal Period Number] = SelectedFiscalPeriodNumber,
                'Date'[Fiscal Day Of Period Number] <= MaxFiscalDayOfPeriod
            )



### LY


                 LY Measure=
          CALCULATE (
              [Measure],
              ALL ( 'Date' ),
              TREATAS (
                  SELECTCOLUMNS (
                      'Date',
                      "Full Date LY", LOOKUPVALUE (
                          'Date'[Full Date],
                          'Date'[Fiscal Day Of Year Number], 'Date'[Fiscal Day of Year Number],
                          'Date'[Fiscal Year Number], 'Date'[Fiscal Year Number] - 1
                      )
                  ),
                  'Date'[Full Date]
              )
          )


### LY YTD


   
   
   
          LY YTD Measure=
          VAR SelectedFiscalYearNumber = SELECTEDVALUE ( 'Date'[Fiscal Year Number] ) - 1
          VAR MaxFiscalDayOfYear = MAX ( 'Date'[Fiscal Day Of Year Number] )
          RETURN
              CALCULATE (
                  [Measure],
                  ALL ( 'Date' ),
                  'Date'[Fiscal Year Number] = SelectedFiscalYearNumber,
                  'Date'[Fiscal Day Of Year Number] <= MaxFiscalDayOfYear
              )


### LY QTD




            LY QTD=
            VAR SelectedFiscalQuarterNumber = SELECTEDVALUE ( 'Date'[Fiscal Quarter Number] ) - 10
            VAR MaxFiscalDayOfQuarter = MAX ( 'Date'[Fiscal Day Of Quarter Number] )
            RETURN
                CALCULATE (
                    [Measure],
                    ALL ( 'Date' ),
                    'Date'[Fiscal Quarter Number] = SelectedFiscalQuarterNumber,
                    'Date'[Fiscal Day Of Quarter Number] <= MaxFiscalDayOfQuarter
                )

### LY PTD




          LY PTD Measure=
          
                      VAR SelectedFiscalPeriodNumber = SELECTEDVALUE ( 'Date'[Fiscal Period Number] ) - 100
            VAR MaxFiscalDayOfPeriod = MAX ( 'Date'[Fiscal Day Of Period Number] )
            RETURN
                CALCULATE (
                    [Measure,
                    ALL ( 'Date' ),
                    'Date'[Fiscal Period Number] = SelectedFiscalPeriodNumber,
                    'Date'[Fiscal Day Of Period Number] <= MaxFiscalDayOfPeriod
                )

## LY (periodic snapshot)

                              Measure LY Periodic:=
                              var datetable =		
                                                            TREATAS(
                                                            SELECTCOLUMNS(
                                                                      DimDate,
                                                                      "CalendarDate",
                                                                                LOOKUPVALUE(
                                                                                DimDate[CalendarDate],
                                                                                DimDate[DayOfYearNumber],DimDate[DayOfYearNumber],
                                                                                DimDate[YearNumber],DimDate[YearNumber]-1
                                                                                )
                                                                      ),
                                                                      DimDate[CalendarDate]
                                                                      )
                                                                      return

                                        CALCULATE(
                                                  [Measure],
                                                  all(DimDate),
                                                  datetable
                                                                      )	
