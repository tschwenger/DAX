
//create the hierarchy
  HierarchyPath=PATH(Resources'[EmployeeKey,'Resources[ParentEmployeeKey])


Then create a calculated column for each level of the hierarchy
THIS RETURNS THE FIRST LEVELOF THE PATH

//THIS RETURNS THE FIRST LEVEL OF THE HIERARCHY
  LEVEL 1=LOOKUPVALUE([FULL NAME],[EMPLOYEEKEY],VALUE(PATHITEM([HIERARCHYPATH,1)))
