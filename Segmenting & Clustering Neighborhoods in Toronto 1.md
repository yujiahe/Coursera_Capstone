
## Segmenting and Clustering Neighborhoods in Toronto PART 1

IBM DATA SCIENCE PROFESSIONAL CERTIFICATE - # 9 Applied Data Science Capstone - WEEK 3 | COURSERA

### 1. Data Scraping and Data Cleaning


```python
# import the libraries
import pandas as pd
import numpy as np
import requests
from bs4 import BeautifulSoup
```


```python
# scrap data from the wiki page and transform the data to a data frame
website_url = requests.get('https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M').text
soup = BeautifulSoup(website_url, 'lxml')
table = soup.find_all('table')[0]
df = pd.read_html(str(table))[0]
df = df.rename(columns=df.iloc[0]).drop(df.index[0])
```


```python
# the dataframe consist of three columns: PostalCode, Borough, and Neighborhood
df[0:5]
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>M1A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M2A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront</td>
    </tr>
  </tbody>
</table>
</div>




```python
# drop the rows that have "not assigned" neighbourhood
df.drop(df[df.Borough == 'Not assigned'].index, inplace=True)
df = df.reset_index()
```


```python
# drop the old index column
df = df.drop(['index'], axis = 1)
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M9A</td>
      <td>Etobicoke</td>
      <td>Islington Avenue</td>
    </tr>
    <tr>
      <th>8</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Rouge</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Malvern</td>
    </tr>
    <tr>
      <th>10</th>
      <td>M3B</td>
      <td>North York</td>
      <td>Don Mills North</td>
    </tr>
    <tr>
      <th>11</th>
      <td>M4B</td>
      <td>East York</td>
      <td>Woodbine Gardens</td>
    </tr>
    <tr>
      <th>12</th>
      <td>M4B</td>
      <td>East York</td>
      <td>Parkview Hill</td>
    </tr>
    <tr>
      <th>13</th>
      <td>M5B</td>
      <td>Downtown Toronto</td>
      <td>Ryerson</td>
    </tr>
    <tr>
      <th>14</th>
      <td>M5B</td>
      <td>Downtown Toronto</td>
      <td>Garden District</td>
    </tr>
    <tr>
      <th>15</th>
      <td>M6B</td>
      <td>North York</td>
      <td>Glencairn</td>
    </tr>
    <tr>
      <th>16</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>Cloverdale</td>
    </tr>
    <tr>
      <th>17</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>Islington</td>
    </tr>
    <tr>
      <th>18</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>Martin Grove</td>
    </tr>
    <tr>
      <th>19</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>Princess Gardens</td>
    </tr>
    <tr>
      <th>20</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>West Deane Park</td>
    </tr>
    <tr>
      <th>21</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Highland Creek</td>
    </tr>
    <tr>
      <th>22</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Rouge Hill</td>
    </tr>
    <tr>
      <th>23</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Port Union</td>
    </tr>
    <tr>
      <th>24</th>
      <td>M3C</td>
      <td>North York</td>
      <td>Flemingdon Park</td>
    </tr>
    <tr>
      <th>25</th>
      <td>M3C</td>
      <td>North York</td>
      <td>Don Mills South</td>
    </tr>
    <tr>
      <th>26</th>
      <td>M4C</td>
      <td>East York</td>
      <td>Woodbine Heights</td>
    </tr>
    <tr>
      <th>27</th>
      <td>M5C</td>
      <td>Downtown Toronto</td>
      <td>St. James Town</td>
    </tr>
    <tr>
      <th>28</th>
      <td>M6C</td>
      <td>York</td>
      <td>Humewood-Cedarvale</td>
    </tr>
    <tr>
      <th>29</th>
      <td>M9C</td>
      <td>Etobicoke</td>
      <td>Bloordale Gardens</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>182</th>
      <td>M1W</td>
      <td>Scarborough</td>
      <td>L'Amoreaux West</td>
    </tr>
    <tr>
      <th>183</th>
      <td>M1W</td>
      <td>Scarborough</td>
      <td>Steeles West</td>
    </tr>
    <tr>
      <th>184</th>
      <td>M4W</td>
      <td>Downtown Toronto</td>
      <td>Rosedale</td>
    </tr>
    <tr>
      <th>185</th>
      <td>M5W</td>
      <td>Downtown Toronto</td>
      <td>Stn A PO Boxes 25 The Esplanade</td>
    </tr>
    <tr>
      <th>186</th>
      <td>M8W</td>
      <td>Etobicoke</td>
      <td>Alderwood</td>
    </tr>
    <tr>
      <th>187</th>
      <td>M8W</td>
      <td>Etobicoke</td>
      <td>Long Branch</td>
    </tr>
    <tr>
      <th>188</th>
      <td>M9W</td>
      <td>Etobicoke</td>
      <td>Northwest</td>
    </tr>
    <tr>
      <th>189</th>
      <td>M1X</td>
      <td>Scarborough</td>
      <td>Upper Rouge</td>
    </tr>
    <tr>
      <th>190</th>
      <td>M4X</td>
      <td>Downtown Toronto</td>
      <td>Cabbagetown</td>
    </tr>
    <tr>
      <th>191</th>
      <td>M4X</td>
      <td>Downtown Toronto</td>
      <td>St. James Town</td>
    </tr>
    <tr>
      <th>192</th>
      <td>M5X</td>
      <td>Downtown Toronto</td>
      <td>First Canadian Place</td>
    </tr>
    <tr>
      <th>193</th>
      <td>M5X</td>
      <td>Downtown Toronto</td>
      <td>Underground city</td>
    </tr>
    <tr>
      <th>194</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>The Kingsway</td>
    </tr>
    <tr>
      <th>195</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>Montgomery Road</td>
    </tr>
    <tr>
      <th>196</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>Old Mill North</td>
    </tr>
    <tr>
      <th>197</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
    </tr>
    <tr>
      <th>198</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td>Business Reply Mail Processing Centre 969 Eastern</td>
    </tr>
    <tr>
      <th>199</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Humber Bay</td>
    </tr>
    <tr>
      <th>200</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>King's Mill Park</td>
    </tr>
    <tr>
      <th>201</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Kingsway Park South East</td>
    </tr>
    <tr>
      <th>202</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Mimico NE</td>
    </tr>
    <tr>
      <th>203</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Old Mill South</td>
    </tr>
    <tr>
      <th>204</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>The Queensway East</td>
    </tr>
    <tr>
      <th>205</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Royal York South East</td>
    </tr>
    <tr>
      <th>206</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Sunnylea</td>
    </tr>
    <tr>
      <th>207</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Kingsway Park South West</td>
    </tr>
    <tr>
      <th>208</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Mimico NW</td>
    </tr>
    <tr>
      <th>209</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>The Queensway West</td>
    </tr>
    <tr>
      <th>210</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Royal York South West</td>
    </tr>
    <tr>
      <th>211</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>South of Bloor</td>
    </tr>
  </tbody>
</table>
<p>212 rows × 3 columns</p>
</div>




```python
# For the cell that has a borough but a "not assigned" neighbourhood,
# we make the neighbourhood the same as the borough
df[df.Neighbourhood == 'Not assigned']
df.at[df[df.Neighbourhood == 'Not assigned'].index, 'Neighbourhood']= "Queen's Park"
df[0:7]
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Queen's Park</td>
    </tr>
  </tbody>
</table>
</div>




```python
# combine the neighbourhoods that have the same postal code into one row
# separate the neighbourhoods with commas
df = pd.DataFrame(df.groupby(['Postcode','Borough'])['Neighbourhood'].apply(list)).reset_index()
df[0:5]
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
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>[Rouge, Malvern]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>[Highland Creek, Rouge Hill, Port Union]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>Scarborough</td>
      <td>[Guildwood, Morningside, West Hill]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>Scarborough</td>
      <td>[Woburn]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>Scarborough</td>
      <td>[Cedarbrae]</td>
    </tr>
  </tbody>
</table>
</div>




```python
# print the number of rows of your dataframe
df.shape
```




    (103, 3)




```python

```
