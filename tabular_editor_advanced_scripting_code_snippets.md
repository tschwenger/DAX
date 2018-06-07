
# Useful code snippets for the tabular editor

## Loop over and change the format of all measures in the table

* This is useful when there is a standardized data format for the all measures in a table. 

* This snippet can also be applied to measures for a particular table

    foreach(var table in Model.Tables)
      {
        foreach(var measure in table.Measures)
      {
          // Foramat every measure in Model to the required format.
          measure.FormatString = "#,##0.000000;(#,##0.000000)";
      }
    }
