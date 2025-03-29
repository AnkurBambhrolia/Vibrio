#account was created to NASAs EarthData
#After creating account salinity data was found on https://search.earthdata.nasa.gov/search/granules?p=C2589165108-POCLOUD&pg[0][v]=f&tl=1743225748.026!3!!
#the following data set was downloaded OISSS_L4_multimission_monthly_v2_2.0
#a total of 150 files were downloaded ranging from 2011-2024

# steps to convert netCDF files to csv files
import xarray as xr
import os

#filepath to the NetCDF files 
data_dir = "/Users/danielaramos/Downloads/OISSS_L4_multimission_monthly_v2_2.0-20250217_070629"

#list of file paths
file_list = [os.path.join(data_dir, file) for file in os.listdir(data_dir) if file.endswith(".nc")]

#Open multiple NetCDF files as a single dataset, concatenating along the coordinates
ds = xr.open_mfdataset(file_list, combine="by_coords")

#concatenated dataset to a new NetCDF file
ds.to_netcdf("concatenated_data.nc")

#Print dataset info to verify
print(ds)
<xarray.Dataset>
Dimensions:                    (longitude: 1440, latitude: 720, time: 150)
Coordinates:
  * longitude                  (longitude) float32 -179.9 -179.6 ... 179.6 179.9
  * latitude                   (latitude) float32 -89.88 -89.62 ... 89.62 89.88
  * time                       (time) datetime64[ns] 2011-09-16 ... 2024-02-15
Data variables:
    sss                        (latitude, longitude, time) float32 dask.array<chunksize=(720, 1440, 1), meta=np.ndarray>
    sss_empirical_uncertainty  (latitude, longitude, time) float32 dask.array<chunksize=(720, 1440, 1), meta=np.ndarray>
    sss_formal_uncertainty     (latitude, longitude, time) float32 dask.array<chunksize=(720, 1440, 1), meta=np.ndarray>
    sss_climatology            (latitude, longitude, time) float32 dask.array<chunksize=(720, 1440, 1), meta=np.ndarray>
    sss_anomaly                (latitude, longitude, time) float32 dask.array<chunksize=(720, 1440, 1), meta=np.ndarray>
Attributes: (12/42)
    Conventions:                   CF-1.8, ACDD-1.3
    standard_name_vocabulary:      CF Standard Name Table v27
    Title:                         Multi-Mission Optimally Interpolated Sea S...
    Short_Name:                    OISSS_L4_multimission_v2_monthly
    Version:                       V2.0
    Processing_Level:              Level 4
    ...                            ...
    geospatial_lat_resolution:     0.25
    geospatial_lat_units:          degrees_north
    geospatial_lon_min:            -180.0
    geospatial_lon_max:            180.0
    geospatial_lon_resolution:     0.25
    geospatial_lon_units:          degrees_east
import pandas as pd

#Convert xarray Dataset to pandas DataFrame
df = ds.to_dataframe().reset_index()

#Display the first few rows (optional)
print(df.head())
   longitude  latitude       time  sss  sss_empirical_uncertainty  \
0   -179.875   -89.875 2011-09-16  NaN                        NaN   
1   -179.875   -89.875 2011-10-16  NaN                        NaN   
2   -179.875   -89.875 2011-11-16  NaN                        NaN   
3   -179.875   -89.875 2011-12-16  NaN                        NaN   
4   -179.875   -89.875 2012-01-16  NaN                        NaN   

   sss_formal_uncertainty  sss_climatology  sss_anomaly  
0                     NaN              NaN          NaN  
1                     NaN              NaN          NaN  
2                     NaN              NaN          NaN  
3                     NaN              NaN          NaN  
4                     NaN              NaN          NaN  
output_csv_ = "concatenated_oisss_data_.csv"
df.to_csv(output_csv_, index=False)
print(f"CSV file saved successfully as {output_csv_}")
CSV file saved successfully as concatenated_oisss_data_.csv

# import new csv file from the concatenated netcdf files
import pandas as pd 
data_frame=pd.read_csv("concatenated_oisss_data_.csv")
data_frame=data_frame.dropna(subset=['sss']) #drop empty rows from the column sss
data_frame=data_frame.reset_index(drop=True) #reset the index of the file 
print(data_frame)
          longitude  latitude        time        sss  \
0          -179.875   -77.875  2013-12-16  34.228367   
1          -179.875   -77.875  2018-01-16  34.262707   
2          -179.875   -77.875  2018-12-16  34.331047   
3          -179.875   -77.875  2019-01-16  34.105392   
4          -179.875   -77.875  2019-12-16  34.008965   
...             ...       ...         ...        ...   
80441150    179.875    77.375  2012-08-16  27.949165   
80441151    179.875    77.375  2012-09-16  27.920988   
80441152    179.875    77.375  2020-09-15  27.146154   
80441153    179.875    77.625  2012-08-16  27.946297   
80441154    179.875    77.625  2012-09-16  27.888905   

          sss_empirical_uncertainty  sss_formal_uncertainty  sss_climatology  \
0                          0.122035                0.097519              NaN   
1                          0.122035                0.463454              NaN   
2                          0.122035                0.224755              NaN   
3                          0.122035                0.346862              NaN   
4                          0.122035                0.366728              NaN   
...                             ...                     ...              ...   
80441150                        NaN                0.415122              NaN   
80441151                        NaN                0.391366              NaN   
80441152                        NaN                0.190532              NaN   
80441153                        NaN                0.457406              NaN   
80441154                        NaN                0.329395              NaN   

          sss_anomaly  
0                 NaN  
1                 NaN  
2                 NaN  
3                 NaN  
4                 NaN  
...               ...  
80441150          NaN  
80441151          NaN  
80441152          NaN  
80441153          NaN  
80441154          NaN  

[80441155 rows x 8 columns]

data_frame.to_csv("oisss_data_removed_sss_nan_.csv",index=False) #save file 


df=pd.read_csv("oisss_data_removed_sss_nan_.csv")
df= df[(df["longitude"]>= -99.875) & (df["longitude"] <=-60.125) & (df["latitude"]>= 22.125) & (df["latitude"] <=49.875)] #filter east coast/gulf coordinates 
print(df)
          longitude  latitude        time        sss  \
22758761    -97.375    22.125  2011-09-16  36.573425   
22758762    -97.375    22.125  2011-10-16  36.402847   
22758763    -97.375    22.125  2011-11-16  36.264780   
22758764    -97.375    22.125  2011-12-16  36.279392   
22758765    -97.375    22.125  2012-01-16  36.285408   
...             ...       ...         ...        ...   
29609827    -60.125    49.875  2023-10-16  30.572357   
29609828    -60.125    49.875  2023-11-16  30.969070   
29609829    -60.125    49.875  2023-12-16  31.247175   
29609830    -60.125    49.875  2024-01-16  31.846640   
29609831    -60.125    49.875  2024-02-15  31.153300   

          sss_empirical_uncertainty  sss_formal_uncertainty  sss_climatology  \
22758761                   0.139093                0.370939        36.434680   
22758762                   0.139093                0.225227        36.330574   
22758763                   0.139093                0.114375        36.154667   
22758764                   0.139093                0.122853        36.152550   
22758765                   0.139093                0.318495        36.188870   
...                             ...                     ...              ...   
29609827                   0.392585                0.331747        31.008572   
29609828                   0.392585                0.431415        31.365828   
29609829                   0.392585                0.479736        31.429064   
29609830                   0.392585                0.611493        31.616940   
29609831                   0.392585                0.576688        31.786175   

          sss_anomaly  
22758761     0.138745  
22758762     0.072275  
22758763     0.110112  
22758764     0.126842  
22758765     0.096538  
...               ...  
29609827    -0.436214  
29609828    -0.396757  
29609829    -0.181888  
29609830     0.229700  
29609831    -0.632877  

[1187160 rows x 8 columns]
df.to_csv("oisss_eastcoast_gulf_data_.csv",index=False)###file with refined range of coordinates

import pandas as pd
df= pd.read_csv("oisss_eastcoast_gulf_data_.csv")
df["time"] = pd.to_datetime(df["time"])  # Convert time frame
df["year"]=df["time"].dt.year ##added year column
print(df)
df["month"]=df["time"].dt.month ##added month column
print(df)
df = df.drop(columns=["time"])
print(df)
df.to_csv("sss_eastcoast_gulf_data_cleaned__.csv", index= False) ###removed time column as month and year were split into separate columns 
         longitude  latitude       time        sss  sss_empirical_uncertainty  \
0          -97.375    22.125 2011-09-16  36.573425                   0.139093   
1          -97.375    22.125 2011-10-16  36.402847                   0.139093   
2          -97.375    22.125 2011-11-16  36.264780                   0.139093   
3          -97.375    22.125 2011-12-16  36.279392                   0.139093   
4          -97.375    22.125 2012-01-16  36.285408                   0.139093   
...            ...       ...        ...        ...                        ...   
1187155    -60.125    49.875 2023-10-16  30.572357                   0.392585   
1187156    -60.125    49.875 2023-11-16  30.969070                   0.392585   
1187157    -60.125    49.875 2023-12-16  31.247175                   0.392585   
1187158    -60.125    49.875 2024-01-16  31.846640                   0.392585   
1187159    -60.125    49.875 2024-02-15  31.153300                   0.392585   

         sss_formal_uncertainty  sss_climatology  sss_anomaly  year  
0                      0.370939        36.434680     0.138745  2011  
1                      0.225227        36.330574     0.072275  2011  
2                      0.114375        36.154667     0.110112  2011  
3                      0.122853        36.152550     0.126842  2011  
4                      0.318495        36.188870     0.096538  2012  
...                         ...              ...          ...   ...  
1187155                0.331747        31.008572    -0.436214  2023  
1187156                0.431415        31.365828    -0.396757  2023  
1187157                0.479736        31.429064    -0.181888  2023  
1187158                0.611493        31.616940     0.229700  2024  
1187159                0.576688        31.786175    -0.632877  2024  

[1187160 rows x 9 columns]
         longitude  latitude       time        sss  sss_empirical_uncertainty  \
0          -97.375    22.125 2011-09-16  36.573425                   0.139093   
1          -97.375    22.125 2011-10-16  36.402847                   0.139093   
2          -97.375    22.125 2011-11-16  36.264780                   0.139093   
3          -97.375    22.125 2011-12-16  36.279392                   0.139093   
4          -97.375    22.125 2012-01-16  36.285408                   0.139093   
...            ...       ...        ...        ...                        ...   
1187155    -60.125    49.875 2023-10-16  30.572357                   0.392585   
1187156    -60.125    49.875 2023-11-16  30.969070                   0.392585   
1187157    -60.125    49.875 2023-12-16  31.247175                   0.392585   
1187158    -60.125    49.875 2024-01-16  31.846640                   0.392585   
1187159    -60.125    49.875 2024-02-15  31.153300                   0.392585   

         sss_formal_uncertainty  sss_climatology  sss_anomaly  year  month  
0                      0.370939        36.434680     0.138745  2011      9  
1                      0.225227        36.330574     0.072275  2011     10  
2                      0.114375        36.154667     0.110112  2011     11  
3                      0.122853        36.152550     0.126842  2011     12  
4                      0.318495        36.188870     0.096538  2012      1  
...                         ...              ...          ...   ...    ...  
1187155                0.331747        31.008572    -0.436214  2023     10  
1187156                0.431415        31.365828    -0.396757  2023     11  
1187157                0.479736        31.429064    -0.181888  2023     12  
1187158                0.611493        31.616940     0.229700  2024      1  
1187159                0.576688        31.786175    -0.632877  2024      2  

[1187160 rows x 10 columns]
         longitude  latitude        sss  sss_empirical_uncertainty  \
0          -97.375    22.125  36.573425                   0.139093   
1          -97.375    22.125  36.402847                   0.139093   
2          -97.375    22.125  36.264780                   0.139093   
3          -97.375    22.125  36.279392                   0.139093   
4          -97.375    22.125  36.285408                   0.139093   
...            ...       ...        ...                        ...   
1187155    -60.125    49.875  30.572357                   0.392585   
1187156    -60.125    49.875  30.969070                   0.392585   
1187157    -60.125    49.875  31.247175                   0.392585   
1187158    -60.125    49.875  31.846640                   0.392585   
1187159    -60.125    49.875  31.153300                   0.392585   

         sss_formal_uncertainty  sss_climatology  sss_anomaly  year  month  
0                      0.370939        36.434680     0.138745  2011      9  
1                      0.225227        36.330574     0.072275  2011     10  
2                      0.114375        36.154667     0.110112  2011     11  
3                      0.122853        36.152550     0.126842  2011     12  
4                      0.318495        36.188870     0.096538  2012      1  
...                         ...              ...          ...   ...    ...  
1187155                0.331747        31.008572    -0.436214  2023     10  
1187156                0.431415        31.365828    -0.396757  2023     11  
1187157                0.479736        31.429064    -0.181888  2023     12  
1187158                0.611493        31.616940     0.229700  2024      1  
1187159                0.576688        31.786175    -0.632877  2024      2  

[1187160 rows x 9 columns]
df=pd.read_csv("sss_eastcoast_gulf_data_cleaned__.csv") 


## ADD STATE/REGION 
#create function for state
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

# import dataset 
import pandas as pd
df= pd.read_csv("sss_eastcoast_gulf_data_cleaned__.csv")
print(df)
         longitude  latitude        sss  sss_empirical_uncertainty  \
0          -97.375    22.125  36.573425                   0.139093   
1          -97.375    22.125  36.402847                   0.139093   
2          -97.375    22.125  36.264780                   0.139093   
3          -97.375    22.125  36.279392                   0.139093   
4          -97.375    22.125  36.285408                   0.139093   
...            ...       ...        ...                        ...   
1187155    -60.125    49.875  30.572357                   0.392585   
1187156    -60.125    49.875  30.969070                   0.392585   
1187157    -60.125    49.875  31.247175                   0.392585   
1187158    -60.125    49.875  31.846640                   0.392585   
1187159    -60.125    49.875  31.153300                   0.392585   

         sss_formal_uncertainty  sss_climatology  sss_anomaly  year  month  
0                      0.370939        36.434680     0.138745  2011      9  
1                      0.225227        36.330574     0.072275  2011     10  
2                      0.114375        36.154667     0.110112  2011     11  
3                      0.122853        36.152550     0.126842  2011     12  
4                      0.318495        36.188870     0.096538  2012      1  
...                         ...              ...          ...   ...    ...  
1187155                0.331747        31.008572    -0.436214  2023     10  
1187156                0.431415        31.365828    -0.396757  2023     11  
1187157                0.479736        31.429064    -0.181888  2023     12  
1187158                0.611493        31.616940     0.229700  2024      1  
1187159                0.576688        31.786175    -0.632877  2024      2  

[1187160 rows x 9 columns]
# apply function to dataset

df["state"] = df.apply(lambda row: assign_state(row['latitude'], row['longitude']), axis=1)
df.head()
longitude	latitude	sss	sss_empirical_uncertainty	sss_formal_uncertainty	sss_climatology	sss_anomaly	year	month	state
0	-97.375	22.125	36.573425	0.139093	0.370939	36.434680	0.138745	2011	9	Outside Study Area
1	-97.375	22.125	36.402847	0.139093	0.225227	36.330574	0.072275	2011	10	Outside Study Area
2	-97.375	22.125	36.264780	0.139093	0.114375	36.154667	0.110112	2011	11	Outside Study Area
3	-97.375	22.125	36.279392	0.139093	0.122853	36.152550	0.126842	2011	12	Outside Study Area
4	-97.375	22.125	36.285408	0.139093	0.318495	36.188870	0.096538	2012	1	Outside Study Area

# create region

def assign_region(state):
    if state in { "Connecticut", "Maine", "Massachusetts", "New Hampshire","Rhode Island","Vermont" }:
        return "New England"
    elif state in { "New Jersey", "New York", "Pennsylvania" }:
        return "Mid Atlantic"
    elif state in { "Delaware", "District of Columbia", "Florida", "Georgia", "Maryland", "North Carolina", "South Carolina", "Virginia", "West Virginia"}:
        return "South Atlantic"
    elif state in { "Alabama", "Kentucky", "Mississippi", "Tennessee"}:
        return "East South Central"
    elif state in { "Arkansas","Louisiana", "Oklahoma", "Texas"}:
        return "West South Central"
    
    return "Unknown"

# apply function
df["geographic division"] = df.apply(lambda row: assign_region(row["state"]), axis=1)
df.head()
longitude	latitude	sss	sss_empirical_uncertainty	sss_formal_uncertainty	sss_climatology	sss_anomaly	year	month	state	geographic division
0	-97.375	22.125	36.573425	0.139093	0.370939	36.434680	0.138745	2011	9	Outside Study Area	Unknown
1	-97.375	22.125	36.402847	0.139093	0.225227	36.330574	0.072275	2011	10	Outside Study Area	Unknown
2	-97.375	22.125	36.264780	0.139093	0.114375	36.154667	0.110112	2011	11	Outside Study Area	Unknown
3	-97.375	22.125	36.279392	0.139093	0.122853	36.152550	0.126842	2011	12	Outside Study Area	Unknown
4	-97.375	22.125	36.285408	0.139093	0.318495	36.188870	0.096538	2012	1	Outside Study Area	Unknown
df.to_csv("sss_eastcoast_gulf_2011_2024.csv",index=False) 
print(df)
         longitude  latitude        sss  sss_empirical_uncertainty  \
0          -97.375    22.125  36.573425                   0.139093   
1          -97.375    22.125  36.402847                   0.139093   
2          -97.375    22.125  36.264780                   0.139093   
3          -97.375    22.125  36.279392                   0.139093   
4          -97.375    22.125  36.285408                   0.139093   
...            ...       ...        ...                        ...   
1187155    -60.125    49.875  30.572357                   0.392585   
1187156    -60.125    49.875  30.969070                   0.392585   
1187157    -60.125    49.875  31.247175                   0.392585   
1187158    -60.125    49.875  31.846640                   0.392585   
1187159    -60.125    49.875  31.153300                   0.392585   

         sss_formal_uncertainty  sss_climatology  sss_anomaly  year  month  \
0                      0.370939        36.434680     0.138745  2011      9   
1                      0.225227        36.330574     0.072275  2011     10   
2                      0.114375        36.154667     0.110112  2011     11   
3                      0.122853        36.152550     0.126842  2011     12   
4                      0.318495        36.188870     0.096538  2012      1   
...                         ...              ...          ...   ...    ...   
1187155                0.331747        31.008572    -0.436214  2023     10   
1187156                0.431415        31.365828    -0.396757  2023     11   
1187157                0.479736        31.429064    -0.181888  2023     12   
1187158                0.611493        31.616940     0.229700  2024      1   
1187159                0.576688        31.786175    -0.632877  2024      2   

                      state geographic division  
0        Outside Study Area             Unknown  
1        Outside Study Area             Unknown  
2        Outside Study Area             Unknown  
3        Outside Study Area             Unknown  
4        Outside Study Area             Unknown  
...                     ...                 ...  
1187155  Outside Study Area             Unknown  
1187156  Outside Study Area             Unknown  
1187157  Outside Study Area             Unknown  
1187158  Outside Study Area             Unknown  
1187159  Outside Study Area             Unknown  

[1187160 rows x 11 columns]
# save file 
df= pd.read_csv("sss_eastcoast_gulf_2011_2024.csv")
df=df[df["year"]>= 2018] #filtered year so it matches the data from the beam dashboard 
print(df)
         longitude  latitude        sss  sss_empirical_uncertainty  \
76         -97.375    22.125  36.121117                   0.139093   
77         -97.375    22.125  36.224472                   0.139093   
78         -97.375    22.125  36.070534                   0.139093   
79         -97.375    22.125  36.173410                   0.139093   
80         -97.375    22.125  36.331900                   0.139093   
...            ...       ...        ...                        ...   
1187155    -60.125    49.875  30.572357                   0.392585   
1187156    -60.125    49.875  30.969070                   0.392585   
1187157    -60.125    49.875  31.247175                   0.392585   
1187158    -60.125    49.875  31.846640                   0.392585   
1187159    -60.125    49.875  31.153300                   0.392585   

         sss_formal_uncertainty  sss_climatology  sss_anomaly  year  month  \
76                     0.197707        36.188870    -0.067752  2018      1   
77                     0.204209        36.267480    -0.043005  2018      2   
78                     0.199547        36.218730    -0.148197  2018      3   
79                     0.197390        36.251507    -0.078097  2018      4   
80                     0.187805        36.179520     0.152382  2018      5   
...                         ...              ...          ...   ...    ...   
1187155                0.331747        31.008572    -0.436214  2023     10   
1187156                0.431415        31.365828    -0.396757  2023     11   
1187157                0.479736        31.429064    -0.181888  2023     12   
1187158                0.611493        31.616940     0.229700  2024      1   
1187159                0.576688        31.786175    -0.632877  2024      2   

                      state geographic division  
76       Outside Study Area             Unknown  
77       Outside Study Area             Unknown  
78       Outside Study Area             Unknown  
79       Outside Study Area             Unknown  
80       Outside Study Area             Unknown  
...                     ...                 ...  
1187155  Outside Study Area             Unknown  
1187156  Outside Study Area             Unknown  
1187157  Outside Study Area             Unknown  
1187158  Outside Study Area             Unknown  
1187159  Outside Study Area             Unknown  

[586128 rows x 11 columns]
df.to_csv("sss_eastcoast_gulf_2018_2024.csv",index=False) 
