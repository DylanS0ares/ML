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
