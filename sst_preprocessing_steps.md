#import dataset from NOAA https://psl.noaa.gov/mddb2/makePlot.html?variableID=157658&fileID=1463540
#subset the data by selecting desired variables: time range, region, boundaries, projections, etc. 
#for this project date range was modified to (1990-01-01)-(2024-12-01) and region to Eastern US 
#data was the downloaded 

#jupyter notebook of the preprocessing steps 
import pandas as pd
df=pd.read_csv("/Users/danielaramos/Downloads/sst_eastcoast_gulf_1990_2024.csv")
print(df)
               time      lon     lat       sst
0        1990-01-01  260.125  22.125       NaN
1        1990-01-01  260.125  22.375       NaN
2        1990-01-01  260.125  22.625       NaN
3        1990-01-01  260.125  22.875       NaN
4        1990-01-01  260.125  23.125       NaN
...             ...      ...     ...       ...
7526395  2024-12-01  299.875  48.875  3.081613
7526396  2024-12-01  299.875  49.125  2.819032
7526397  2024-12-01  299.875  49.375  2.638387
7526398  2024-12-01  299.875  49.625  2.478387
7526399  2024-12-01  299.875  49.875  2.375484

[7526400 rows x 4 columns]
df=df.dropna(subset=['sst']) #remove empty sst rows 
df=df.reset_index(drop=True)
print(df)
               time      lon     lat        sst
0        1990-01-01  262.375  22.125  21.928709
1        1990-01-01  262.375  22.375  22.170000
2        1990-01-01  262.375  22.625  22.272257
3        1990-01-01  262.375  22.875  22.225483
4        1990-01-01  262.375  23.125  22.103548
...             ...      ...     ...        ...
3822835  2024-12-01  299.875  48.875   3.081613
3822836  2024-12-01  299.875  49.125   2.819032
3822837  2024-12-01  299.875  49.375   2.638387
3822838  2024-12-01  299.875  49.625   2.478387
3822839  2024-12-01  299.875  49.875   2.375484

[3822840 rows x 4 columns]
df.to_csv("sst_filtered_data_removed_nan_.csv",index=False) ##removed empty sst fields & saved in a new csv file
df["standard longitude"]= df["lon"]-360 ##converted to standard longitude
print(df)
               time      lon     lat        sst  standard longitude
0        1990-01-01  262.375  22.125  21.928709             -97.625
1        1990-01-01  262.375  22.375  22.170000             -97.625
2        1990-01-01  262.375  22.625  22.272257             -97.625
3        1990-01-01  262.375  22.875  22.225483             -97.625
4        1990-01-01  262.375  23.125  22.103548             -97.625
...             ...      ...     ...        ...                 ...
3822835  2024-12-01  299.875  48.875   3.081613             -60.125
3822836  2024-12-01  299.875  49.125   2.819032             -60.125
3822837  2024-12-01  299.875  49.375   2.638387             -60.125
3822838  2024-12-01  299.875  49.625   2.478387             -60.125
3822839  2024-12-01  299.875  49.875   2.375484             -60.125

[3822840 rows x 5 columns]
df.to_csv("sst_east_coast_clean_data_.csv",index=False) #the newfile with standard longitude & removed the "lon" column
import pandas as pd
df= pd.read_csv("sst_east_coast_clean_data_.csv")
print(df)
               time      lon     lat        sst  standard longitude
0        1990-01-01  262.375  22.125  21.928709             -97.625
1        1990-01-01  262.375  22.375  22.170000             -97.625
2        1990-01-01  262.375  22.625  22.272257             -97.625
3        1990-01-01  262.375  22.875  22.225483             -97.625
4        1990-01-01  262.375  23.125  22.103548             -97.625
...             ...      ...     ...        ...                 ...
3822835  2024-12-01  299.875  48.875   3.081613             -60.125
3822836  2024-12-01  299.875  49.125   2.819032             -60.125
3822837  2024-12-01  299.875  49.375   2.638387             -60.125
3822838  2024-12-01  299.875  49.625   2.478387             -60.125
3822839  2024-12-01  299.875  49.875   2.375484             -60.125

[3822840 rows x 5 columns]
df["time"] = pd.to_datetime(df["time"]) ##converted to time format
df["year"]=df["time"].dt.year ##added year column
print(df)
              time      lon     lat        sst  standard longitude  year
0       1990-01-01  262.375  22.125  21.928709             -97.625  1990
1       1990-01-01  262.375  22.375  22.170000             -97.625  1990
2       1990-01-01  262.375  22.625  22.272257             -97.625  1990
3       1990-01-01  262.375  22.875  22.225483             -97.625  1990
4       1990-01-01  262.375  23.125  22.103548             -97.625  1990
...            ...      ...     ...        ...                 ...   ...
3822835 2024-12-01  299.875  48.875   3.081613             -60.125  2024
3822836 2024-12-01  299.875  49.125   2.819032             -60.125  2024
3822837 2024-12-01  299.875  49.375   2.638387             -60.125  2024
3822838 2024-12-01  299.875  49.625   2.478387             -60.125  2024
3822839 2024-12-01  299.875  49.875   2.375484             -60.125  2024

[3822840 rows x 6 columns]
df["month"]=df["time"].dt.month ##added month column
print(df)
              time      lon     lat        sst  standard longitude  year  \
0       1990-01-01  262.375  22.125  21.928709             -97.625  1990   
1       1990-01-01  262.375  22.375  22.170000             -97.625  1990   
2       1990-01-01  262.375  22.625  22.272257             -97.625  1990   
3       1990-01-01  262.375  22.875  22.225483             -97.625  1990   
4       1990-01-01  262.375  23.125  22.103548             -97.625  1990   
...            ...      ...     ...        ...                 ...   ...   
3822835 2024-12-01  299.875  48.875   3.081613             -60.125  2024   
3822836 2024-12-01  299.875  49.125   2.819032             -60.125  2024   
3822837 2024-12-01  299.875  49.375   2.638387             -60.125  2024   
3822838 2024-12-01  299.875  49.625   2.478387             -60.125  2024   
3822839 2024-12-01  299.875  49.875   2.375484             -60.125  2024   

         month  
0            1  
1            1  
2            1  
3            1  
4            1  
...        ...  
3822835     12  
3822836     12  
3822837     12  
3822838     12  
3822839     12  

[3822840 rows x 7 columns]
df = df.drop(columns=["time"]) ##removed the time column as i already add year/month columns
print(df)
             lon     lat        sst  standard longitude  year  month
0        262.375  22.125  21.928709             -97.625  1990      1
1        262.375  22.375  22.170000             -97.625  1990      1
2        262.375  22.625  22.272257             -97.625  1990      1
3        262.375  22.875  22.225483             -97.625  1990      1
4        262.375  23.125  22.103548             -97.625  1990      1
...          ...     ...        ...                 ...   ...    ...
3822835  299.875  48.875   3.081613             -60.125  2024     12
3822836  299.875  49.125   2.819032             -60.125  2024     12
3822837  299.875  49.375   2.638387             -60.125  2024     12
3822838  299.875  49.625   2.478387             -60.125  2024     12
3822839  299.875  49.875   2.375484             -60.125  2024     12

[3822840 rows x 6 columns]
print(df)
df.to_csv("sst_gulf_eastcoast_1990_2024_.csv",index=False)
             lon     lat        sst  standard longitude  year  month
0        262.375  22.125  21.928709             -97.625  1990      1
1        262.375  22.375  22.170000             -97.625  1990      1
2        262.375  22.625  22.272257             -97.625  1990      1
3        262.375  22.875  22.225483             -97.625  1990      1
4        262.375  23.125  22.103548             -97.625  1990      1
...          ...     ...        ...                 ...   ...    ...
3822835  299.875  48.875   3.081613             -60.125  2024     12
3822836  299.875  49.125   2.819032             -60.125  2024     12
3822837  299.875  49.375   2.638387             -60.125  2024     12
3822838  299.875  49.625   2.478387             -60.125  2024     12
3822839  299.875  49.875   2.375484             -60.125  2024     12

[3822840 rows x 6 columns]
df=pd.read_csv("sst_gulf_eastcoast_1990_2024_.csv")
df=df[df["year"]>= 2018] #filtered year so it matches the data from the beam dashboard 
print(df)
             lon     lat        sst  standard longitude  year  month
3058272  262.375  22.125  22.196451             -97.625  2018      1
3058273  262.375  22.375  21.989677             -97.625  2018      1
3058274  262.375  22.625  21.612257             -97.625  2018      1
3058275  262.375  22.875  21.104193             -97.625  2018      1
3058276  262.375  23.125  20.624838             -97.625  2018      1
...          ...     ...        ...                 ...   ...    ...
3822835  299.875  48.875   3.081613             -60.125  2024     12
3822836  299.875  49.125   2.819032             -60.125  2024     12
3822837  299.875  49.375   2.638387             -60.125  2024     12
3822838  299.875  49.625   2.478387             -60.125  2024     12
3822839  299.875  49.875   2.375484             -60.125  2024     12

[764568 rows x 6 columns]
df.to_csv("sst_eastcoast_gulf_data_2018_2024__.csv",index=False)


## ADD STATE/REGION TO DATASET

import pandas as pd
df=pd.read_csv("sst_eastcoast_gulf_data_2018_2024__.csv")
print(df)
            lon     lat        sst  standard longitude  year  month
0       262.375  22.125  22.196451             -97.625  2018      1
1       262.375  22.375  21.989677             -97.625  2018      1
2       262.375  22.625  21.612257             -97.625  2018      1
3       262.375  22.875  21.104193             -97.625  2018      1
4       262.375  23.125  20.624838             -97.625  2018      1
...         ...     ...        ...                 ...   ...    ...
764563  299.875  48.875   3.081613             -60.125  2024     12
764564  299.875  49.125   2.819032             -60.125  2024     12
764565  299.875  49.375   2.638387             -60.125  2024     12
764566  299.875  49.625   2.478387             -60.125  2024     12
764567  299.875  49.875   2.375484             -60.125  2024     12

[764568 rows x 6 columns]

## Create state function using an US State Bounding Box csv from https://gist.github.com/a8dx/2340f9527af64f8ef8439366de981168

def assign_state(lat,lon):
    if 30.223 <= lat <=35.008 and -88.473 <= lon <= -84.889:
        return "Alabama"
        
    elif 40.980 <= lat <= 42.050 and -73.727 <=lon <= -71.786 :
        return "Connecticut"
    elif 38.451 <= lat <= 39.839 and -75.788 <=lon <= -75.048:
        return "Delaware"
    elif 24.523 <= lat <= 31.000 and -87.634 <=lon <= -80.031:
            return "Florida"
    elif 30.357 <= lat <= 35.000 and -85.605 <=lon <= -80.839:
            return "Georgia"
    elif 28.928 <= lat <= 33.019  and -94.043 <=lon <= -88.817:
            return "Louisiana"
    elif 42.977 <= lat <= 47.459 and -71.083 <=lon <= -66.949:
            return "Maine"
    elif 37.911 <= lat <= 39.723 and -79.487 <=lon <= -75.048 :
        return "Maryland"
    elif 41.237 <= lat <= 42.886 and -73.508 <=lon <= -69.928:
        return "Massachusetts"
    elif 30.173 <= lat <= 34.996 and -91.655 <=lon <= -88.097:
            return "Mississippi"
    elif 42.696  <= lat <= 45.305 and -72.557 <=lon <= -70.610 :
            return "New Hampshire"
    elif 38.928 <= lat <= 41.357  and -75.559 <=lon <= -73.893:
            return "New Jersey"
    elif  40.496 <= lat <= 45.015 and -79.762 <=lon <= -71.856 :
            return "New York"
    elif 33.842 <= lat <= 36.588 and  -84.321<=lon <= -75.460:
            return "North Carolina"
    elif 41.146 <= lat <= 42.0187  and -71.862 <=lon <= -71.120:
            return "Rhode Island"
    elif 32.034 <= lat <= 35.215 and -83.353 <=lon <= -78.542:
            return "South Carolina"    
    elif 25.837 <= lat <= 36.500  and -106.645 <=lon <= -93.508 :
            return "Texas"
    elif 36.540 <=  lat <= 39.466  and -83.675 <=lon <= -75.242:
            return "Virginia"
    else:
        return "Outside Study Area"

## Apply function to data file (sst_eastcoast_gulf_data_2018_2024__.csv)
df["state"] = df.apply(lambda row: assign_state(row['lat'], row['standard longitude']), axis=1)
df.head()
lon	lat	sst	standard longitude	year	month	state
0	262.375	22.125	22.196451	-97.625	2018	1	Outside Study Area
1	262.375	22.375	21.989677	-97.625	2018	1	Outside Study Area
2	262.375	22.625	21.612257	-97.625	2018	1	Outside Study Area
3	262.375	22.875	21.104193	-97.625	2018	1	Outside Study Area
4	262.375	23.125	20.624838	-97.625	2018	1	Outside Study Area


## Function for Region 
def assign_region(lat,lon):
    if 24 <= lat <=50 and -100 <= lon <= -60:
        if 24 <= lat <= 31 and -98 <=lon <=-80:
            return "Gulf Coast"
        elif 31 <= lat <= 37 and -80 <=lon <= -75:
             return "Southeast Coast"
        elif 37 <= lat <= 44 and -75 <=lon <= -69:
            return "MidAtlantic"
        elif 44 <= lat <= 50 and -69 <=lon <= -60:
            return "NorthEast Coast"
        else:
            return "Other East Coast"
    else:
        return "Outside Study Area"


            
## Apply Function to data file (sst_eastcoast_gulf_data_2018_2024__.csv)     
df["region"] = df.apply(lambda row: assign_region(row['lat'], row['standard longitude']), axis=1)
df.head()
lon	lat	sst	standard longitude	year	month	state	region
0	262.375	22.125	22.196451	-97.625	2018	1	Outside Study Area	Outside Study Area
1	262.375	22.375	21.989677	-97.625	2018	1	Outside Study Area	Outside Study Area
2	262.375	22.625	21.612257	-97.625	2018	1	Outside Study Area	Outside Study Area
3	262.375	22.875	21.104193	-97.625	2018	1	Outside Study Area	Outside Study Area
4	262.375	23.125	20.624838	-97.625	2018	1	Outside Study Area	Outside Study Area
print(df)
            lon     lat        sst  standard longitude  year  month  \
0       262.375  22.125  22.196451             -97.625  2018      1   
1       262.375  22.375  21.989677             -97.625  2018      1   
2       262.375  22.625  21.612257             -97.625  2018      1   
3       262.375  22.875  21.104193             -97.625  2018      1   
4       262.375  23.125  20.624838             -97.625  2018      1   
...         ...     ...        ...                 ...   ...    ...   
764563  299.875  48.875   3.081613             -60.125  2024     12   
764564  299.875  49.125   2.819032             -60.125  2024     12   
764565  299.875  49.375   2.638387             -60.125  2024     12   
764566  299.875  49.625   2.478387             -60.125  2024     12   
764567  299.875  49.875   2.375484             -60.125  2024     12   

                     state              region  
0       Outside Study Area  Outside Study Area  
1       Outside Study Area  Outside Study Area  
2       Outside Study Area  Outside Study Area  
3       Outside Study Area  Outside Study Area  
4       Outside Study Area  Outside Study Area  
...                    ...                 ...  
764563  Outside Study Area     NorthEast Coast  
764564  Outside Study Area     NorthEast Coast  
764565  Outside Study Area     NorthEast Coast  
764566  Outside Study Area     NorthEast Coast  
764567  Outside Study Area     NorthEast Coast  

[764568 rows x 8 columns]

## Save to new data file 

df.to_csv("sst_by_state_region_2018_2024.csv",index=False)

