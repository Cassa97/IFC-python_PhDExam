# IFC-python_PhDExam
PhD project that aims to extract data from excel files to translate them into a new standardized format (IFC). 
The goal is to automate the data translation process to facilitate the creation of a large number of files.
This project was carried out during a PhD course at the Polytechnic of Milan.

Step of the work:


![249261566-08e11d72-c3bc-4c43-a0c1-1d55873bf7f6](https://github.com/Cassa97/IFC-python_PhDExam/assets/115898053/1df25fdd-484c-4ab9-81c8-c2b00b3347ff)

# READING EXCEL INPUT FILE
1. Definition of the Excel worksheet name:
    - The variable name is defined with the name of the Excel worksheet from which to extract the data. In your case, the name of the Excel worksheet is "STRATO DI MURATURA".
2. Definition of the Excel file path:
    - The variable path is defined with the path where the Excel files are located.
3. List of Excel files in the folder:
    - The glob.glob() function is used to obtain a list of all files with the ".xlsm" extension in the folder specified by the path variable.
    - An empty list file_list is defined to store the names of the found Excel files.
4. Reading the Excel files and creating a list of DataFrames:
    - An empty list excl_list is defined to store the DataFrames of the read Excel files.
    - A for loop is used to iterate through each file in the file_list.
    - The pd.read_excel() function is used to read the Excel file specified by the file name and worksheet name (sheet_name).
    - The DataFrame of the Excel file is added to the excl_list.
5. Merging the DataFrames:
    - A new empty DataFrame excl_merged is created to store the final result of the merge.
    - A for loop is used to iterate through each DataFrame in the excl_list.
    - The pd.concat() function is used to merge the current DataFrame with excl_merged, ignoring the row indices (ignore_index=True).
6. Exporting the merged DataFrame to a new Excel file:
    - A new path path_new is defined where the new Excel file will be saved.
    - The to_excel() method is used to export the excl_merged DataFrame to an Excel file in the specified path, with the name "_ALL DESTR OP-LV.xlsx" and without including the row index (index=False).

The entire process combines the data from the specified Excel worksheets into a single DataFrame and exports it to a new Excel file. It also plays a brief alert sound to indicate the completion of the operation.

![ifc - Copia1](https://github.com/Cassa97/IFC-python_PhDExam/assets/115898053/2fd22211-f672-416e-9573-6c95291cad9d)

# CREATION OF OPEN BIM COST ENTITIES
1. Function Definitions:
    - costvalue(name, description, value): Creates and returns an IfcCostValue object with the specified name, description, and value.
    - costitem(name, description): Creates and returns an IfcCostItem object with the specified name and description.
    - relcost(name, description): Creates and returns an IfcRelAssignsToControl object with the specified name and description.
2. Data Loading:
    - The Excel file containing the data is opened using the pd.read_excel() method.
    - The "GENERE" column is filtered to include only rows with the value "OPERA".
3. Data Iteration:
    - Iteration is performed on each row of the DataFrame using the for loop index, row in dati.iterrows():.
    - The object, price, and price list code values are extracted from each row.
4. IFC File Creation for Cost Item:
    - A new ifc_file object is created using ifcopenshell.file() to represent an empty IFC file.
5. Generic Wall Entity Creation:
    - An IfcWall object named wall is created using the ifc_file.createIfcWall() method.
    - The name of the wall entity is set using the "OGGETTO" attribute from the DataFrame.
6. IfcCostItem and IfcCostValue Entity Creation:
    - Values for the name, description, and value of the IfcCostValue object are created based on the wall entity.
    - The costvalue_obj object is created using the costvalue() function with the appropriate values.
    - The cost object is created using the costitem() function with the appropriate name and description.
    - costvalue_obj is assigned as the cost value to cost using the CostValues attribute.
7. Creation of the Relation between Cost Item and Wall Entity:
    - An IfcRelAssignsToControl object named relass_to is created using the relcost() function.
    - The RelatedObjects attribute of relass_to is set to [wall] to indicate that the wall entity is connected.
    - The RelatingControl attribute of relass_to is set to cost to indicate that cost is the associated control.
8. Printing the Results:
    - The cost, cost value, and newly created relation are printed for the control.
9. Saving the IFC File:
    - The path to save the IFC file is specified using the path variable.
    - The ifc_file.write() method is used to write the content of the ifc_file object to the IFC file at the specified path.

The entire process is repeated for each row in the DataFrame, creating a new IFC file for each generic wall entity.


# RUN CODE
To proceed to use the loaded script you need to replace the input and output path with your local location where the downloaded files were saved. 
Also at the end of the script will be overwritten and updated the files contained in the folder "IfcCostItem" with the new IFC files cost and excel files "_ALL DESTR OP-LV_updated" and "Conversion files OBJECT-IFCCLASS"

In case you want to edit the input files you will need to replace/edit the files in the folder "Single Files
