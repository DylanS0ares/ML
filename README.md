# ML
Exploring Data Retrieval and Machine Learning with Pandas

First of all, let's introduce some types of files that it can be use with pandas.

CSV Files :
  Comma-separated(CSV) files consist of rows of data , separated by comma. In Pandas , CSV files can typically be read using just a few lines of code.

## Import the data

   import pandas as pd
   filepath= 'data/iris_data.csv'
   data = pd.read_csv(filepath)
   print(data.iloc[:5])


## Reading CSV Files: Useful Arguments

    #  Different delimiters - tab-separeted file (.tsv)
     data = pd.read_csv(filepath,sep='\t')
    #  Different delimiters - space-separated file:
     data = pd.read_csv(filepath,delim_whitespace=True)
    #  Don't use first row for column names:
     data = pd.read_csv(filepath,header=None)
    # Specify column names:
     data = pd.read_csv(filepath, names=['Name1','Name2'])
    #Custom missing values:
     data = pd.read_csv(filepath,na_values=['NA',99])


Json Files:
  Javascript Object Notation(JSON) files are a standard way to store data across plataforms. JSON Files are very similar in structure to python dictionaries.
  # Read JSON file as dataframe
    # data = pd.read_json(file_path)
    # # Write dataframe file to json
    # data.to_json('outputfile.json')

The next step was to understand how databases work, and IBM provided examples that used Jupyter.However, I had to make changes to python code, because it didn't work well.

### Learning Objective(s)

 - Create a SQL database connection to a sample SQL database, and read records from that database
 - Explore common input parameters

### Packages

 - [Pandas](https://pandas.pydata.org/pandas-docs/stable/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01)
 - [Pandas.read_sql](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01)
 - [SQLite3](https://docs.python.org/3.6/library/sqlite3.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01)

 - ## Simple data reads

Structured Query Language (SQL) is an [ANSI specification](https://docs.oracle.com/database/121/SQLRF/ap_standard_sql001.htm?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01#SQLRF55514), implemented by various databases. SQL is a powerful format for interacting with large databases efficiently, and SQL allows for a consistent experience across a large market of databases. We'll be using sqlite, a lightweight and somewhat restricted version of sql for this example. sqlite uses a slightly modified version of SQL, which may be different than what you're used to. 
## Database connections

Our first step will be to create a connection to our SQL database. A few common SQL databases used with Python include:

 - Microsoft SQL Server
 - Postgres
 - MySQL
 - AWS Redshift
 - AWS Aurora
 - Oracle DB
 - Terradata
 - Db2 Family
 - Many, many others
 
Each of these databases will require a slightly different setup, and may require credentials (username & password), tokens, or other access requirements. We'll be using `sqlite3` to connect to our database, but other connection packages include:

 - [`SQLAlchemy`](https://www.sqlalchemy.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01) (most common)
 - [`psycopg2`](http://initd.org/psycopg/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01)
 - [`MySQLdb`](http://mysql-python.sourceforge.net/MySQLdb.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMML0232ENSkillsNetwork837-2023-01-01)


```python
# Imports
import sqlite3 as sq3
import pandas.io.sql as pds
import pandas as pd

# Download the database
!wget -P data https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-ML0232EN-SkillsNetwork/asset/classic_rock.db

# We now have a live connection to our SQL database
# Initialize path to SQLite database
path = 'data/classic_rock.db'
con = sq3.Connection(path)

# Write the query
query = '''
SELECT * 
FROM rock_songs;
'''

# Execute the query
observations = pds.read_sql(query, con)
observations.head()

# We can also run any supported SQL query
# Write the query
query = '''
SELECT Artist, Release_Year, COUNT(*) AS num_songs, AVG(PlayCount) AS avg_plays  
FROM rock_songs
GROUP BY Artist, Release_Year
ORDER BY num_songs DESC;
'''

# Execute the query
observations = pds.read_sql(query, con)
observations.head()

## Common parameters

# There are a number of common parameters that can be used to read in SQL data with formatting:
# - coerce_float: Attempt to force numbers into floats
# - parse_dates: List of columns to parse as dates
# - chunksize: Number of rows to include in each chunk
 
# Let's have a look at using some of these parameters
query = '''
SELECT Artist, Release_Year, COUNT(*) AS num_songs, AVG(PlayCount) AS avg_plays  
FROM rock_songs
GROUP BY Artist, Release_Year
ORDER BY num_songs DESC;
'''

# Execute the query
observations_generator = pds.read_sql(query,
                            con,
                            coerce_float=True, # Doesn't affect this dataset, because floats were correctly parsed
                            parse_dates=['Release_Year'], # Parse `Release_Year` as a date
                            chunksize=5 # Allows for streaming results as a series of shorter tables
                           )

for index, observations in enumerate(observations_generator):
    if index < 5:
        print(f'Observations index: {index}')
        display(observations)
