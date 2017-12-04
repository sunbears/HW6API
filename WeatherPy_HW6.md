

```python
#HW 6  WeatherPy
    #Create a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. 
    #Utilize a simple Python library, the OpenWeatherMap API, 
    #and a little common sense to create a representative model of weather across world cities.
```


```python
#Dependencies
import matplotlib.pyplot as plt
import seaborn as sns
import scipy #mandatory dependency in seaborn module, per seaborn documentation
import numpy as np
import pandas as pd
import requests as req
import json
import citipy # I had to enter in the Gitbash consule "pip install citipy", then return to the jupyter notebook.
from citipy import citipy #Needed this extra line of code in order for citipy to work
```


```python
#The apikeys module wasn't able to be installed for some reason. 
#I tried 'pip install apikeys' in my basic command prompt console within my PythonData environment but came across error:
    #Could not find a version that satisifes the requirement apikeys (from versions: ) No matching distribution found for apikeys
#So I tried another module called 'config' that I found from this website: http://www.blacktechdiva.com/hide-api-keys/
import config
```


```python
# METHOD 1: randomly select 600 cities using Citipy

# Randomly select latitude across the full range of latitude (-90 to 90)
lat_sample = np.random.randint(-90,90,600)
# Randomly select longtiude across the full range of longitude (-180 to 180)
lon_sample = np.random.randint(-90,90,600)
# Zip together to create a list of tuples
latlon_sample = list(zip(lat_sample, lon_sample))
```


```python
# ALTERNATIVE METHOD 2: At first, I didn't realize the instructions said to use Citipy. 
    # OpenWeatherAPI documentation referenced a list of cities here http://bulk.openweathermap.org/sample/  
    # I downloaded the city.list.json zip file and saved in the same directory.

#json_path = os.path.join('city.list.json')
#with open(json_path, 'r', encoding="utf8") as json_file:
#    cities_json = json.load(json_file)

#From looking at the first dictionary in the json list I see that 'name' holds the name of the city
#print(cities_json[0])

#Use a for loop to extract a random sample of city names 
#random_sample = np.random.randint(0,len(cities_json),600)
#cities_sample = []
#for x in random_sample:
#    city_name = cities_json[x]['name']
#    cities_sample.append(city_name)
```


```python
# Identify the city closest to each of the lat long pairs
citynames = []

for pair in latlon_sample:
    city = citipy.nearest_city(pair[0], pair[1])
    citynames.append(city.city_name)
```


```python
key = config.api_key
url = "http://api.openweathermap.org/data/2.5/weather"
params = {'appid': key,'q': '', 'units': 'metric'}

#Perform a weather check on each of the cities using a series of successive API calls.
    #Include a print log of each city as it's being processed with the city number, city name, and requested URL.
    #Temperature(F), Humidity(%), Cloudiness(%), Wind Speed(mph)

weather_data=[]
countnum = 0

for city in citynames:
    params['q'] = city
    response = req.get(url,params=params).json()
    weather_data.append(response)
    countnum = countnum +1
    print('Processing Record ' + str(countnum) + ' | ' + city + ' | url: api.openweathermap.org/data/2.5/weather?q=' + city)
```

    Processing Record 1 | mahebourg | url: api.openweathermap.org/data/2.5/weather?q=mahebourg
    Processing Record 2 | tumannyy | url: api.openweathermap.org/data/2.5/weather?q=tumannyy
    Processing Record 3 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 4 | mastic beach | url: api.openweathermap.org/data/2.5/weather?q=mastic beach
    Processing Record 5 | torbay | url: api.openweathermap.org/data/2.5/weather?q=torbay
    Processing Record 6 | feijo | url: api.openweathermap.org/data/2.5/weather?q=feijo
    Processing Record 7 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 8 | amalapuram | url: api.openweathermap.org/data/2.5/weather?q=amalapuram
    Processing Record 9 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 10 | east london | url: api.openweathermap.org/data/2.5/weather?q=east london
    Processing Record 11 | owosso | url: api.openweathermap.org/data/2.5/weather?q=owosso
    Processing Record 12 | ajdabiya | url: api.openweathermap.org/data/2.5/weather?q=ajdabiya
    Processing Record 13 | bathsheba | url: api.openweathermap.org/data/2.5/weather?q=bathsheba
    Processing Record 14 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 15 | morondava | url: api.openweathermap.org/data/2.5/weather?q=morondava
    Processing Record 16 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 17 | turgoyak | url: api.openweathermap.org/data/2.5/weather?q=turgoyak
    Processing Record 18 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 19 | havoysund | url: api.openweathermap.org/data/2.5/weather?q=havoysund
    Processing Record 20 | funadhoo | url: api.openweathermap.org/data/2.5/weather?q=funadhoo
    Processing Record 21 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 22 | umm kaddadah | url: api.openweathermap.org/data/2.5/weather?q=umm kaddadah
    Processing Record 23 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 24 | east london | url: api.openweathermap.org/data/2.5/weather?q=east london
    Processing Record 25 | tamandare | url: api.openweathermap.org/data/2.5/weather?q=tamandare
    Processing Record 26 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 27 | hamilton | url: api.openweathermap.org/data/2.5/weather?q=hamilton
    Processing Record 28 | babanusah | url: api.openweathermap.org/data/2.5/weather?q=babanusah
    Processing Record 29 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 30 | ulundi | url: api.openweathermap.org/data/2.5/weather?q=ulundi
    Processing Record 31 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 32 | verkhnyaya inta | url: api.openweathermap.org/data/2.5/weather?q=verkhnyaya inta
    Processing Record 33 | manaure | url: api.openweathermap.org/data/2.5/weather?q=manaure
    Processing Record 34 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 35 | port elizabeth | url: api.openweathermap.org/data/2.5/weather?q=port elizabeth
    Processing Record 36 | tambopata | url: api.openweathermap.org/data/2.5/weather?q=tambopata
    Processing Record 37 | atar | url: api.openweathermap.org/data/2.5/weather?q=atar
    Processing Record 38 | dudinka | url: api.openweathermap.org/data/2.5/weather?q=dudinka
    Processing Record 39 | barentsburg | url: api.openweathermap.org/data/2.5/weather?q=barentsburg
    Processing Record 40 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 41 | monrovia | url: api.openweathermap.org/data/2.5/weather?q=monrovia
    Processing Record 42 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 43 | ondangwa | url: api.openweathermap.org/data/2.5/weather?q=ondangwa
    Processing Record 44 | kopervik | url: api.openweathermap.org/data/2.5/weather?q=kopervik
    Processing Record 45 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 46 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 47 | kampene | url: api.openweathermap.org/data/2.5/weather?q=kampene
    Processing Record 48 | belushya guba | url: api.openweathermap.org/data/2.5/weather?q=belushya guba
    Processing Record 49 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 50 | savannah bight | url: api.openweathermap.org/data/2.5/weather?q=savannah bight
    Processing Record 51 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 52 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 53 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 54 | kosovska mitrovica | url: api.openweathermap.org/data/2.5/weather?q=kosovska mitrovica
    Processing Record 55 | nouadhibou | url: api.openweathermap.org/data/2.5/weather?q=nouadhibou
    Processing Record 56 | tazovskiy | url: api.openweathermap.org/data/2.5/weather?q=tazovskiy
    Processing Record 57 | agha jari | url: api.openweathermap.org/data/2.5/weather?q=agha jari
    Processing Record 58 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 59 | mudyuga | url: api.openweathermap.org/data/2.5/weather?q=mudyuga
    Processing Record 60 | senneterre | url: api.openweathermap.org/data/2.5/weather?q=senneterre
    Processing Record 61 | berlevag | url: api.openweathermap.org/data/2.5/weather?q=berlevag
    Processing Record 62 | lysva | url: api.openweathermap.org/data/2.5/weather?q=lysva
    Processing Record 63 | pisco | url: api.openweathermap.org/data/2.5/weather?q=pisco
    Processing Record 64 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 65 | port elizabeth | url: api.openweathermap.org/data/2.5/weather?q=port elizabeth
    Processing Record 66 | walvis bay | url: api.openweathermap.org/data/2.5/weather?q=walvis bay
    Processing Record 67 | dalen | url: api.openweathermap.org/data/2.5/weather?q=dalen
    Processing Record 68 | belushya guba | url: api.openweathermap.org/data/2.5/weather?q=belushya guba
    Processing Record 69 | camacha | url: api.openweathermap.org/data/2.5/weather?q=camacha
    Processing Record 70 | tessalit | url: api.openweathermap.org/data/2.5/weather?q=tessalit
    Processing Record 71 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 72 | shieli | url: api.openweathermap.org/data/2.5/weather?q=shieli
    Processing Record 73 | barhi | url: api.openweathermap.org/data/2.5/weather?q=barhi
    Processing Record 74 | gulshat | url: api.openweathermap.org/data/2.5/weather?q=gulshat
    Processing Record 75 | viedma | url: api.openweathermap.org/data/2.5/weather?q=viedma
    Processing Record 76 | barentsburg | url: api.openweathermap.org/data/2.5/weather?q=barentsburg
    Processing Record 77 | lokosovo | url: api.openweathermap.org/data/2.5/weather?q=lokosovo
    Processing Record 78 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 79 | acari | url: api.openweathermap.org/data/2.5/weather?q=acari
    Processing Record 80 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 81 | hamilton | url: api.openweathermap.org/data/2.5/weather?q=hamilton
    Processing Record 82 | bo | url: api.openweathermap.org/data/2.5/weather?q=bo
    Processing Record 83 | raga | url: api.openweathermap.org/data/2.5/weather?q=raga
    Processing Record 84 | tutoia | url: api.openweathermap.org/data/2.5/weather?q=tutoia
    Processing Record 85 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 86 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 87 | nokaneng | url: api.openweathermap.org/data/2.5/weather?q=nokaneng
    Processing Record 88 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 89 | larsnes | url: api.openweathermap.org/data/2.5/weather?q=larsnes
    Processing Record 90 | gubkinskiy | url: api.openweathermap.org/data/2.5/weather?q=gubkinskiy
    Processing Record 91 | santa isabel do rio negro | url: api.openweathermap.org/data/2.5/weather?q=santa isabel do rio negro
    Processing Record 92 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 93 | grand river south east | url: api.openweathermap.org/data/2.5/weather?q=grand river south east
    Processing Record 94 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 95 | luau | url: api.openweathermap.org/data/2.5/weather?q=luau
    Processing Record 96 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 97 | mahebourg | url: api.openweathermap.org/data/2.5/weather?q=mahebourg
    Processing Record 98 | tsihombe | url: api.openweathermap.org/data/2.5/weather?q=tsihombe
    Processing Record 99 | castro | url: api.openweathermap.org/data/2.5/weather?q=castro
    Processing Record 100 | jujuy | url: api.openweathermap.org/data/2.5/weather?q=jujuy
    Processing Record 101 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 102 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 103 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 104 | coihaique | url: api.openweathermap.org/data/2.5/weather?q=coihaique
    Processing Record 105 | mullaitivu | url: api.openweathermap.org/data/2.5/weather?q=mullaitivu
    Processing Record 106 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 107 | menongue | url: api.openweathermap.org/data/2.5/weather?q=menongue
    Processing Record 108 | betioky | url: api.openweathermap.org/data/2.5/weather?q=betioky
    Processing Record 109 | ostrovnoy | url: api.openweathermap.org/data/2.5/weather?q=ostrovnoy
    Processing Record 110 | hurghada | url: api.openweathermap.org/data/2.5/weather?q=hurghada
    Processing Record 111 | epernay | url: api.openweathermap.org/data/2.5/weather?q=epernay
    Processing Record 112 | coquimbo | url: api.openweathermap.org/data/2.5/weather?q=coquimbo
    Processing Record 113 | husavik | url: api.openweathermap.org/data/2.5/weather?q=husavik
    Processing Record 114 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 115 | takoradi | url: api.openweathermap.org/data/2.5/weather?q=takoradi
    Processing Record 116 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 117 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 118 | corum | url: api.openweathermap.org/data/2.5/weather?q=corum
    Processing Record 119 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 120 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 121 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 122 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 123 | amderma | url: api.openweathermap.org/data/2.5/weather?q=amderma
    Processing Record 124 | presidencia roque saenz pena | url: api.openweathermap.org/data/2.5/weather?q=presidencia roque saenz pena
    Processing Record 125 | hambantota | url: api.openweathermap.org/data/2.5/weather?q=hambantota
    Processing Record 126 | east london | url: api.openweathermap.org/data/2.5/weather?q=east london
    Processing Record 127 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 128 | san ramon | url: api.openweathermap.org/data/2.5/weather?q=san ramon
    Processing Record 129 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 130 | sigulda | url: api.openweathermap.org/data/2.5/weather?q=sigulda
    Processing Record 131 | vilhena | url: api.openweathermap.org/data/2.5/weather?q=vilhena
    Processing Record 132 | lebu | url: api.openweathermap.org/data/2.5/weather?q=lebu
    Processing Record 133 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 134 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 135 | tumannyy | url: api.openweathermap.org/data/2.5/weather?q=tumannyy
    Processing Record 136 | thompson | url: api.openweathermap.org/data/2.5/weather?q=thompson
    Processing Record 137 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 138 | colchester | url: api.openweathermap.org/data/2.5/weather?q=colchester
    Processing Record 139 | east london | url: api.openweathermap.org/data/2.5/weather?q=east london
    Processing Record 140 | bandarbeyla | url: api.openweathermap.org/data/2.5/weather?q=bandarbeyla
    Processing Record 141 | santa lucia | url: api.openweathermap.org/data/2.5/weather?q=santa lucia
    Processing Record 142 | lipin bor | url: api.openweathermap.org/data/2.5/weather?q=lipin bor
    Processing Record 143 | arraial do cabo | url: api.openweathermap.org/data/2.5/weather?q=arraial do cabo
    Processing Record 144 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 145 | bilma | url: api.openweathermap.org/data/2.5/weather?q=bilma
    Processing Record 146 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 147 | torbay | url: api.openweathermap.org/data/2.5/weather?q=torbay
    Processing Record 148 | grindavik | url: api.openweathermap.org/data/2.5/weather?q=grindavik
    Processing Record 149 | bakel | url: api.openweathermap.org/data/2.5/weather?q=bakel
    Processing Record 150 | eten | url: api.openweathermap.org/data/2.5/weather?q=eten
    Processing Record 151 | ortigueira | url: api.openweathermap.org/data/2.5/weather?q=ortigueira
    Processing Record 152 | luderitz | url: api.openweathermap.org/data/2.5/weather?q=luderitz
    Processing Record 153 | dingle | url: api.openweathermap.org/data/2.5/weather?q=dingle
    Processing Record 154 | narragansett | url: api.openweathermap.org/data/2.5/weather?q=narragansett
    Processing Record 155 | east london | url: api.openweathermap.org/data/2.5/weather?q=east london
    Processing Record 156 | gilbues | url: api.openweathermap.org/data/2.5/weather?q=gilbues
    Processing Record 157 | sabha | url: api.openweathermap.org/data/2.5/weather?q=sabha
    Processing Record 158 | santa maria | url: api.openweathermap.org/data/2.5/weather?q=santa maria
    Processing Record 159 | saint george | url: api.openweathermap.org/data/2.5/weather?q=saint george
    Processing Record 160 | laguna | url: api.openweathermap.org/data/2.5/weather?q=laguna
    Processing Record 161 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 162 | rutana | url: api.openweathermap.org/data/2.5/weather?q=rutana
    Processing Record 163 | karaul | url: api.openweathermap.org/data/2.5/weather?q=karaul
    Processing Record 164 | salalah | url: api.openweathermap.org/data/2.5/weather?q=salalah
    Processing Record 165 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 166 | grand river south east | url: api.openweathermap.org/data/2.5/weather?q=grand river south east
    Processing Record 167 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 168 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 169 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 170 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 171 | marcona | url: api.openweathermap.org/data/2.5/weather?q=marcona
    Processing Record 172 | bathsheba | url: api.openweathermap.org/data/2.5/weather?q=bathsheba
    Processing Record 173 | marcona | url: api.openweathermap.org/data/2.5/weather?q=marcona
    Processing Record 174 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 175 | sao gabriel da cachoeira | url: api.openweathermap.org/data/2.5/weather?q=sao gabriel da cachoeira
    Processing Record 176 | east london | url: api.openweathermap.org/data/2.5/weather?q=east london
    Processing Record 177 | torbay | url: api.openweathermap.org/data/2.5/weather?q=torbay
    Processing Record 178 | punta arenas | url: api.openweathermap.org/data/2.5/weather?q=punta arenas
    Processing Record 179 | goderich | url: api.openweathermap.org/data/2.5/weather?q=goderich
    Processing Record 180 | lukulu | url: api.openweathermap.org/data/2.5/weather?q=lukulu
    Processing Record 181 | dingle | url: api.openweathermap.org/data/2.5/weather?q=dingle
    Processing Record 182 | barawe | url: api.openweathermap.org/data/2.5/weather?q=barawe
    Processing Record 183 | dharur | url: api.openweathermap.org/data/2.5/weather?q=dharur
    Processing Record 184 | boa vista | url: api.openweathermap.org/data/2.5/weather?q=boa vista
    Processing Record 185 | sorvag | url: api.openweathermap.org/data/2.5/weather?q=sorvag
    Processing Record 186 | punta arenas | url: api.openweathermap.org/data/2.5/weather?q=punta arenas
    Processing Record 187 | carnarvon | url: api.openweathermap.org/data/2.5/weather?q=carnarvon
    Processing Record 188 | klaksvik | url: api.openweathermap.org/data/2.5/weather?q=klaksvik
    Processing Record 189 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 190 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 191 | black river | url: api.openweathermap.org/data/2.5/weather?q=black river
    Processing Record 192 | tasiilaq | url: api.openweathermap.org/data/2.5/weather?q=tasiilaq
    Processing Record 193 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 194 | punta arenas | url: api.openweathermap.org/data/2.5/weather?q=punta arenas
    Processing Record 195 | kolarovo | url: api.openweathermap.org/data/2.5/weather?q=kolarovo
    Processing Record 196 | sur | url: api.openweathermap.org/data/2.5/weather?q=sur
    Processing Record 197 | henties bay | url: api.openweathermap.org/data/2.5/weather?q=henties bay
    Processing Record 198 | springbok | url: api.openweathermap.org/data/2.5/weather?q=springbok
    Processing Record 199 | victoria | url: api.openweathermap.org/data/2.5/weather?q=victoria
    Processing Record 200 | the valley | url: api.openweathermap.org/data/2.5/weather?q=the valley
    Processing Record 201 | bathsheba | url: api.openweathermap.org/data/2.5/weather?q=bathsheba
    Processing Record 202 | ponta do sol | url: api.openweathermap.org/data/2.5/weather?q=ponta do sol
    Processing Record 203 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 204 | bambous virieux | url: api.openweathermap.org/data/2.5/weather?q=bambous virieux
    Processing Record 205 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 206 | hithadhoo | url: api.openweathermap.org/data/2.5/weather?q=hithadhoo
    Processing Record 207 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 208 | kavaratti | url: api.openweathermap.org/data/2.5/weather?q=kavaratti
    Processing Record 209 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 210 | tasiilaq | url: api.openweathermap.org/data/2.5/weather?q=tasiilaq
    Processing Record 211 | hithadhoo | url: api.openweathermap.org/data/2.5/weather?q=hithadhoo
    Processing Record 212 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 213 | bolshiye klyuchishchi | url: api.openweathermap.org/data/2.5/weather?q=bolshiye klyuchishchi
    Processing Record 214 | santa luzia | url: api.openweathermap.org/data/2.5/weather?q=santa luzia
    Processing Record 215 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 216 | saint-joseph | url: api.openweathermap.org/data/2.5/weather?q=saint-joseph
    Processing Record 217 | suruc | url: api.openweathermap.org/data/2.5/weather?q=suruc
    Processing Record 218 | saldanha | url: api.openweathermap.org/data/2.5/weather?q=saldanha
    Processing Record 219 | vila velha | url: api.openweathermap.org/data/2.5/weather?q=vila velha
    Processing Record 220 | marrakesh | url: api.openweathermap.org/data/2.5/weather?q=marrakesh
    Processing Record 221 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 222 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 223 | cururupu | url: api.openweathermap.org/data/2.5/weather?q=cururupu
    Processing Record 224 | bossembele | url: api.openweathermap.org/data/2.5/weather?q=bossembele
    Processing Record 225 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 226 | ribeira grande | url: api.openweathermap.org/data/2.5/weather?q=ribeira grande
    Processing Record 227 | miram shah | url: api.openweathermap.org/data/2.5/weather?q=miram shah
    Processing Record 228 | san jose de guanipa | url: api.openweathermap.org/data/2.5/weather?q=san jose de guanipa
    Processing Record 229 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 230 | jyvaskyla | url: api.openweathermap.org/data/2.5/weather?q=jyvaskyla
    Processing Record 231 | kaz | url: api.openweathermap.org/data/2.5/weather?q=kaz
    Processing Record 232 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 233 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 234 | attawapiskat | url: api.openweathermap.org/data/2.5/weather?q=attawapiskat
    Processing Record 235 | kavaratti | url: api.openweathermap.org/data/2.5/weather?q=kavaratti
    Processing Record 236 | kavaratti | url: api.openweathermap.org/data/2.5/weather?q=kavaratti
    Processing Record 237 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 238 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 239 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 240 | mau | url: api.openweathermap.org/data/2.5/weather?q=mau
    Processing Record 241 | punta arenas | url: api.openweathermap.org/data/2.5/weather?q=punta arenas
    Processing Record 242 | arraial do cabo | url: api.openweathermap.org/data/2.5/weather?q=arraial do cabo
    Processing Record 243 | longyearbyen | url: api.openweathermap.org/data/2.5/weather?q=longyearbyen
    Processing Record 244 | yuzhnyy | url: api.openweathermap.org/data/2.5/weather?q=yuzhnyy
    Processing Record 245 | aswan | url: api.openweathermap.org/data/2.5/weather?q=aswan
    Processing Record 246 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 247 | olafsvik | url: api.openweathermap.org/data/2.5/weather?q=olafsvik
    Processing Record 248 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 249 | vardo | url: api.openweathermap.org/data/2.5/weather?q=vardo
    Processing Record 250 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 251 | lodwar | url: api.openweathermap.org/data/2.5/weather?q=lodwar
    Processing Record 252 | yar-sale | url: api.openweathermap.org/data/2.5/weather?q=yar-sale
    Processing Record 253 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 254 | hithadhoo | url: api.openweathermap.org/data/2.5/weather?q=hithadhoo
    Processing Record 255 | kumba | url: api.openweathermap.org/data/2.5/weather?q=kumba
    Processing Record 256 | calvinia | url: api.openweathermap.org/data/2.5/weather?q=calvinia
    Processing Record 257 | wahran | url: api.openweathermap.org/data/2.5/weather?q=wahran
    Processing Record 258 | at-bashi | url: api.openweathermap.org/data/2.5/weather?q=at-bashi
    Processing Record 259 | kavos | url: api.openweathermap.org/data/2.5/weather?q=kavos
    Processing Record 260 | olafsvik | url: api.openweathermap.org/data/2.5/weather?q=olafsvik
    Processing Record 261 | tasiilaq | url: api.openweathermap.org/data/2.5/weather?q=tasiilaq
    Processing Record 262 | vodnyy | url: api.openweathermap.org/data/2.5/weather?q=vodnyy
    Processing Record 263 | belushya guba | url: api.openweathermap.org/data/2.5/weather?q=belushya guba
    Processing Record 264 | garowe | url: api.openweathermap.org/data/2.5/weather?q=garowe
    Processing Record 265 | clyde river | url: api.openweathermap.org/data/2.5/weather?q=clyde river
    Processing Record 266 | praia da vitoria | url: api.openweathermap.org/data/2.5/weather?q=praia da vitoria
    Processing Record 267 | tobane | url: api.openweathermap.org/data/2.5/weather?q=tobane
    Processing Record 268 | bam | url: api.openweathermap.org/data/2.5/weather?q=bam
    Processing Record 269 | port elizabeth | url: api.openweathermap.org/data/2.5/weather?q=port elizabeth
    Processing Record 270 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 271 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 272 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 273 | iquitos | url: api.openweathermap.org/data/2.5/weather?q=iquitos
    Processing Record 274 | iqaluit | url: api.openweathermap.org/data/2.5/weather?q=iqaluit
    Processing Record 275 | adrar | url: api.openweathermap.org/data/2.5/weather?q=adrar
    Processing Record 276 | kruisfontein | url: api.openweathermap.org/data/2.5/weather?q=kruisfontein
    Processing Record 277 | hambantota | url: api.openweathermap.org/data/2.5/weather?q=hambantota
    Processing Record 278 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 279 | port elizabeth | url: api.openweathermap.org/data/2.5/weather?q=port elizabeth
    Processing Record 280 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 281 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 282 | magugu | url: api.openweathermap.org/data/2.5/weather?q=magugu
    Processing Record 283 | balkanabat | url: api.openweathermap.org/data/2.5/weather?q=balkanabat
    Processing Record 284 | tres arroyos | url: api.openweathermap.org/data/2.5/weather?q=tres arroyos
    Processing Record 285 | upernavik | url: api.openweathermap.org/data/2.5/weather?q=upernavik
    Processing Record 286 | valjala | url: api.openweathermap.org/data/2.5/weather?q=valjala
    Processing Record 287 | oranjestad | url: api.openweathermap.org/data/2.5/weather?q=oranjestad
    Processing Record 288 | manadhoo | url: api.openweathermap.org/data/2.5/weather?q=manadhoo
    Processing Record 289 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 290 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 291 | iqaluit | url: api.openweathermap.org/data/2.5/weather?q=iqaluit
    Processing Record 292 | sinegorskiy | url: api.openweathermap.org/data/2.5/weather?q=sinegorskiy
    Processing Record 293 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 294 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 295 | vardo | url: api.openweathermap.org/data/2.5/weather?q=vardo
    Processing Record 296 | bowmore | url: api.openweathermap.org/data/2.5/weather?q=bowmore
    Processing Record 297 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 298 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 299 | wadi | url: api.openweathermap.org/data/2.5/weather?q=wadi
    Processing Record 300 | sao filipe | url: api.openweathermap.org/data/2.5/weather?q=sao filipe
    Processing Record 301 | puerto narino | url: api.openweathermap.org/data/2.5/weather?q=puerto narino
    Processing Record 302 | pangoa | url: api.openweathermap.org/data/2.5/weather?q=pangoa
    Processing Record 303 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 304 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 305 | yacuiba | url: api.openweathermap.org/data/2.5/weather?q=yacuiba
    Processing Record 306 | bambous virieux | url: api.openweathermap.org/data/2.5/weather?q=bambous virieux
    Processing Record 307 | iralaya | url: api.openweathermap.org/data/2.5/weather?q=iralaya
    Processing Record 308 | mbuji-mayi | url: api.openweathermap.org/data/2.5/weather?q=mbuji-mayi
    Processing Record 309 | afgoye | url: api.openweathermap.org/data/2.5/weather?q=afgoye
    Processing Record 310 | adrar | url: api.openweathermap.org/data/2.5/weather?q=adrar
    Processing Record 311 | talnakh | url: api.openweathermap.org/data/2.5/weather?q=talnakh
    Processing Record 312 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 313 | hobyo | url: api.openweathermap.org/data/2.5/weather?q=hobyo
    Processing Record 314 | berlevag | url: api.openweathermap.org/data/2.5/weather?q=berlevag
    Processing Record 315 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 316 | ponta do sol | url: api.openweathermap.org/data/2.5/weather?q=ponta do sol
    Processing Record 317 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 318 | clyde river | url: api.openweathermap.org/data/2.5/weather?q=clyde river
    Processing Record 319 | sao filipe | url: api.openweathermap.org/data/2.5/weather?q=sao filipe
    Processing Record 320 | jos | url: api.openweathermap.org/data/2.5/weather?q=jos
    Processing Record 321 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 322 | tambacounda | url: api.openweathermap.org/data/2.5/weather?q=tambacounda
    Processing Record 323 | grand river south east | url: api.openweathermap.org/data/2.5/weather?q=grand river south east
    Processing Record 324 | klaksvik | url: api.openweathermap.org/data/2.5/weather?q=klaksvik
    Processing Record 325 | la baneza | url: api.openweathermap.org/data/2.5/weather?q=la baneza
    Processing Record 326 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 327 | kruisfontein | url: api.openweathermap.org/data/2.5/weather?q=kruisfontein
    Processing Record 328 | piney green | url: api.openweathermap.org/data/2.5/weather?q=piney green
    Processing Record 329 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 330 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 331 | karauzyak | url: api.openweathermap.org/data/2.5/weather?q=karauzyak
    Processing Record 332 | barentsburg | url: api.openweathermap.org/data/2.5/weather?q=barentsburg
    Processing Record 333 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 334 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 335 | raudeberg | url: api.openweathermap.org/data/2.5/weather?q=raudeberg
    Processing Record 336 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 337 | belyy yar | url: api.openweathermap.org/data/2.5/weather?q=belyy yar
    Processing Record 338 | torbay | url: api.openweathermap.org/data/2.5/weather?q=torbay
    Processing Record 339 | hithadhoo | url: api.openweathermap.org/data/2.5/weather?q=hithadhoo
    Processing Record 340 | olafsvik | url: api.openweathermap.org/data/2.5/weather?q=olafsvik
    Processing Record 341 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 342 | doha | url: api.openweathermap.org/data/2.5/weather?q=doha
    Processing Record 343 | bambous virieux | url: api.openweathermap.org/data/2.5/weather?q=bambous virieux
    Processing Record 344 | vila franca do campo | url: api.openweathermap.org/data/2.5/weather?q=vila franca do campo
    Processing Record 345 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 346 | manta | url: api.openweathermap.org/data/2.5/weather?q=manta
    Processing Record 347 | louis trichardt | url: api.openweathermap.org/data/2.5/weather?q=louis trichardt
    Processing Record 348 | sorland | url: api.openweathermap.org/data/2.5/weather?q=sorland
    Processing Record 349 | mucurapo | url: api.openweathermap.org/data/2.5/weather?q=mucurapo
    Processing Record 350 | hambantota | url: api.openweathermap.org/data/2.5/weather?q=hambantota
    Processing Record 351 | lilongwe | url: api.openweathermap.org/data/2.5/weather?q=lilongwe
    Processing Record 352 | moyale | url: api.openweathermap.org/data/2.5/weather?q=moyale
    Processing Record 353 | jalu | url: api.openweathermap.org/data/2.5/weather?q=jalu
    Processing Record 354 | caravelas | url: api.openweathermap.org/data/2.5/weather?q=caravelas
    Processing Record 355 | marsa matruh | url: api.openweathermap.org/data/2.5/weather?q=marsa matruh
    Processing Record 356 | kazalinsk | url: api.openweathermap.org/data/2.5/weather?q=kazalinsk
    Processing Record 357 | kigali | url: api.openweathermap.org/data/2.5/weather?q=kigali
    Processing Record 358 | dikson | url: api.openweathermap.org/data/2.5/weather?q=dikson
    Processing Record 359 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 360 | mungaa | url: api.openweathermap.org/data/2.5/weather?q=mungaa
    Processing Record 361 | surgut | url: api.openweathermap.org/data/2.5/weather?q=surgut
    Processing Record 362 | dikson | url: api.openweathermap.org/data/2.5/weather?q=dikson
    Processing Record 363 | bertinoro | url: api.openweathermap.org/data/2.5/weather?q=bertinoro
    Processing Record 364 | tasiilaq | url: api.openweathermap.org/data/2.5/weather?q=tasiilaq
    Processing Record 365 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 366 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 367 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 368 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 369 | rehoboth | url: api.openweathermap.org/data/2.5/weather?q=rehoboth
    Processing Record 370 | dikson | url: api.openweathermap.org/data/2.5/weather?q=dikson
    Processing Record 371 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 372 | monrovia | url: api.openweathermap.org/data/2.5/weather?q=monrovia
    Processing Record 373 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 374 | hambantota | url: api.openweathermap.org/data/2.5/weather?q=hambantota
    Processing Record 375 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 376 | biltine | url: api.openweathermap.org/data/2.5/weather?q=biltine
    Processing Record 377 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 378 | zaozerne | url: api.openweathermap.org/data/2.5/weather?q=zaozerne
    Processing Record 379 | inirida | url: api.openweathermap.org/data/2.5/weather?q=inirida
    Processing Record 380 | beterou | url: api.openweathermap.org/data/2.5/weather?q=beterou
    Processing Record 381 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 382 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 383 | verkhoturye | url: api.openweathermap.org/data/2.5/weather?q=verkhoturye
    Processing Record 384 | ostrovnoy | url: api.openweathermap.org/data/2.5/weather?q=ostrovnoy
    Processing Record 385 | pangnirtung | url: api.openweathermap.org/data/2.5/weather?q=pangnirtung
    Processing Record 386 | torbay | url: api.openweathermap.org/data/2.5/weather?q=torbay
    Processing Record 387 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 388 | victoria | url: api.openweathermap.org/data/2.5/weather?q=victoria
    Processing Record 389 | la sarre | url: api.openweathermap.org/data/2.5/weather?q=la sarre
    Processing Record 390 | sayyan | url: api.openweathermap.org/data/2.5/weather?q=sayyan
    Processing Record 391 | uhlove | url: api.openweathermap.org/data/2.5/weather?q=uhlove
    Processing Record 392 | antofagasta | url: api.openweathermap.org/data/2.5/weather?q=antofagasta
    Processing Record 393 | hofn | url: api.openweathermap.org/data/2.5/weather?q=hofn
    Processing Record 394 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 395 | saldanha | url: api.openweathermap.org/data/2.5/weather?q=saldanha
    Processing Record 396 | kavaratti | url: api.openweathermap.org/data/2.5/weather?q=kavaratti
    Processing Record 397 | tasiilaq | url: api.openweathermap.org/data/2.5/weather?q=tasiilaq
    Processing Record 398 | rawson | url: api.openweathermap.org/data/2.5/weather?q=rawson
    Processing Record 399 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 400 | praia da vitoria | url: api.openweathermap.org/data/2.5/weather?q=praia da vitoria
    Processing Record 401 | sokolo | url: api.openweathermap.org/data/2.5/weather?q=sokolo
    Processing Record 402 | florian | url: api.openweathermap.org/data/2.5/weather?q=florian
    Processing Record 403 | narsaq | url: api.openweathermap.org/data/2.5/weather?q=narsaq
    Processing Record 404 | trelew | url: api.openweathermap.org/data/2.5/weather?q=trelew
    Processing Record 405 | adrar | url: api.openweathermap.org/data/2.5/weather?q=adrar
    Processing Record 406 | nicolas bravo | url: api.openweathermap.org/data/2.5/weather?q=nicolas bravo
    Processing Record 407 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 408 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 409 | dikson | url: api.openweathermap.org/data/2.5/weather?q=dikson
    Processing Record 410 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 411 | escanaba | url: api.openweathermap.org/data/2.5/weather?q=escanaba
    Processing Record 412 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 413 | klaksvik | url: api.openweathermap.org/data/2.5/weather?q=klaksvik
    Processing Record 414 | ler | url: api.openweathermap.org/data/2.5/weather?q=ler
    Processing Record 415 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 416 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 417 | yarim | url: api.openweathermap.org/data/2.5/weather?q=yarim
    Processing Record 418 | victoria | url: api.openweathermap.org/data/2.5/weather?q=victoria
    Processing Record 419 | atar | url: api.openweathermap.org/data/2.5/weather?q=atar
    Processing Record 420 | sao filipe | url: api.openweathermap.org/data/2.5/weather?q=sao filipe
    Processing Record 421 | bambous virieux | url: api.openweathermap.org/data/2.5/weather?q=bambous virieux
    Processing Record 422 | tasiilaq | url: api.openweathermap.org/data/2.5/weather?q=tasiilaq
    Processing Record 423 | upernavik | url: api.openweathermap.org/data/2.5/weather?q=upernavik
    Processing Record 424 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 425 | bargal | url: api.openweathermap.org/data/2.5/weather?q=bargal
    Processing Record 426 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 427 | tete | url: api.openweathermap.org/data/2.5/weather?q=tete
    Processing Record 428 | angermunde | url: api.openweathermap.org/data/2.5/weather?q=angermunde
    Processing Record 429 | paamiut | url: api.openweathermap.org/data/2.5/weather?q=paamiut
    Processing Record 430 | gravatai | url: api.openweathermap.org/data/2.5/weather?q=gravatai
    Processing Record 431 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 432 | albany | url: api.openweathermap.org/data/2.5/weather?q=albany
    Processing Record 433 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 434 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 435 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 436 | azanka | url: api.openweathermap.org/data/2.5/weather?q=azanka
    Processing Record 437 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 438 | havre-saint-pierre | url: api.openweathermap.org/data/2.5/weather?q=havre-saint-pierre
    Processing Record 439 | ponto novo | url: api.openweathermap.org/data/2.5/weather?q=ponto novo
    Processing Record 440 | attawapiskat | url: api.openweathermap.org/data/2.5/weather?q=attawapiskat
    Processing Record 441 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 442 | dingle | url: api.openweathermap.org/data/2.5/weather?q=dingle
    Processing Record 443 | castro | url: api.openweathermap.org/data/2.5/weather?q=castro
    Processing Record 444 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 445 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 446 | mittweida | url: api.openweathermap.org/data/2.5/weather?q=mittweida
    Processing Record 447 | marawi | url: api.openweathermap.org/data/2.5/weather?q=marawi
    Processing Record 448 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 449 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 450 | carauari | url: api.openweathermap.org/data/2.5/weather?q=carauari
    Processing Record 451 | lagoa | url: api.openweathermap.org/data/2.5/weather?q=lagoa
    Processing Record 452 | urdzhar | url: api.openweathermap.org/data/2.5/weather?q=urdzhar
    Processing Record 453 | benghazi | url: api.openweathermap.org/data/2.5/weather?q=benghazi
    Processing Record 454 | dingle | url: api.openweathermap.org/data/2.5/weather?q=dingle
    Processing Record 455 | valdivia | url: api.openweathermap.org/data/2.5/weather?q=valdivia
    Processing Record 456 | puerto leguizamo | url: api.openweathermap.org/data/2.5/weather?q=puerto leguizamo
    Processing Record 457 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 458 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 459 | longyearbyen | url: api.openweathermap.org/data/2.5/weather?q=longyearbyen
    Processing Record 460 | gravdal | url: api.openweathermap.org/data/2.5/weather?q=gravdal
    Processing Record 461 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 462 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 463 | talnakh | url: api.openweathermap.org/data/2.5/weather?q=talnakh
    Processing Record 464 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 465 | vardo | url: api.openweathermap.org/data/2.5/weather?q=vardo
    Processing Record 466 | valparaiso | url: api.openweathermap.org/data/2.5/weather?q=valparaiso
    Processing Record 467 | busselton | url: api.openweathermap.org/data/2.5/weather?q=busselton
    Processing Record 468 | santa cruz | url: api.openweathermap.org/data/2.5/weather?q=santa cruz
    Processing Record 469 | grindavik | url: api.openweathermap.org/data/2.5/weather?q=grindavik
    Processing Record 470 | jiddah | url: api.openweathermap.org/data/2.5/weather?q=jiddah
    Processing Record 471 | smolensk | url: api.openweathermap.org/data/2.5/weather?q=smolensk
    Processing Record 472 | altay | url: api.openweathermap.org/data/2.5/weather?q=altay
    Processing Record 473 | sistranda | url: api.openweathermap.org/data/2.5/weather?q=sistranda
    Processing Record 474 | mizdah | url: api.openweathermap.org/data/2.5/weather?q=mizdah
    Processing Record 475 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 476 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 477 | port elizabeth | url: api.openweathermap.org/data/2.5/weather?q=port elizabeth
    Processing Record 478 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 479 | los llanos de aridane | url: api.openweathermap.org/data/2.5/weather?q=los llanos de aridane
    Processing Record 480 | huarmey | url: api.openweathermap.org/data/2.5/weather?q=huarmey
    Processing Record 481 | jamestown | url: api.openweathermap.org/data/2.5/weather?q=jamestown
    Processing Record 482 | chuy | url: api.openweathermap.org/data/2.5/weather?q=chuy
    Processing Record 483 | kavaratti | url: api.openweathermap.org/data/2.5/weather?q=kavaratti
    Processing Record 484 | mto wa mbu | url: api.openweathermap.org/data/2.5/weather?q=mto wa mbu
    Processing Record 485 | dingle | url: api.openweathermap.org/data/2.5/weather?q=dingle
    Processing Record 486 | kampene | url: api.openweathermap.org/data/2.5/weather?q=kampene
    Processing Record 487 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 488 | qaanaaq | url: api.openweathermap.org/data/2.5/weather?q=qaanaaq
    Processing Record 489 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 490 | touros | url: api.openweathermap.org/data/2.5/weather?q=touros
    Processing Record 491 | iqaluit | url: api.openweathermap.org/data/2.5/weather?q=iqaluit
    Processing Record 492 | tambura | url: api.openweathermap.org/data/2.5/weather?q=tambura
    Processing Record 493 | ballina | url: api.openweathermap.org/data/2.5/weather?q=ballina
    Processing Record 494 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 495 | dingle | url: api.openweathermap.org/data/2.5/weather?q=dingle
    Processing Record 496 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 497 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 498 | jhikargachha | url: api.openweathermap.org/data/2.5/weather?q=jhikargachha
    Processing Record 499 | hermanus | url: api.openweathermap.org/data/2.5/weather?q=hermanus
    Processing Record 500 | souillac | url: api.openweathermap.org/data/2.5/weather?q=souillac
    Processing Record 501 | vestmannaeyjar | url: api.openweathermap.org/data/2.5/weather?q=vestmannaeyjar
    Processing Record 502 | ilulissat | url: api.openweathermap.org/data/2.5/weather?q=ilulissat
    Processing Record 503 | sorland | url: api.openweathermap.org/data/2.5/weather?q=sorland
    Processing Record 504 | grand river south east | url: api.openweathermap.org/data/2.5/weather?q=grand river south east
    Processing Record 505 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 506 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 507 | loandjili | url: api.openweathermap.org/data/2.5/weather?q=loandjili
    Processing Record 508 | los llanos de aridane | url: api.openweathermap.org/data/2.5/weather?q=los llanos de aridane
    Processing Record 509 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 510 | berlevag | url: api.openweathermap.org/data/2.5/weather?q=berlevag
    Processing Record 511 | muisne | url: api.openweathermap.org/data/2.5/weather?q=muisne
    Processing Record 512 | vila franca do campo | url: api.openweathermap.org/data/2.5/weather?q=vila franca do campo
    Processing Record 513 | jutai | url: api.openweathermap.org/data/2.5/weather?q=jutai
    Processing Record 514 | caconda | url: api.openweathermap.org/data/2.5/weather?q=caconda
    Processing Record 515 | attawapiskat | url: api.openweathermap.org/data/2.5/weather?q=attawapiskat
    Processing Record 516 | najran | url: api.openweathermap.org/data/2.5/weather?q=najran
    Processing Record 517 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 518 | husavik | url: api.openweathermap.org/data/2.5/weather?q=husavik
    Processing Record 519 | novoagansk | url: api.openweathermap.org/data/2.5/weather?q=novoagansk
    Processing Record 520 | ponta do sol | url: api.openweathermap.org/data/2.5/weather?q=ponta do sol
    Processing Record 521 | upernavik | url: api.openweathermap.org/data/2.5/weather?q=upernavik
    Processing Record 522 | manaure | url: api.openweathermap.org/data/2.5/weather?q=manaure
    Processing Record 523 | saldanha | url: api.openweathermap.org/data/2.5/weather?q=saldanha
    Processing Record 524 | cape town | url: api.openweathermap.org/data/2.5/weather?q=cape town
    Processing Record 525 | itarema | url: api.openweathermap.org/data/2.5/weather?q=itarema
    Processing Record 526 | saldanha | url: api.openweathermap.org/data/2.5/weather?q=saldanha
    Processing Record 527 | mahebourg | url: api.openweathermap.org/data/2.5/weather?q=mahebourg
    Processing Record 528 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 529 | husavik | url: api.openweathermap.org/data/2.5/weather?q=husavik
    Processing Record 530 | ust-koksa | url: api.openweathermap.org/data/2.5/weather?q=ust-koksa
    Processing Record 531 | yefremov | url: api.openweathermap.org/data/2.5/weather?q=yefremov
    Processing Record 532 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 533 | mar del plata | url: api.openweathermap.org/data/2.5/weather?q=mar del plata
    Processing Record 534 | portsmouth | url: api.openweathermap.org/data/2.5/weather?q=portsmouth
    Processing Record 535 | klaksvik | url: api.openweathermap.org/data/2.5/weather?q=klaksvik
    Processing Record 536 | lagoa | url: api.openweathermap.org/data/2.5/weather?q=lagoa
    Processing Record 537 | lagoa | url: api.openweathermap.org/data/2.5/weather?q=lagoa
    Processing Record 538 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 539 | tsihombe | url: api.openweathermap.org/data/2.5/weather?q=tsihombe
    Processing Record 540 | marcona | url: api.openweathermap.org/data/2.5/weather?q=marcona
    Processing Record 541 | saint george | url: api.openweathermap.org/data/2.5/weather?q=saint george
    Processing Record 542 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 543 | mayor pablo lagerenza | url: api.openweathermap.org/data/2.5/weather?q=mayor pablo lagerenza
    Processing Record 544 | campbellton | url: api.openweathermap.org/data/2.5/weather?q=campbellton
    Processing Record 545 | illoqqortoormiut | url: api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut
    Processing Record 546 | tsihombe | url: api.openweathermap.org/data/2.5/weather?q=tsihombe
    Processing Record 547 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 548 | taolanaro | url: api.openweathermap.org/data/2.5/weather?q=taolanaro
    Processing Record 549 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 550 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 551 | margate | url: api.openweathermap.org/data/2.5/weather?q=margate
    Processing Record 552 | punta arenas | url: api.openweathermap.org/data/2.5/weather?q=punta arenas
    Processing Record 553 | ponta do sol | url: api.openweathermap.org/data/2.5/weather?q=ponta do sol
    Processing Record 554 | santa luzia | url: api.openweathermap.org/data/2.5/weather?q=santa luzia
    Processing Record 555 | dikson | url: api.openweathermap.org/data/2.5/weather?q=dikson
    Processing Record 556 | harlingen | url: api.openweathermap.org/data/2.5/weather?q=harlingen
    Processing Record 557 | belushya guba | url: api.openweathermap.org/data/2.5/weather?q=belushya guba
    Processing Record 558 | piacabucu | url: api.openweathermap.org/data/2.5/weather?q=piacabucu
    Processing Record 559 | maragogi | url: api.openweathermap.org/data/2.5/weather?q=maragogi
    Processing Record 560 | riyadh | url: api.openweathermap.org/data/2.5/weather?q=riyadh
    Processing Record 561 | zelenoborsk | url: api.openweathermap.org/data/2.5/weather?q=zelenoborsk
    Processing Record 562 | saint-joseph | url: api.openweathermap.org/data/2.5/weather?q=saint-joseph
    Processing Record 563 | topsham | url: api.openweathermap.org/data/2.5/weather?q=topsham
    Processing Record 564 | dikson | url: api.openweathermap.org/data/2.5/weather?q=dikson
    Processing Record 565 | tessalit | url: api.openweathermap.org/data/2.5/weather?q=tessalit
    Processing Record 566 | muros | url: api.openweathermap.org/data/2.5/weather?q=muros
    Processing Record 567 | rondonopolis | url: api.openweathermap.org/data/2.5/weather?q=rondonopolis
    Processing Record 568 | malinyi | url: api.openweathermap.org/data/2.5/weather?q=malinyi
    Processing Record 569 | bathsheba | url: api.openweathermap.org/data/2.5/weather?q=bathsheba
    Processing Record 570 | rutigliano | url: api.openweathermap.org/data/2.5/weather?q=rutigliano
    Processing Record 571 | dhidhdhoo | url: api.openweathermap.org/data/2.5/weather?q=dhidhdhoo
    Processing Record 572 | salalah | url: api.openweathermap.org/data/2.5/weather?q=salalah
    Processing Record 573 | barentsburg | url: api.openweathermap.org/data/2.5/weather?q=barentsburg
    Processing Record 574 | mahebourg | url: api.openweathermap.org/data/2.5/weather?q=mahebourg
    Processing Record 575 | wazzan | url: api.openweathermap.org/data/2.5/weather?q=wazzan
    Processing Record 576 | saint-philippe | url: api.openweathermap.org/data/2.5/weather?q=saint-philippe
    Processing Record 577 | bredasdorp | url: api.openweathermap.org/data/2.5/weather?q=bredasdorp
    Processing Record 578 | mahebourg | url: api.openweathermap.org/data/2.5/weather?q=mahebourg
    Processing Record 579 | wladyslawowo | url: api.openweathermap.org/data/2.5/weather?q=wladyslawowo
    Processing Record 580 | barentsburg | url: api.openweathermap.org/data/2.5/weather?q=barentsburg
    Processing Record 581 | victoria | url: api.openweathermap.org/data/2.5/weather?q=victoria
    Processing Record 582 | jega | url: api.openweathermap.org/data/2.5/weather?q=jega
    Processing Record 583 | orsha | url: api.openweathermap.org/data/2.5/weather?q=orsha
    Processing Record 584 | souillac | url: api.openweathermap.org/data/2.5/weather?q=souillac
    Processing Record 585 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 586 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 587 | saint-pierre | url: api.openweathermap.org/data/2.5/weather?q=saint-pierre
    Processing Record 588 | palafrugell | url: api.openweathermap.org/data/2.5/weather?q=palafrugell
    Processing Record 589 | georgetown | url: api.openweathermap.org/data/2.5/weather?q=georgetown
    Processing Record 590 | chipinge | url: api.openweathermap.org/data/2.5/weather?q=chipinge
    Processing Record 591 | port alfred | url: api.openweathermap.org/data/2.5/weather?q=port alfred
    Processing Record 592 | sao joao da barra | url: api.openweathermap.org/data/2.5/weather?q=sao joao da barra
    Processing Record 593 | rengo | url: api.openweathermap.org/data/2.5/weather?q=rengo
    Processing Record 594 | atar | url: api.openweathermap.org/data/2.5/weather?q=atar
    Processing Record 595 | porto novo | url: api.openweathermap.org/data/2.5/weather?q=porto novo
    Processing Record 596 | carbonia | url: api.openweathermap.org/data/2.5/weather?q=carbonia
    Processing Record 597 | attawapiskat | url: api.openweathermap.org/data/2.5/weather?q=attawapiskat
    Processing Record 598 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    Processing Record 599 | nushki | url: api.openweathermap.org/data/2.5/weather?q=nushki
    Processing Record 600 | ushuaia | url: api.openweathermap.org/data/2.5/weather?q=ushuaia
    


```python
# Create lists of latitudes and temperatures
lat_data = []
lon_data =[]
temp_data = []
humidity_data = []
wind_data = []
cloudiness_data = []
city_data = []
country_data = []
date_data = []

# I learned that not all the cities identified by the citipy modules is identifiable by OpenWeather.
# In which case, the results from OpenWeather contains a key that called 'message' with the value = 'city not found'.
# 'message' is a key that doesn't appear in the dictionary for a city that is identified by OpenWeather.
# Hence, in the for loop below, I inserted an if statement to append data only when the 'message' is not in the city's dictionary keys.

for data in weather_data:
    if 'message' not in data.keys(): 
        date_data.append(data['dt'])
        city_data.append(data['name'])
        country_data.append(data['sys']['country'])
        lat_data.append(data['coord']['lat'])
        lon_data.append(data['coord']['lon'])
        temp_data.append(data['main']['temp'])
        humidity_data.append(data['main']['humidity'])
        wind_data.append(data['wind']['speed'])
        cloudiness_data.append(data['clouds']['all'])

# Shortcut:
# lat_data = [data['coord']['lat'] for data in weather_data]
# temp_data = [data['main']['temp'] for data in weather_data]

weather_dict = {'Date': date_data,
                'City': city_data,
                'Country': country_data,
                'Latitude': lat_data,
                'Longitude': lon_data,
                'Cloudiness': cloudiness_data,
                'Max Temp': temp_data,                 
                'Humidity': humidity_data,
                'Wind Speed': wind_data,
                }
weather_data_df = pd.DataFrame(weather_dict)
weather_data_df = weather_data_df[['Date','City','Country','Latitude','Longitude','Cloudiness',
                                   'Max Temp', 'Humidity', 'Wind Speed']]
```


```python
weather_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Cloudiness</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1512363600</td>
      <td>Mahebourg</td>
      <td>MU</td>
      <td>-20.41</td>
      <td>57.70</td>
      <td>40</td>
      <td>29.00</td>
      <td>62</td>
      <td>7.20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1512367236</td>
      <td>Ushuaia</td>
      <td>AR</td>
      <td>-54.80</td>
      <td>-68.30</td>
      <td>76</td>
      <td>8.46</td>
      <td>85</td>
      <td>5.90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1512366960</td>
      <td>Mastic Beach</td>
      <td>US</td>
      <td>40.77</td>
      <td>-72.85</td>
      <td>1</td>
      <td>2.60</td>
      <td>100</td>
      <td>2.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1512363600</td>
      <td>Torbay</td>
      <td>CA</td>
      <td>47.67</td>
      <td>-52.73</td>
      <td>90</td>
      <td>0.00</td>
      <td>100</td>
      <td>5.70</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1512367335</td>
      <td>Feijo</td>
      <td>BR</td>
      <td>-8.16</td>
      <td>-70.35</td>
      <td>12</td>
      <td>25.11</td>
      <td>92</td>
      <td>1.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(len(weather_data_df))
```

    511
    


```python
#Of the 600 cities identified by citipy, only 511 were identified by OpenWeather
```


```python
#For ease of interpretability, convert the Date into a recognizable format
import time
formatted_date = []

for date in weather_data_df['Date']:
    formatted_date.append(time.ctime(date))   
weather_data_df['Date'] = formatted_date
weather_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Cloudiness</th>
      <th>Max Temp</th>
      <th>Humidity</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sun Dec  3 21:00:00 2017</td>
      <td>Mahebourg</td>
      <td>MU</td>
      <td>-20.41</td>
      <td>57.70</td>
      <td>40</td>
      <td>29.00</td>
      <td>62</td>
      <td>7.20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sun Dec  3 22:00:36 2017</td>
      <td>Ushuaia</td>
      <td>AR</td>
      <td>-54.80</td>
      <td>-68.30</td>
      <td>76</td>
      <td>8.46</td>
      <td>85</td>
      <td>5.90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sun Dec  3 21:56:00 2017</td>
      <td>Mastic Beach</td>
      <td>US</td>
      <td>40.77</td>
      <td>-72.85</td>
      <td>1</td>
      <td>2.60</td>
      <td>100</td>
      <td>2.57</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sun Dec  3 21:00:00 2017</td>
      <td>Torbay</td>
      <td>CA</td>
      <td>47.67</td>
      <td>-52.73</td>
      <td>90</td>
      <td>0.00</td>
      <td>100</td>
      <td>5.70</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sun Dec  3 22:02:15 2017</td>
      <td>Feijo</td>
      <td>BR</td>
      <td>-8.16</td>
      <td>-70.35</td>
      <td>12</td>
      <td>25.11</td>
      <td>92</td>
      <td>1.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Save a CSV of all data retrieved
weather_data_df.to_csv('Weather_Data_DF3.csv',sep=',')
```


```python
# Scatterplot: Temperature v. Latitude
plt.scatter(weather_data_df["Latitude"], weather_data_df["Max Temp"], marker="o")
plt.title("Max Temp (Celsius) v. City Latitude" + '\n' + time.strftime("%d/%m/%Y"))
plt.ylabel("Max Temp (Celsius)")
plt.xlabel("City Latitude")
plt.grid(True)
plt.savefig('Temp v City Latitude.png') #I discovered that if I put plt.show before the plt.savefig, the image will be blank.
plt.show()
```


![png](output_13_0.png)



```python
#For fun, I used seaborn to plot the distribution of each x and y variable.
sns.jointplot(x="Latitude", y="Max Temp", data=weather_data_df)
plt.savefig('Temp v City Latitude_sns.png')
plt.show()
```


![png](output_14_0.png)



```python
# Scatterplot: Humidity(%) v. Latitude
plt.scatter(weather_data_df["Latitude"], weather_data_df["Humidity"], marker="o")
plt.title("Humidity (%) v. City Latitude" + '\n' + time.strftime("%d/%m/%Y"))
plt.ylabel("Humidity (%)")
plt.xlabel("City Latitude")
plt.grid(True)
plt.savefig('Humidity v City Latitude.png')
plt.show()
```


![png](output_15_0.png)



```python
#For fun, I used seaborn to plot a regression line
f, ax = plt.subplots(figsize=(6, 4))
sns.regplot(x="Latitude", y="Humidity", data=weather_data_df, ax=ax);
plt.savefig('Humidity v City Latitude_sns.png')
plt.show()
```


![png](output_16_0.png)



```python
# Scatterplot: Cloudiness(%) v. Latitude
plt.scatter(weather_data_df["Latitude"], weather_data_df["Cloudiness"], marker="o")
plt.title("Cloudiness (%) v. Latitude" + '\n' + time.strftime("%d/%m/%Y"))
plt.ylabel("Cloudiness (%)")
plt.xlabel("City Latitude")
plt.grid(True)
plt.show()
plt.savefig('Cloudiness v City Latitude.png')
```


![png](output_17_0.png)



```python
# Scatterplot: Wind Speed(mph) v. Latitude
plt.scatter(weather_data_df["Latitude"], weather_data_df["Wind Speed"], marker="o")
plt.title("Wind Speed (mph) v. City Latitude" + '\n' + time.strftime("%d/%m/%Y"))
plt.ylabel("Wind Speed (mph))")
plt.xlabel("City Latitude")
plt.grid(True)
plt.savefig('Wind Speed v City Latitude.png')
plt.show()
```


![png](output_18_0.png)



```python
sns.jointplot(x="Latitude", y="Wind Speed", data=weather_data_df)
plt.savefig('Wind Speed v City Latitude_sns.png')
plt.show()
```


    <matplotlib.figure.Figure at 0x87def98>



![png](output_19_1.png)



```python
# In looking at the full list of 517 cities in the csv, there are a lot of duplicate ciites.
# I think this is becuase I generated 600 random pair of whole nubmers with a range between -90 and 90.
# I think this would be mitigated if I generated lat lon with at least 4 significant figures so there's more variation
# in the nearest_city generated by citipy.

#Observation #1: The temperature trend seems to reverse near the equator (latitude = 0).
#As the latitude approaches the equator from the north or south, temperature increases.
#I believe that if we were run this analysis six months from now, the graph would be reversed 
#since I know that the northern (positive latitude) and southern (negative latitude) hemispheres have opposite seasons.

#Observation #2:  There doesn't seem to be clear linear relationship between latitude and humidity.  
#We are able to tell from the scatterplot that there are are way more cities with humidity >80 than cities <80.

#Observation #3: There doesn't seem to be a clearn linear relationship between latitude and wind speed or cloudiness.
#Although based on the distribution of dots, there seems to be an equal spread of cloudiness across all cities
#and an unequal spread of windspeed across cities.  Most cities have windspeed <4 mph. This can also be seen by the 
#distribution plot of the y variable.
```
