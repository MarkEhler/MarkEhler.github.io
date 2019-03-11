---
layout: post
title:      "Dartmouth Wilderness Trout Tagging"
date:       2019-03-11 18:30:41 +0000
permalink:  dartmouth_wilderness_trout_tagging
---


The following notebook is a work in progress for the data collected form trout spawning waters in nothern New Hampshire.

Data was collected in the Summer and Fall of 2018 and the following data set describes the information recorded form those efforts.

The goal of this notebook is to make something useful from a messy dataset.  Lots of effort was put into collecting this data,

but it is hardly useful to a data scientist.  Many real world situations will find you with less than ideal resources to complete a task.

This notebook seeks to make something deliverable from a difficult situation.

```
import pandas as pd
import numpy as np

pass1 = pd.read_excel("C:/Users/Mark/Desktop/misc_stuff/Wanding_Combined.xlsx", 0)
pass2 = pd.read_excel("C:/Users/Mark/Desktop/misc_stuff/Wanding_Combined.xlsx", 1)
pass3 = pd.read_excel("C:/Users/Mark/Desktop/misc_stuff/Wanding_Combined.xlsx", 2)

print(pass1.shape, pass2.shape, pass3.shape)
(4018, 19) (3907, 19) (2809, 18)
```


`df.isna().sum()`

Scan Date          78
Scan Time          78
Download Date      66
Download Time      66
Reader ID        4508
HEX Tag ID        103
DEC Tag ID         13
Meter            1320
Habitat          1423
Wood             5699
Latitude           88
Longitude          50
File Name          26
Wander           6568
Unit             6568
Start            8852
End              8280
Wood?            8023
start            9409

We're only going to scratch the surface of the cleaning we need to do on this data set.


```
df.Latitude.replace('+','')
df = df.drop(['Dead or Live', 'Live/Dead?', 'Unnamed: 18'], axis=1)
df.columns
Index(['Scan Date', 'Scan Time', 'Download Date', 'Download Time', 'Reader ID',
       'HEX Tag ID', 'DEC Tag ID', 'Meter', 'Habitat', 'Wood', 'Latitude',
       'Longitude', 'File Name', 'Wander', 'Unit', 'Start', 'End', 'Wood?',
       'start'],
      dtype='object')
```

Knowing that Ali was the worker who put in the most hours on the project and that women are under represented in the field I can assume that Alison tagged more than she was credited.  We will be making a map, an easy way to find a base map starting point is to get the mean of all the locations.

```

df2 = df.dropna(subset = ['Longitude', 'Latitude'])
print(f' Lon Center: {df2.Longitude.mean()} \n Lat Center {df2.Latitude.mean()}')

df2.Wander = df2.Wander.fillna("Alison")
df2.head()
```


```
import folium


long = -71.09671
lat = 44.9239

#Create a map of the area
base_map = folium.Map([lat, long], zoom_start=20)
base_map
```

we have thousands of tags, so we will have to break this data into subsets to make them digestable.

```
import numpy as np

#Generate some random locations to add to our map
samp = df2.sample(50)
x = samp.Latitude
y = samp.Longitude
desc = samp.Wander
points = list(zip(x, y, desc))
foliumdata = pd.concat([x,y,desc], axis=1, ignore_index = True)

#just another way to look at the geodata

foliumdata.head()
```

We will now make a map with locations of the fish as they were found and a popup for the person who found the fish.  This can be used to judge worker preformance and territories.

```
for idx, data in enumerate(points):
    lat = x.iloc[idx]
    long = y.iloc[idx]
    popup_txt = desc.iloc[idx]
    popup = folium.Popup(popup_txt, parse_html=True)
    marker = folium.Marker(location=[lat, long], popup = popup)
    marker.add_to(base_map)
base_map
```

from here we could also label the tags by which features the fish was found by.  We could color the markers according to the season they were found in order to track any migration patterns that might exist, but this is a start.



