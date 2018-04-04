

```python
import matplotlib.pyplot as plt
import seaborn as sb
import json
import pandas as pd
from pprint import pprint
from jupyterthemes import jtplot
import requests
from config import api_key
import random
jtplot.style(theme='onedork')
```


```python
#I downloaded the conplete Open Weather Cities list from http://bulk.openweathermap.org/sample/ and put it into my resources folder
citilistpath = "Resources/city.list.json"
#Create a dataframe with the data for all cities
citilistdf = pd.read_json(citilistpath)
citilistdf.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>coord</th>
      <th>country</th>
      <th>id</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>{'lon': 34.283333, 'lat': 44.549999}</td>
      <td>UA</td>
      <td>707860</td>
      <td>Hurzuf</td>
    </tr>
    <tr>
      <th>1</th>
      <td>{'lon': 37.666668, 'lat': 55.683334}</td>
      <td>RU</td>
      <td>519188</td>
      <td>Novinki</td>
    </tr>
    <tr>
      <th>2</th>
      <td>{'lon': 84.633331, 'lat': 28}</td>
      <td>NP</td>
      <td>1283378</td>
      <td>GorkhÄ�</td>
    </tr>
    <tr>
      <th>3</th>
      <td>{'lon': 76, 'lat': 29}</td>
      <td>IN</td>
      <td>1270260</td>
      <td>State of HaryÄ�na</td>
    </tr>
    <tr>
      <th>4</th>
      <td>{'lon': 33.900002, 'lat': 44.599998}</td>
      <td>UA</td>
      <td>708546</td>
      <td>Holubynka</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Method to randomly sample 500 city ids from the list of all city ids available on openweather (random.sample does not allow duplicates)
#Take the list of all IDS and select 500
#Print the length to confirm we sampled exactly 500
cityidlist = list(citilistdf['id'])
cityids = random.sample(cityidlist, 500)
print(len(cityids))
```

    500
    


```python
#Construct URL and all lists to store retrieved data
url = "http://api.openweathermap.org/data/2.5/weather?units=imperial"
namelist = []
cityidlist = []
latitudelist = []
temperaturelist = []
humiditylist = []
cloudinesslist = []
windspeedlist = []
countrylist = []
#Iterate through all cityids
for cityid in cityids:
    #Construct query url using base url, api key, and the current city id
    query_url = url + "&appid=" + api_key + "&id=" + str(cityid)
    #convert to json
    weather_response = requests.get(query_url)
    weather_json = weather_response.json()
    name = weather_json['name']
    #find and append proper piece of data from the weather data to the list
    namelist.append(weather_json['name'])
    cityidlist.append(weather_json['id'])
    latitudelist.append(weather_json['coord']['lat'])
    temperaturelist.append(weather_json['main']['temp'])
    humiditylist.append(weather_json['main']['humidity'])
    cloudinesslist.append(weather_json['clouds']['all'])
    windspeedlist.append(weather_json['wind']['speed'])
    countrylist.append(weather_json['sys']['country'])
    #print City Name, Country Code, City ID and URL without API Key
    print(name+","+weather_json['sys']['country']+","+str(cityid))
    print(url+"&id="+str(cityid))
```

    Attnang-Puchheim,AT,3233832
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3233832
    Sabanagrande,HN,3602680
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3602680
    Albánchez,ES,6355521
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6355521
    Us,FR,2971316
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2971316
    Reghin,RO,668997
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=668997
    Leon Valley,US,4705923
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4705923
    Nesselwangle,AT,2770879
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2770879
    Ludchurch,GB,2643438
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2643438
    Argostolion,GR,264668
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=264668
    Cajahs Mountain,US,4458580
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4458580
    Brazii,RO,683770
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=683770
    Ezanville,FR,3019175
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3019175
    Indor,SE,2703756
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2703756
    Gieselwerder,DE,2920560
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2920560
    Apetlon,AT,2782454
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2782454
    Carrickroe,IE,3310670
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3310670
    Konak,RS,789346
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=789346
    Vila Real de Santo António,PT,8010527
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=8010527
    Le Chesnay,FR,6455402
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6455402
    Giebultow,PL,3099388
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3099388
    Antanetikely,MG,1070738
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1070738
    Alhendin,ES,2521984
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2521984
    Limours,FR,2998269
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2998269
    Westborough,US,4955149
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4955149
    Quarter of Vieux-Fort,LC,3576413
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3576413
    Mount Horeb,US,5263667
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5263667
    Yorosso,ML,2448442
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2448442
    Caldwell,US,4678111
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4678111
    Schwaney,DE,2835258
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2835258
    Tolk,DE,2821956
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2821956
    Bighorn,US,5413922
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5413922
    Frucht,DE,2924125
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2924125
    Tabaco,PH,1689433
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1689433
    Panggungasri,ID,7375255
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7375255
    Naray,AF,1132336
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1132336
    Cerrina,IT,6534395
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6534395
    Faramans,FR,3019055
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3019055
    Duncans Creek,AU,2168159
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2168159
    Fantanele,RO,678152
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=678152
    Kornos,CY,146438
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=146438
    Corato,IT,3178131
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3178131
    Crondall,GB,2651920
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2651920
    Saint-Pierre-d'Aurillac,FR,6432439
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6432439
    Grossweitzschen,DE,2914872
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2914872
    Peujeuh,ID,7921409
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7921409
    Almstedt,DE,6552471
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6552471
    Guba,PH,1712609
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1712609
    Elsau,CH,7285722
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7285722
    Lestrobe,ES,6544457
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6544457
    Jelenia Gora,PL,3097257
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3097257
    Llangunnor,GB,7301148
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7301148
    Sorbolo,IT,3166374
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3166374
    San Sebastian de los Reyes,ES,3110040
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3110040
    Zorbau,DE,2803892
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2803892
    Villarboit,IT,3164159
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3164159
    Sioux County,US,4876545
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4876545
    Gonzales,US,4693940
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4693940
    Ottana,IT,3171782
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3171782
    Cadott,US,5247387
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5247387
    Jonava,LT,598818
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=598818
    Eastland,US,4688071
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4688071
    Bethany,CA,5899156
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5899156
    Ellendale,US,5059052
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5059052
    San Miguel de Salinas,ES,2511277
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2511277
    Vogelsang,DE,2816913
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2816913
    Woods Reef,AU,2143056
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2143056
    Schlindermanderscheid,LU,2960094
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2960094
    Kemeten,AT,7872740
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7872740
    Mala Tokmachka,UA,702153
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=702153
    Aurith,DE,2953996
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2953996
    Heuningspruit,ZA,996683
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=996683
    Platikambos,GR,254990
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=254990
    Montemarano,IT,3172847
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3172847
    Dalaguete,PH,1716108
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1716108
    Jiaotang,CN,1806017
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1806017
    Esches,FR,3019810
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3019810
    Muñoveros,ES,6360864
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6360864
    Klofta,NO,3149813
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3149813
    Wetzikon / Kempten,CH,6292591
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6292591
    Dütschow,DE,2934195
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2934195
    Baizhang,CN,1817205
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1817205
    Nurallao,IT,2523988
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2523988
    El Aguacate,DO,3508639
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3508639
    Cabulay,PH,1721370
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1721370
    Ngiyono,ID,8067846
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=8067846
    Ipil,PH,1710609
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1710609
    Taocun,CN,1793165
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1793165
    Dagebull,DE,2939489
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2939489
    Pratápolis,BR,3452380
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3452380
    Greimerath,DE,6554325
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6554325
    Vecoux,FR,2970256
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2970256
    Aichstetten,DE,6556053
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6556053
    Boltaña,ES,6358311
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6358311
    Schonenbuch,CH,2658708
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2658708
    Cartyville,CA,5917819
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5917819
    Sao Joao Batista,BR,3448906
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3448906
    Lomaso,IT,6534576
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6534576
    Ponitz,DE,2852729
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2852729
    Dharsin,NP,7934341
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7934341
    Niederheimbach,DE,6555288
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6555288
    Sadar,ID,7579649
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7579649
    Carmen de Areco,AR,3435644
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3435644
    Pignon,HT,3718834
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3718834
    Süri,CH,7670424
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7670424
    Saint-Étienne-la-Thillaye,FR,6427430
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6427430
    San Juan Tecuaco,GT,3589880
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3589880
    Lyndonville,US,5238203
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5238203
    Bias,FR,3032791
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3032791
    Les Montets,CH,7286331
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7286331
    Irsch,DE,2895755
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2895755
    Junguitu,ES,3120071
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3120071
    Woppenrieth,DE,2806207
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2806207
    Calvizzano,IT,3181140
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3181140
    Helm,US,4390078
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4390078
    Dayr al Asal al Fawqa,PS,283120
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=283120
    Hertingshausen,DE,2905548
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2905548
    Barra da Estiva,BR,3470744
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3470744
    Palma Soriano,CU,3545064
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3545064
    Piherarh Municipality,FM,7626977
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7626977
    Coleman,CA,5925154
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5925154
    Oberzelg,CH,2659318
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2659318
    Vilar do Pinheiro,PT,2732442
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2732442
    Siemianowice Śląskie,PL,7531569
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7531569
    Riverview,AU,2151513
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2151513
    Montes Claros,BR,3456814
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3456814
    Okha,RU,2122614
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2122614
    Los Angeles,PH,1705545
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1705545
    Langhem,SE,2697455
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2697455
    Niederbüren,CH,7286636
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7286636
    Santa Bárbara de Casa,ES,6358244
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6358244
    Häder,DE,2912807
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2912807
    Huta Stara B,PL,3097935
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3097935
    Maasmechelen,BE,2791961
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2791961
    Rio de Mouro,PT,2263827
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2263827
    Steenbecque,FR,6438507
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6438507
    Xialaba,CN,1790672
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1790672
    El Vedero,VE,3755700
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3755700
    Livadákion,GR,735395
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=735395
    Pancorbo,ES,6356523
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6356523
    San Augustine,US,4726256
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4726256
    Pontestura,IT,3170215
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3170215
    Canton de Mersch,LU,2960275
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2960275
    Sidamon,ES,3109012
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3109012
    Suicheng,CN,1793899
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1793899
    Neudau,AT,2770838
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2770838
    Gneven,DE,6548350
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6548350
    Virey,FR,6435710
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6435710
    Fradelos,PT,8011309
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=8011309
    Fallston,US,4354708
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4354708
    Atteln,DE,2954952
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2954952
    Wapielsk,PL,3082647
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3082647
    Celso Ramos,BR,3466481
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3466481
    Tauscha,DE,2823789
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2823789
    Wolbrom,PL,3081495
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3081495
    Worgl,AT,2760854
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2760854
    Parròquia de Canillo,AD,3041203
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3041203
    Pitogo,PH,1692788
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1692788
    Cobadin,RO,681257
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=681257
    Ilow,PL,770798
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=770798
    Pollachi,IN,1259440
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1259440
    Schwanewede,DE,2835260
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2835260
    Toledo,US,5174035
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5174035
    Qinglong,CN,1553833
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1553833
    Idar,DE,2896757
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2896757
    Berceo,ES,3128229
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3128229
    Saint-Briac-sur-Mer,FR,2981294
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2981294
    Bygrave,GB,2654134
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2654134
    Gasen,AT,2778832
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2778832
    Woodbridge,CA,6184009
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6184009
    Holdenville,US,4538869
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4538869
    La Joya,US,4703997
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4703997
    Pekutatan,ID,7553299
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7553299
    Clovis,US,5338122
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5338122
    San-Martino-di-Lota,FR,2976186
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2976186
    Heric,FR,3013457
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3013457
    Klokočov,CZ,3073574
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3073574
    Qualeup,AU,2062823
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2062823
    Villodrigo,ES,3104657
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3104657
    Bunesti,RO,683279
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=683279
    Roye,FR,6442093
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6442093
    Umunede,NG,2320417
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2320417
    Huagai,CN,1807723
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1807723
    Berstett,FR,3033219
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3033219
    Thionville,FR,6454375
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6454375
    East Dereham,GB,2650470
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2650470
    Politischer Bezirk Schwaz,AT,2765389
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2765389
    Ulica Starowiejska,PL,756349
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=756349
    Oud-Loosdrecht,NL,2748926
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2748926
    Wudian,CN,1791209
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1791209
    San Pietro in Amantea,IT,2523387
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2523387
    Poduri,RO,670274
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=670274
    Saint-Ay,FR,6434701
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6434701
    Forest Oak,US,4324944
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4324944
    Saint-Étienne-du-Rouvray,FR,6443443
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6443443
    Artaiz,ES,3129345
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3129345
    Hukvaldy,CZ,3074733
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3074733
    San Potito Sannitico,IT,3167796
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3167796
    Purna,IN,1259177
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1259177
    Vaudeloges,FR,2970533
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2970533
    San Nicolas de los Arroyos,AR,3836846
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3836846
    Castano Primo,IT,6536502
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6536502
    Bialet Massé,AR,3864261
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3864261
    Silberstedt,DE,6551995
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6551995
    Zhentou,CN,1924521
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1924521
    Michurinsk,RU,527191
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=527191
    Région de Gao,ML,2457161
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2457161
    Llombai,ES,2514994
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2514994
    Kimovsk,RU,548658
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=548658
    Tavernspite,GB,2636175
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2636175
    Weenzen,DE,6552496
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6552496
    Raheenagh,IE,3291406
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3291406
    Hemmersheim,DE,6556907
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6556907
    Landesbergen,DE,6558436
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6558436
    Yuxin,CN,6682545
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6682545
    Leeton,AU,7839744
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7839744
    Truskolasy,PL,3083201
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3083201
    Devant,CH,2661029
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2661029
    Rietfontein,ZA,961935
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=961935
    Ocean Grove,US,5102136
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5102136
    Ponferrada,ES,3113236
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3113236
    Livani,LV,457860
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=457860
    Trescasas,ES,3107509
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3107509
    San Martin,AR,3836992
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3836992
    Sangdang,CN,7384060
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7384060
    Pont-Aven,FR,6431022
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6431022
    Harsova,RO,676163
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=676163
    Unterstrogn,DE,2818590
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2818590
    Pueai Noi,TH,7510962
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7510962
    Zhaqsy,KZ,1517020
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1517020
    Tiverton,CA,6167033
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6167033
    Kiskore,HU,718911
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=718911
    Borgloon,BE,2801467
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2801467
    Mojotoro,BO,3910203
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3910203
    Caimancito,AR,3863491
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3863491
    Niederscheyern,DE,2862903
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2862903
    Kasukabe,JP,1859884
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1859884
    Blume,DE,2947573
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2947573
    Synyanyrd,RU,485203
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=485203
    Pekmezli,TR,439101
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=439101
    Huntlosen,DE,2897287
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2897287
    Rheinberg,DE,2847662
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2847662
    Kumo,NG,2333451
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2333451
    Sankt Anna am Aigen,AT,7872310
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7872310
    Modot,MN,2029989
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2029989
    Amial,PT,2742935
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2742935
    Mussidan,FR,6453829
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6453829
    Carenas,ES,3126286
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3126286
    Nooksack,US,5804860
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5804860
    Zhongan,CN,1784477
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1784477
    Al Artawiyah,SA,110031
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=110031
    Gemeente Druten,NL,2756538
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2756538
    Schechingen,DE,2840116
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2840116
    Oberboihingen,DE,6555430
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6555430
    Shearwater,CA,6145615
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6145615
    Malberg,DE,2874142
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2874142
    Ebenthal,AT,2780544
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2780544
    Loyalhanna,US,5199209
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5199209
    Hannersdorf,AT,2776891
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2776891
    Haoli,CN,7326394
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7326394
    Bolszewo,PL,3102961
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3102961
    Villers-la-Montagne,FR,2968382
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2968382
    Finkenwerder,DE,2926732
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2926732
    Ross,US,5691399
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5691399
    Magadanskaya Oblast’,RU,2123627
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2123627
    Bānskhoh,IN,1277225
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1277225
    Waikato,NZ,2180293
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2180293
    Gorcop,PK,1178148
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1178148
    Laguna Seca,HN,3607802
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3607802
    Chavenon,FR,6446548
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6446548
    Ganwang,CN,2037306
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2037306
    Rauschwitz,DE,2849857
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2849857
    Saint-Gengoux-le-National,FR,2980006
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2980006
    Bamfield,CA,5892444
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5892444
    Mount Keira,AU,2206111
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2206111
    Cronenberg,DE,6555082
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6555082
    Jaynagar,IN,1269093
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1269093
    Hujra,PK,1176800
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1176800
    Charnay,FR,3026538
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3026538
    Solana,PH,1685880
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1685880
    Zlonice,CZ,3061360
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3061360
    Caetanópolis,BR,3468329
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3468329
    Gars,DE,2922502
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2922502
    Loteamento Jardim Ana Maria,BR,7619472
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7619472
    Polovinnoye,RU,1494525
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1494525
    Chum Phae,TH,1611026
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1611026
    Roberts County,US,5230872
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5230872
    Canton de Redange,LU,2960161
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2960161
    Yuzhnyy,RU,841412
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=841412
    Erlong,CN,2037470
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2037470
    Akko,NG,2351027
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2351027
    Issoire,FR,3012664
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3012664
    Anchau,NG,2349951
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2349951
    Rising Sun,US,4366852
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4366852
    Streifen,DE,2826003
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2826003
    Youhua,CN,1552324
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1552324
    Cabezabellosa de la Calzada,ES,6360316
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6360316
    Saint-Pé-de-Léren,FR,6440602
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6440602
    Permatang Damar Laut,MY,1735071
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1735071
    Huraydah,YE,79393
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=79393
    DeLand,US,4152890
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4152890
    Marineo,IT,2524255
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2524255
    Mitu,CO,3674676
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3674676
    Purchena,ES,2512133
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2512133
    Iturmendi,ES,3120248
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3120248
    Badbergen,DE,6552978
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6552978
    Tha Sala,TH,1150200
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1150200
    Giershausen,DE,2920589
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2920589
    Delphos,US,5151941
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5151941
    Wanaque,US,5106014
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5106014
    Waake,DE,6552350
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6552350
    Dickson City,US,5186924
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5186924
    Gogino,RU,6610226
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6610226
    Levesen,DE,2878224
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2878224
    Kamienica,PL,769697
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=769697
    Bolesławiec,PL,7532649
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7532649
    Drocourt,FR,3020792
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3020792
    Garden City,US,4196688
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4196688
    Hesseneck,DE,3274969
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3274969
    Salching,DE,2842315
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2842315
    Worth an der Donau,DE,2806080
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2806080
    Hinigaran,PH,1711621
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1711621
    Aksu,TR,752359
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=752359
    Shermans Corner,US,4978496
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4978496
    Roskoshnoye,UA,695441
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=695441
    Roperuelos del Páramo,ES,6358674
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6358674
    Penha,BR,3454213
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3454213
    Saint-Martin-le-Gréard,FR,6435622
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6435622
    Capestang,FR,3028720
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3028720
    Qasr al Farafirah,EG,350661
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=350661
    Bochum-Werne,DE,2810834
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2810834
    Schleiden,DE,2838744
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2838744
    Arganil,PT,8010481
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=8010481
    Oostelbeers,NL,2749513
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2749513
    Les,ES,6362989
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6362989
    South Lebanon,US,4525084
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4525084
    Kirchdorf am Inn,DE,2890602
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2890602
    Selogiri,ID,1627969
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1627969
    Neukirchen,DE,2864759
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2864759
    Nay,FR,2990851
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2990851
    Schleitheim,CH,7287092
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7287092
    Stützengrün,DE,6548593
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6548593
    Céligny,CH,2661240
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2661240
    Haverhill,US,4158239
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4158239
    Kaaawa,US,5856759
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5856759
    Habach,DE,2913069
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2913069
    Parbayse,FR,2988587
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2988587
    Barich,CA,5893004
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5893004
    Schirnding,DE,2839066
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2839066
    Niedersohren,DE,2862862
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2862862
    Al Tuhayta,YE,77655
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=77655
    Zeleny Hay,UA,687345
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=687345
    Streaky Bay,AU,7839460
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7839460
    Eidfjord,NO,3158668
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3158668
    Kelburn,NZ,2188922
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2188922
    Carbon County,US,5536454
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5536454
    Pouancé,FR,6435268
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6435268
    Anavra,GR,265095
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=265095
    Kosi,IN,1266073
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1266073
    Tumut Shire,AU,7839368
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7839368
    Schönenbuch,CH,7287108
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7287108
    Grenada,GD,3580239
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3580239
    Ainsworth,US,5062769
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5062769
    Bankstown,AU,7839688
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7839688
    Macedo de Cavaleiros,PT,8010464
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=8010464
    Malpica,ES,2514219
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2514219
    City of Parramatta,AU,7302259
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7302259
    Xinhe,CN,7643671
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7643671
    Taourirt,MA,2530048
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2530048
    Chevry,FR,3025274
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3025274
    Loscorrales,ES,3118025
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3118025
    Kirchlinteln,DE,6552843
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6552843
    Malabon,PH,1703320
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1703320
    Oleiros,ES,3115177
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3115177
    São José da Bela Vista,BR,3448696
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3448696
    Chichester,GB,2653192
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2653192
    South Valley Stream,US,5139015
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5139015
    Lark Harbour,CA,6049990
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6049990
    Reckendorf,DE,2849674
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2849674
    Chuvashskaya Respublika,RU,567395
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=567395
    Bischofsheim,DE,2948289
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2948289
    Peralta,DO,3495020
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3495020
    Астафьево,RU,817459
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=817459
    Zigos,GR,733749
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=733749
    Gwennap,GB,7294624
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7294624
    Naisud,PH,1698532
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1698532
    Liencres,ES,3118447
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3118447
    Pine Island Center,US,4168493
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4168493
    Wuhan,CN,1791247
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1791247
    Mykolayivka,UA,700180
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=700180
    Sennecey-le-Grand,FR,2975080
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2975080
    Polzeath,GB,6324514
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6324514
    Pringgodani,ID,7350812
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7350812
    Spiegelberg,DE,2830561
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2830561
    Port Royal,JM,3488980
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3488980
    Hlebine,HR,3199376
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3199376
    Eiras,ES,3123703
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3123703
    Mücka,DE,6548644
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6548644
    Wijnegem,BE,2783685
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2783685
    Tunau,DE,6555896
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6555896
    Licq-Athérey,FR,6440474
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6440474
    Berg,CH,6290903
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6290903
    Hodgleigh,AU,2163334
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2163334
    Saint-Romain-le-Puy,FR,6434316
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6434316
    Shelford,AU,2149680
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2149680
    Big Pine,US,5328426
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5328426
    Pinerolo,IT,6540662
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6540662
    Sauvagnat-Sainte-Marthe,FR,6440235
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6440235
    Friedrichsfehn,DE,2924618
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2924618
    Engertshofen,DE,2930157
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2930157
    Groß Grenz,DE,2915579
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2915579
    Dobrinje,BA,3201931
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3201931
    Strunino,RU,817682
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=817682
    Deebing Heights,AU,7932667
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7932667
    Spalding County,US,4223848
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4223848
    Auersthal,AT,2782204
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2782204
    Loncoche,CL,3882582
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3882582
    San Giuliano,IT,3168226
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3168226
    Busbys Flat,AU,2172989
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2172989
    Agua Verde,MX,4019219
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4019219
    Brok,PL,775304
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=775304
    Jozerand,FR,6440056
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6440056
    Beaupuy,FR,3034107
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3034107
    Estrees-Saint-Denis,FR,3019500
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3019500
    Holly Springs,US,4201047
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4201047
    Battlefield,US,4375979
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4375979
    Kasing,DE,2892545
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2892545
    Bullaun,IE,2965939
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2965939
    San Pedro Sula,HN,3601782
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3601782
    Polyarnyy,RU,1494463
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1494463
    Lukovetskiy,RU,533418
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=533418
    el Vendrell,ES,6361405
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6361405
    Gunjrauliya,IN,1270675
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1270675
    Bitkine,TD,2435124
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2435124
    Soto y Amío,ES,6358702
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6358702
    Kynsperk nad Ohri,CZ,3072394
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3072394
    Krumstedt,DE,2883238
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2883238
    Barbata,IT,6534955
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6534955
    Tordehumos,ES,6362286
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6362286
    Kolyberovo,RU,546013
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=546013
    Vasil’sursk,RU,476732
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=476732
    Hejiang,CN,1808612
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1808612
    Mailing,DE,2874267
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2874267
    Cicevac,RS,791936
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=791936
    Miaoling,CN,7745048
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7745048
    San Antonio,PH,1690303
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1690303
    Kinheim,DE,6554339
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6554339
    Salas Nocajski,RS,3191339
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3191339
    Sudnikovo,RU,486840
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=486840
    As,NO,3163608
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3163608
    Ashcroft,CA,5887531
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5887531
    Farciennes,BE,2798471
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2798471
    Piana di Monte Verna,IT,6535629
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6535629
    Nocera Umbra,IT,3172242
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3172242
    Fernsdorf,DE,2927090
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2927090
    Punata,BO,3907080
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3907080
    Nievern,DE,2862440
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2862440
    Orsfeld,DE,6554485
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6554485
    Ocentejo,ES,6357971
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6357971
    Ratesti,RO,669116
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=669116
    Odemira,PT,8010442
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=8010442
    Armillas,ES,3129493
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3129493
    Erlenbach,DE,6555516
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6555516
    Jaro,NP,7996816
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7996816
    Kelwa,IN,1267311
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1267311
    Stötten,DE,2826344
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2826344
    Poplar Hill,CA,6110958
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6110958
    Yaotsu,JP,1848518
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1848518
    Obshtina Dulovo,BG,731816
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=731816
    Colebrook,AU,2171050
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2171050
    Murillo Colonia,US,4713506
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=4713506
    Lagendorf,DE,2881942
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2881942
    Aranjuez,ES,3129857
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3129857
    Georgensgmund,DE,2921267
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2921267
    Batouri,CM,2234663
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2234663
    Matangad,PH,1700433
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1700433
    San Fernando,NI,3616871
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3616871
    Dock Sud,AR,3435050
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3435050
    Bojnice,SK,3061015
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3061015
    Tretiy Severnyy,RU,496290
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=496290
    Voerendaal,NL,2745369
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2745369
    San Miguel,PH,1688998
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1688998
    Castilla,PE,3698671
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3698671
    Homer,US,5121169
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5121169
    Duppach,DE,6554688
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6554688
    Natitingou,BJ,2392601
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2392601
    Kleinvollstedt,DE,2888063
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2888063
    Netstal,CH,2659497
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2659497
    São Miguel de Taipu,BR,3388272
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3388272
    Tret’ya Rota,RU,481725
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=481725
    Turner,US,5757706
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5757706
    Verkhniy Ufaley,RU,1487394
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=1487394
    Hassi Bahbah,DZ,2494073
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2494073
    Cranbury,US,5097006
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=5097006
    Hallsberg,SE,2708511
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2708511
    Tortuera,ES,6358051
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=6358051
    Zetten,NL,2743952
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2743952
    Lambeth,GB,3333163
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=3333163
    Baishazhou,CN,7636847
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=7636847
    Schiffdorf,DE,2839345
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=2839345
    Šapine,RS,786100
    http://api.openweathermap.org/data/2.5/weather?units=imperial&id=786100
    


```python
#Print the length of the lists to confirm the data was collected properly
print(len(namelist))
print(len(countrylist))
print(len(latitudelist))
print(len(temperaturelist))
print(len(humiditylist))
print(len(cloudinesslist))
print(len(windspeedlist))
```

    500
    500
    500
    500
    500
    500
    500
    


```python
#Create a dictionary to store all the collected data
weather_dict = {
    'Name': namelist,
    'ID': cityidlist,
    'Country': countrylist,
    'Cloudiness (%)': cloudinesslist,
    'Temperature (F)': temperaturelist,
    'Latitude (Deg)': latitudelist,
    'Humidity (%)': humiditylist,
    'Wind Speed (mph)': windspeedlist
}
#Convert to dataframe and label columns
weatherdf = pd.DataFrame(weather_dict, columns=('Name','ID', 'Country','Temperature (F)','Latitude (Deg)','Humidity (%)','Cloudiness (%)','Wind Speed (mph)'))
weatherdf.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>ID</th>
      <th>Country</th>
      <th>Temperature (F)</th>
      <th>Latitude (Deg)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Attnang-Puchheim</td>
      <td>3233832</td>
      <td>AT</td>
      <td>57.20</td>
      <td>48.01</td>
      <td>67</td>
      <td>20</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sabanagrande</td>
      <td>3602680</td>
      <td>HN</td>
      <td>82.40</td>
      <td>13.81</td>
      <td>48</td>
      <td>40</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Albánchez</td>
      <td>6355521</td>
      <td>ES</td>
      <td>59.00</td>
      <td>37.29</td>
      <td>67</td>
      <td>0</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Us</td>
      <td>2971316</td>
      <td>FR</td>
      <td>50.20</td>
      <td>49.10</td>
      <td>76</td>
      <td>75</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reghin</td>
      <td>668997</td>
      <td>RO</td>
      <td>52.77</td>
      <td>46.77</td>
      <td>81</td>
      <td>36</td>
      <td>2.24</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Convert this dataframe to a csv and put in in the Output folder
weatherdf.to_csv("Output/weather_data_csv")
```


```python
#Create Scatter Plot, label and save it to a png in the Output folder, then display the plot
plt.scatter(latitudelist, temperaturelist, marker="o", facecolors="red", edgecolors="black", alpha=0.75)
plt.xlabel("Latitude (Degrees)")
plt.ylabel("Temperature (Fahrenheit)")
plt.title("City Latitude vs Average Temperature on 4/4/2018")
plt.savefig("Output/Latitude_vs_Temperature")
#plt.grid()
plt.show()
```


![png](output_7_0.png)



```python
#Create Scatter Plot, label and save it to a png in the Output folder, then display the plot
plt.scatter(latitudelist, humiditylist, marker="o", facecolors="green", edgecolors="black", alpha=0.75)
plt.xlabel("Latitude (Degrees)")
plt.ylabel("Humidity (%)")
plt.title("City Latitude vs Humidity on 4/4/2018")
plt.savefig("Output/Latitude_vs_Humidity")
#plt.grid()
plt.show()
```


![png](output_8_0.png)



```python
#Create Scatter Plot, label and save it to a png in the Output folder, then display the plot
plt.scatter(latitudelist, cloudinesslist, marker="o", facecolors="blue", edgecolors="black", alpha=0.75)
plt.xlabel("Latitude (Degrees)")
plt.ylabel("Cloudiness (%)")
plt.title("City Latitude vs Cloudiness on 4/4/2018")
plt.savefig("Output/Latitude_vs_Cloudiness")
#plt.grid()
plt.show()
```


![png](output_9_0.png)



```python
#Create Scatter Plot, label and save it to a png in the Output folder, then display the plot
plt.scatter(latitudelist, windspeedlist, marker="o", facecolors="purple", edgecolors="black", alpha=0.75)
plt.xlabel("Latitude (Degrees)")
plt.ylabel("Wind Speed (mph)")
plt.title("City Latitude vs Wind Speed on 4/4/2018")
plt.savefig("Output/Latitude_vs_Wind_Speed")
#plt.grid()
plt.show()
```


![png](output_10_0.png)



```python
#Three Observable Trends
# 1. Temperature is a lot warmer in the Southern Hemisphere than the Northern Hemisphere. But the warmest points are nearest to the equator.
# 2. There were a lot more sampled cities in the Northern Hemisphere (Likely a lot more cities in the Northern Hemisphere for the open weather database)
# 3. Latitude did not have a strong effect on humidity, wind speed, or cloudiness for the sampled day.
```
