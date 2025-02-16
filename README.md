DATA ENGINEERING END-TO-END YOUTUBE TRENDING ANALYSIS

STEP 1 - ANALYSING DATA

I downloaded the dataset from Kaggle and I first analyzed the dataset. The folder contained .JSON files and also .csv files. Before working with the data, I first had to analyze and see how the data was.
The CSV files had no problems other than encoding which I solved by resaving it as UTF-8 encoding. As for the .JSON files their formatting had problems
![Screenshot (567)](https://github.com/user-attachments/assets/cee5a200-c04b-4fc0-aa71-7a38abfaaebd)
As you can see the items part is an array which would cause problems if I had to use it, so before that I was going to upload the data and then preprocess it. 

STEP 2 - UPLOADING DATA

I used s3 buckets to upload my data. I formatted the .csv files using Windows CMD commands in such a way that they would form folders according to their region. Like below:
![Screenshot (568)](https://github.com/user-attachments/assets/063fd72c-ed0c-44cb-8e71-e51588e0bc18)
and I uploaded the.JOSN files in one folder

STEP 3 - CREATING TABLE FOR CSV FILES

I used AWS Glue crawlers to create a table in the data catalog, which I will alter to join with the .JSON files table to form a final table for analysis. The schema for the table looked like this
![Screenshot (569)](https://github.com/user-attachments/assets/adba2ce2-d7bc-4ce6-a104-8dc0c2172de7)
because each region was stored in a separate folder, it created a partition according to regions.

STEP 4 - CLEANING AND NORMALISING JSON FILES

Before I make a table for the .JSON files, I first had to clean the files. Upon analysis, everything I needed was in the 'Items' array, so I used AWS Lambda to extract the info, normalize it and then store it as a separate table. If I skipped this step the schema would show items as an array that cannot be used to join with the CSV table. I created a function for which the code is above and ran a test to store it as a parquet file in the bucket. This created a table with the schema that I needed.
![Screenshot (570)](https://github.com/user-attachments/assets/b11d5c04-17be-4df2-8435-9e4b5936da41)


STEP 5 - PERFORMING JOIN

Now that the tables are made, they are ready to be inner joined. I used an ETL job like the one below
![Screenshot (571)](https://github.com/user-attachments/assets/5b04d64a-e21f-4a54-bf2b-c9cbeed9a409)
I changed the schema of the .JSON table before joining because i was going to use 'id' as the key to join the tables but it had a different data type than the 'category_id' key I was going to use from the CSV table. So I made sure both had the same data types before joining them.
Once joined, a table was formed which was ready to be used for analysis

STEP 6 - VISUALISATION

I used Amazon Quicksight to create a dashboard and to perform analysis. I made and published a dashboard with few graphs like:
![Screenshot (566)](https://github.com/user-attachments/assets/45f40102-672a-4cb0-8786-b13a3fd7cf18)


I downloaded the dataset from Kaggle (https://www.kaggle.com/datasets/datasnaek/youtube-new) and used 4 regions
