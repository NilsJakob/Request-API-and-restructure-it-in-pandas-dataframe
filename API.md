# How to retrieve information with API and restructure it in pandas


```python
import requests
import json
import pandas as pd
```


```python
url = ' https://api.nve.no/web/Powerplant/GetHydroPowerPlantsInOperation'
#url = ' https://api.nve.no/web/Powerplant/'
response = requests.get(url)


data = response.json()
response.text
df=pd.DataFrame(data)
print(df.head())
list(df)

```

       VannKraftverkID          Navn VannKVType VannKVTypeID  \
    0                2      Adamselv  Kraftverk            K   
    1             1268           Aga  Kraftverk            K   
    2             1527      Aklestad  Kraftverk            K   
    3             1677  Akslandselva  Kraftverk            K   
    4             2054           Ala  Kraftverk            K   
    
                       HovedEier HovedEier_OrgNr            Fylke FylkesNr  \
    0        STATKRAFT ENERGI AS       987059729         Finnmark       56   
    1                                                    Vestland       46   
    2            HÅVARD AKLESTAD       988117722  Møre og Romsdal       15   
    3  AKSLANDSELVA KRAFTVERK AS       991005625         Vestland       46   
    4          SKAGERAK KRAFT AS       979563531        Innlandet       34   
    
       Kommune KommuneNr  ...  Kraftverkstatus  NVEOmraadeID  NVEOmraadeNavn  \
    0  Lebesby        24  ...           Idrift          None            None   
    1     Voss        21  ...           Idrift          None            None   
    2    Ørsta        20  ...           Idrift          None            None   
    3     Etne        11  ...           Idrift          None            None   
    4     Vang        54  ...           Idrift          None            None   
    
          Nedborsfeltnavn  SPPunkt  SPSone  UnderBygging UteAvDrift  \
    0      Friarfjordelva       01     893         False       None   
    1   Granvinvassdraget     None     999         False       None   
    2         Bondalselva     None     999         False       None   
    3        Akslandselva     None     999         False       None   
    4  Drammensvassdraget     None     043         False       None   
    
      VassdragsOmraadeID  VassdragsOmraadeNavn  
    0                 12      NVE - område: 12  
    1                  6       NVE - område: 6  
    2                  8       NVE - område: 8  
    3                  5       NVE - område: 5  
    4                  1       NVE - område: 1  
    
    [5 rows x 32 columns]
    




    ['VannKraftverkID',
     'Navn',
     'VannKVType',
     'VannKVTypeID',
     'HovedEier',
     'HovedEier_OrgNr',
     'Fylke',
     'FylkesNr',
     'Kommune',
     'KommuneNr',
     'ForsteUtnyttelseAvFalletDato',
     'DatoForEldsteKraftproduserendeDel',
     'MaksYtelse',
     'MidProd_91_20',
     'BruttoFallhoyde_M',
     'Slukeevne',
     'EnEkv',
     'ElspotomraadeNummer',
     'RegineNr',
     'ErIDrift',
     'IDriftDato',
     'Konsesjoner',
     'Kraftverkstatus',
     'NVEOmraadeID',
     'NVEOmraadeNavn',
     'Nedborsfeltnavn',
     'SPPunkt',
     'SPSone',
     'UnderBygging',
     'UteAvDrift',
     'VassdragsOmraadeID',
     'VassdragsOmraadeNavn']




```python
# Define the threshold value
threshold = 370

# Filter rows where 'Score' is greater than 90 and select 'Name' and 'City' columns
filtered_df = df.loc[df['MaksYtelse'] > threshold][['Navn', 'HovedEier', 'MaksYtelse', 'ElspotomraadeNummer','Nedborsfeltnavn']].sort_values(by='MaksYtelse', ascending=False)

print(filtered_df)
```

               Navn                HovedEier  MaksYtelse ElspotomraadeNummer  \
    799    Kvilldal      STATKRAFT ENERGI AS      1240.0                   2   
    1558    Tonstad  SIRA KVINA KRAFTSELSKAP       960.0                   2   
    22    Aurland 1        HAFSLUND KRAFT AS       840.0                   5   
    1263    Saurdal      STATKRAFT ENERGI AS       640.0                   2   
    1484    Sy-Sima      STATKRAFT ENERGI AS       620.0                   5   
    1462  Svartisen      STATKRAFT ENERGI AS       600.0                   4   
    831   Lang-Sima      STATKRAFT ENERGI AS       500.0                   5   
    1136       Rana      STATKRAFT ENERGI AS       500.0                   4   
    1555      Tokke      STATKRAFT ENERGI AS       430.0                   2   
    1617       Tyin          HYDRO ENERGI AS       374.0                   5   
    
             Nedborsfeltnavn  
    799    Suldalsvassdraget  
    1558                Sira  
    22    Aurlandsvassdraget  
    1263   Suldalsvassdraget  
    1484      Simavassdraget  
    1462        Storflatelva  
    831       Osa-vassdraget  
    1136      Ranavassdraget  
    1555    Skiensvassdraget  
    1617    Årdalsvassdraget  
    


```python
# Filter for rows where 'City' is 'New York'
filtered_df = df.loc[df['HovedEier'] == 'HYDRO ENERGI AS'][['Navn', 'HovedEier', 'MaksYtelse', 'ElspotomraadeNummer']]
print(filtered_df)
```

               Navn        HovedEier  MaksYtelse ElspotomraadeNummer
    302    Fivlemyr  HYDRO ENERGI AS        2.00                   5
    369    Frøystul  HYDRO ENERGI AS       45.60                   2
    552       Herva  HYDRO ENERGI AS       33.00                   5
    589     Holsbru  HYDRO ENERGI AS       48.90                   5
    916   Mannsberg  HYDRO ENERGI AS        3.52                   5
    960      Moflåt  HYDRO ENERGI AS       30.00                   2
    991         Mæl  HYDRO ENERGI AS       37.50                   2
    1295     Skagen  HYDRO ENERGI AS      270.00                   5
    1475  Svelgfoss  HYDRO ENERGI AS       92.00                   2
    1513     Såheim  HYDRO ENERGI AS      189.00                   2
    1617       Tyin  HYDRO ENERGI AS      374.00                   5
    1699     Vemork  HYDRO ENERGI AS      204.00                   2
    


```python
filtered_df = df[(df['HovedEier'] == 'HYDRO ENERGI AS') & (df['Nedborsfeltnavn'] == 'Skiensvassdraget')][['Navn', 'HovedEier', 'MaksYtelse', 'ElspotomraadeNummer','Nedborsfeltnavn']]
print(filtered_df)
```

               Navn        HovedEier  MaksYtelse ElspotomraadeNummer  \
    369    Frøystul  HYDRO ENERGI AS        45.6                   2   
    960      Moflåt  HYDRO ENERGI AS        30.0                   2   
    991         Mæl  HYDRO ENERGI AS        37.5                   2   
    1475  Svelgfoss  HYDRO ENERGI AS        92.0                   2   
    1513     Såheim  HYDRO ENERGI AS       189.0                   2   
    1699     Vemork  HYDRO ENERGI AS       204.0                   2   
    
           Nedborsfeltnavn  
    369   Skiensvassdraget  
    960   Skiensvassdraget  
    991   Skiensvassdraget  
    1475  Skiensvassdraget  
    1513  Skiensvassdraget  
    1699  Skiensvassdraget  
    


```python
url = 'http://api.tvmaze.com/singlesearch/shows'
params = {'q':'Girls'}
response = requests.get(url,params)
if response.status_code == 200:
    print(response.text)
else:
    print(f'Error: {response.status_code}')
```


```python
url = 'https://api.nve.no/web/WindPowerplant/GetWindPowerPlantsInOperation'

response = requests.get(url)


data = response.json()
response.text
df=pd.DataFrame(data)
df
```


```python
url = 'https://api.nve.no/web/Powerplant/GetHydroPowerPlantsInOperation'
params = {'q':'Girls'}
response = requests.get(url,params)
if response.status_code == 200:
    print(response.text)
else:
    print(f'Error: {response.status_code}')
```


```python
import requests

url = 'https://api.nve.no/web/Powerplant'
params = {
    "VannkraftverkID": 1
}

try:
    response = requests.get(url, params=params)
    if response.status_code == 200:
        data = response.json()
        print("API Response with Query Parameters:", data)
    else:
        print(f"Error: {response.status_code} - {response.text}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```


```python

```
