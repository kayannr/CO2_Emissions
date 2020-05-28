# CO2_Emissions
![CO2_Emissions](Images/bra05.19.analytics-coal-emissions.jpg)

### Introduction: 
>Recent reports reveal that the Covid-19 shutdowns cut Carbon dioxide (CO2) emissions down to approximately 17%. Fossil fuel use is the primary source of CO2 but it can also be emitted from direct human-induced impacts on forestry and other land use. The global CO2 emission has significantly increased since the 1900s and remains as a major concern for the environmental and public health sector. The datasets for Global CO2 emissions by country are publicly available online for extraction.

### Project Description:
This project explores the annual global carbon dioxide emission for each country that has publicly available data. The final dataset generated after performing ETL is loaded to its final destination, MongoDB. The following list shows a general breakdown of the different tasks completed for this project: 
>   * 1: Data collection/extraction 
>   * 2: Data exploration, cleanup, and transformation
>   * 3: Data loading 

### Purpose: 
The purpose of this project is to perfrom ETL (Extract, Transform, Load) on datasets collected from variety of sources. The collection of datasets is transformed in order to make it usable for data analysis.  

### Scope: 
* Annual global CO2 emission by country.
* The collected data includes year 1970 through 2019.

### Dataset Sources: 
>* Knoema: https://knoema.com/EDGARED2019/global-ghg-and-co2-emissions 
>    * 208 countries (1970-2018)
>* Nationmaster: https://www.nationmaster.com/nmx/ranking/carbon-dioxide-emissions
>    * 66 countries (2019)
>* Population 2019: https://worldpopulationreview.com/

### Actions and Tasks: 
* Importing dependencies: 
>"pandas, requests, knoema, pymongo" - dependencies

* Collecting all the data from online sources: A total of 3 datasets were collected from different online sources. 
    * JSON file: contains 2019 population count for every country (https://worldpopulationreview.com/)
    * API: contains annual global CO2 emission for a total of 208 countries from 1970 through 2018 (https://knoema.com/EDGARED2019/global-ghg-and-co2-emissions)
    * Web-scraped: contains annual global CO2 emission for a total of 66 countries (https://www.nationmaster.com/nmx/ranking/carbon-dioxide-emissions)

* Data Cleaning, Transformation, and Exploration
    * This task includes checking for null values, verifying data type of each column contained in the dataframes, renaming columns, removing multi-index, adding/removing necessary columns, resetting index, merging datasets, and reorganizing the order of columns. 

        * Null values check: 
            All three datasets collected do not contain any null values based on the null values check performed. 

        * Data type verification: 
            The data types are verified as float64 and object for all three datasets collected. Converting object type to string is not needed in pandas, since strings   strings are stored as objects. Pandas uses object as the datatype for strings so that you can perform size-changing manipulations on the strings (e.g., concatenating them with other strings) without having to recreate the entire column with a new string length.

        * Column names revision:
            * 2019 Population by Country (JSON): After the removal of unwanted columns in the dataset, the remaining columns are then renamed. 'name' column is changed to 'Location' in order to match the 'Location' column in other datasets for merging. The 'pop2019' column is renamed to 'Population(2019)' for clarity. 

            * 1970-2018 carbon dioxide emission data for 208 countries (knoema API): Most of the column names in this dataset are intially shown in a 'yyyy-mm-dd hr:min:secs' format. These columns are originally in a datetime data type, which is converted into object when exported as a csv that is subsequently read into a dataframe. The columnns are renamed in order to only show the year instead of including extra strings of characters since the CO2 emission by country is an annual data. 

            * 2019 carbon dioxide emission data for 66 countries (web-scraped): The 'Million Metric Tons' is renamed to '2019-01-01 00:00:00' and '66 countries column is changed to 'Location' in order to match other dataset columns for merging. 

            * Final Datasets: 'Location' column is changed to 'Country

        * Multi-index removal: The 1970-2018 carbon dioxide emission data for 208 countries (knoema API) originally has multi-index, which is removed by transposing the table and resetting the index. 

        * Column addition and removal: 
            * 2019 Population by Country (JSON):
                The columns deleted are 'Rank','GrowthRate', 'area', 'Density', and 'pop2020'. Population 2019 is chosen instead of Population 2020 since the CO2 emission data is not yet available for year 2020. 
            * 1970-2018 carbon dioxide emission data for 208 countries (knoema API): 
                No columns removed/added to this dataset
            * 2019 carbon dioxide emission data for 66 countries (web-scraped): 
                Rank '#', Year, YOY, 5 years CAGR columns are removed from the original dataset. 
            * Final Datasets:
                A 'unit' column is added to this dataset. The 'Frequency' column values are changed from 'A' to 'Annually' for clarification. 

* Loading data into Database (MongoDB)
    * 2 datasets are loaded into MongoDB database called 'globalco2emission_dt' that contains 2 collections: 
>       * 'CO2_208countries' - The data set in this collection includes the annual global CO2 emission for 208 countries 
>       * 'CO2_66countries_1970to2019' - The dataset in this collection includes the annual global CO2 emission for 66 countries 

### Limitations 
The major limitation encountered while completing this project is obtaining a most recent (2019-2020) annual global CO2 emission for all approxmately 200 countries and/or territories in the world. The collected CO2 emissions for year 2019 only includes 66 countries. Even though recent reports reveal that there has been a 17% decrease in the overall CO2 emission during the Covid-19 pandemic, the data for CO2 emissions per country is still not available to the public. However, CO2 emission is available for all countries for 1970 through 2018. 

