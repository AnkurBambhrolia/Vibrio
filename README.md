# 🌊 Vibrio vulnificus Risk Analysis  
**ASU Capstone Group 2 – Vibrio Data Repository**

**Contributors**: Ankur Bambhrolia, Dani Ramos, Sheila Resto Rodriguez, Alicia Acuña  

---

## 📌 Project Overview  
This capstone project analyzes the environmental risk factors associated with *Vibrio vulnificus*, a deadly marine bacterium whose prevalence is increasing due to climate change. The study integrates public health surveillance data with environmental datasets to understand trends and forecast future outbreaks along the Eastern U.S. coastline.

## Data Preocessing
🌡️ **[Sea Surface Temperature Data Processing](./preprocessing_sst.md)** — Process for obtaining and preparing Sea Surface Temperature data
🧂 **[Sea Surface Salinity Data Processing](./preprocessing_sss.md)** — Process for obtaining and preparing Sea Surface Salinity data

## Environmental Drivers of Vibrio vulnificus: A Multiplatform Data Analysis
We used tools such as Python (Google Colab), MySQL, and Power BI to clean, analyze, and visualize the data. The project is presented in multiple formats:

- 📄 **[Final Report (PDF)](./Vibrio_Vulnificus_Research_Paper.pdf)** — A comprehensive written analysis  
- 📊 **[Power BI Dashboard](./Vibrio_Dashboard.pbix)** — Visual exploration of trends, correlations, and forecasts  
- 📽️ **[Research Presentation](https://youtu.be/usGh6bckTsk)** — A presentation of our journey into Vibrio Vulnificus and ecoinformatics
- 🎬 **[Download the Presentation Video (MP4)](./Vibrio_Final_Research_Presentation.mp4)** — Download link for the Research presentaion (88.9 MB)
- :mag: **[Exploratory Data (ipynb)](./Exploratory_Data.ipynb)** — Google Colab Notebook Containing Code for our EDA
- :earth_americas: **[Geospatial Exploratory Data (ipynb)](./Vibrio_Geospatial_EDA.ipynb)** — Google Colab Notebook Containing Code for our Geospatial EDA
- :crystal_ball: **[Vibrio Forecasting Code (ipynb)](./Vibrio_Forecasting.ipynb)** — Google Colab Notebook Containing Code for forecasting SSS, SST and Vibrio Isolates 


---

## 🧪 Methods and Tools  
- **Data Sources**: CDC (BEAM), NOAA (SST), NASA (SSS)  
- **Languages/Tools**: Python (Pandas, NumPy, Scikit-learn, Seaborn), MySQL Workbench, Power BI  
- **Forecasting Models**: SARIMAX, Random Forest, XGBoost, Prophet  

---

## 🔍 Key Findings  
- Rising **Sea Surface Temperature (SST)** is strongly correlated with increased *V. vulnificus* cases  
- **Salinity (SSS)** shows a weaker but relevant inverse relationship  
- Time series forecasting predicts seasonal patterns with a gradual rise in risk  
- SARIMAX offered the most accurate forecast, incorporating both environmental and seasonal trends  

---

## 📂 Repository Contents  
- `Vibrio_Vulnificus_Research_Paper.pdf` — Final written report  
- `Dashboard.pbix` — Power BI dashboard file  
- `notebooks/` — Python scripts (Google Colab format)  
- `data/` — Cleaned and raw datasets  

---

## 📬 Contact  
Have questions or feedback? Feel free to reach out:  
📧 **abambhrolia@gmail.com**
