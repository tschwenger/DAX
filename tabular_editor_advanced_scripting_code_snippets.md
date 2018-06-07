
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

* This would be an update for a particular table:

        foreach(var table in Selected.Tables)
                {
                foreach(var measure in table.Measures)
                {
        // Format every measure in table "fact table" to the required format.
        measure.FormatString = "#,##0.000000;(#,##0.000000)";
         }
                }
                
 Note: The developer needs to select the tables he/she is operating on in the editor
 
 ## Create a standard format for all measures in the cube
 
         ,
          {
            "ID": "STANDARD_FORMAT",
            "Name": "All measures should use our standard format",
            "Category": "Metadata",
            "Description": "There is a newly established format for all of our Measures. We should be using this.",
            "Severity": 1,
            "Scope": "Measure",
            "Expression": "FormatString <> \"#,##0.000000;(#,##0.000000)\"",
            "FixExpression": "FormatString = \"#,##0.000000;(#,##0.000000)\";",
            "CompatibilityLevel": 0
          }
