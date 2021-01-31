```python
import requests
from bs4 import BeautifulSoup
from time import sleep
import pickle
import json
import re
import pandas as pd
import altair as alt

```


```python
with open('restaurant_details_loc.json') as json_file: 
    rlist = json.load(json_file)
```


```python

```


```python
for i in range(len(rlist)):
    rlist[i]['year']=int(re.search('[0-9]?[0-9]',rlist[i]['season']).group(0))
```

# Diners, Drive-Ins, and Lies:
## Guy Fieri's Most Commonly Visited Restaurants

The Food Network Show "Diners, Drive-Ins, and Dives" has been a television staple since 2007. Since then, Guy Fieri and crew have been to over 1,000 restaurants in the United States, and even some international locations. Not even COVID-19 can stop the Mayor of Flavortown: current episodes of the show feature the restaurants sending their food to him. The premise of the show was simple: to celebrate the unsung heroes of the dining scene: your greasy spoons, dimly lit hometown hangouts, and the remnants of mid-century Americana. However, a more recent viewer of the program may be left confused, as it seems that the premise has been abandoned. I was interested in finding this out.

\* Note: I realize it has been a meme for quite some time to give Guy Fieri grief. So I do want to state that this is by no means an attempt to drag Mr. Fieri through the mud. He has done an admirable job highlighting many hidden gems across the country and has built deep relationships with many of them. I do not want to diminish that.

Acquiring a list of locations visited is actually quite straightforward. There are two sources of information: Food Network's own website (https://www.foodnetwork.com/restaurants/shows/diners-drive-ins-and-dives) and a fan site (https://www.dinersdriveinsdiveslocations.com/). Both list the restaurant  name, address, and a brief description. I chose the latter, because it also provided season numbers for each location. 

I then scraped the website for all locations in the United States and Canada (I avoided international locations, given that dining cultures there might be such that the concept of a "dive" would be an entirely different concept). 

I would then perform some text analysis of each restaurants description, looking for descriptive words that might describe a restaurant. This included diners, drive-ins, dives, cafes, bars, bbq, pubs, brunch, bakerys, and tex-mex. Beyond the Big 3, these groups came from eyeballing the descriptions. 

It is also worth noting any given restaurant could be put into multiple categories.


```python
names=[]
diners=[]
driveins=[]
dives=[]
cafes=[]
bars=[]
bbq=[]
pubs=[]
brunch=[]
gas=[]
joint=[]
season=[]
mex=[]
bake=[]
for r in rlist:
    names.append(r['name'])
    season.append(r['year'])
    if re.search(" dive | divey | dive-y ",r['desc'].lower()):
        dives.append(1)
    else:
        dives.append(0)
    if re.search("diner|diners",r['desc'].lower()):
        diners.append(1)
    else:
        diners.append(0)
    if re.search("driveins|drive-ins|drive ins|drivein|drive-in|drive in",r['desc'].lower()):
        driveins.append(1)
    else:
        driveins.append(0)
        
    if re.search(" cafe| cafes",r['desc'].lower()):
        cafes.append(1)
    else:
        cafes.append(0)
        
    if re.search(" barbecue| barbeque| bbq| bbqs| barbeques |barbecue",r['desc'].lower()):
        bbq.append(1)
    else:
        bbq.append(0)
                
    if re.search(" bar| bars",r['desc'].lower()):
        bars.append(1)
    else:
        bars.append(0)
        
    if re.search(" pub| pubs| gastropub| gastropubs",r['desc'].lower()):
        pubs.append(1)
    else:
        pubs.append(0)
    if re.search(" brunch| brunch spot",r['desc'].lower()):
        brunch.append(1)
    else:
        brunch.append(0)
        
    if re.search(" joint| joints",r['desc'].lower()):
        joint.append(1)
    else:
        joint.append(0)        
    if re.search(" gas station| gas stations| pump station| pump stations| fuel station| fuel stations",r['desc'].lower()):
        gas.append(1)
    else:
        gas.append(0)
        
    if re.search(" mexican| taqueria",r['desc'].lower()):
        mex.append(1)
    else:
        mex.append(0)
        
    if re.search(" bakery",r['desc'].lower()):
        bake.append(1)
    else:
        bake.append(0)
wordlist=pd.DataFrame(list(zip(names,season,diners,driveins,dives,cafes,bbq,bars,pubs,brunch,gas,joint,mex,bake)),columns=['name','season','diner','driveins','dives','cafes','bbq','bars','pubs','brunch','gas station','joint','mexican','bakery'])
```


```python
Q=wordlist.mean(axis=0)[2:15].copy().reset_index()
Q.columns=['Category','Proportion']
```

In the end, I had a list of 1185 restaurants.

A list of categories and their counts can be found here:


```python

```


```python
Q.style.hide_index()
```




<style  type="text/css" >
</style><table id="T_ab8c8990_6399_11eb_a7d2_e03f491329ca" ><thead>    <tr>        <th class="col_heading level0 col0" >Category</th>        <th class="col_heading level0 col1" >Proportion</th>    </tr></thead><tbody>
                <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow0_col0" class="data row0 col0" >driveins</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow0_col1" class="data row0 col1" >0.010127</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow1_col0" class="data row1 col0" >dives</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow1_col1" class="data row1 col1" >0.026160</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow2_col0" class="data row2 col0" >cafes</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow2_col1" class="data row2 col1" >0.066667</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow3_col0" class="data row3 col0" >bbq</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow3_col1" class="data row3 col1" >0.108861</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow4_col0" class="data row4 col0" >bars</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow4_col1" class="data row4 col1" >0.176371</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow5_col0" class="data row5 col0" >pubs</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow5_col1" class="data row5 col1" >0.064135</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow6_col0" class="data row6 col0" >brunch</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow6_col1" class="data row6 col1" >0.038819</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow7_col0" class="data row7 col0" >gas station</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow7_col1" class="data row7 col1" >0.006751</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow8_col0" class="data row8 col0" >joint</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow8_col1" class="data row8 col1" >0.145148</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow9_col0" class="data row9 col0" >mexican</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow9_col1" class="data row9 col1" >0.062447</td>
            </tr>
            <tr>
                                <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow10_col0" class="data row10 col0" >bakery</td>
                        <td id="T_ab8c8990_6399_11eb_a7d2_e03f491329carow10_col1" class="data row10 col1" >0.022785</td>
            </tr>
    </tbody></table>



However, I was interested in how these have changed over time. So I broke these down by season:


```python

WL=wordlist.groupby('season').mean()
W=WL.reset_index()[:32]
```


```python
WDF=pd.melt(W, id_vars=['season'])
```


```python

```


```python
def titling(x):
    return x.title()
```


```python
WDF['variable']=WDF['variable'].apply(titling)
```


```python

```


```python
alt.Chart(WDF).mark_line().encode(
    x=alt.X('season:Q',scale=alt.Scale(domain=[1, 31])),
    y=alt.Y('value:Q',title='Proportion of Restaurants'),
    facet=alt.Facet('variable:N',columns=4,title=None),
).properties(
    width=240,height=240).configure_axis(
    labelFontSize=12,
    titleFontSize=18
).configure_headerFacet(
    labelFontSize=20,
    titleFontSize=25
)
```





<div id="altair-viz-b94d034273cc4f6b8351757559918189"></div>
<script type="text/javascript">
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-b94d034273cc4f6b8351757559918189") {
      outputDiv = document.getElementById("altair-viz-b94d034273cc4f6b8351757559918189");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.8.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function loadScript(lib) {
      return new Promise(function(resolve, reject) {
        var s = document.createElement('script');
        s.src = paths[lib];
        s.async = true;
        s.onload = () => resolve(paths[lib]);
        s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
        document.getElementsByTagName("head")[0].appendChild(s);
      });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else if (typeof vegaEmbed === "function") {
      displayChart(vegaEmbed);
    } else {
      loadScript("vega")
        .then(() => loadScript("vega-lite"))
        .then(() => loadScript("vega-embed"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}, "axis": {"labelFontSize": 12, "titleFontSize": 18}, "headerFacet": {"labelFontSize": 20, "titleFontSize": 25}}, "data": {"name": "data-8e93dedf0bddf0a4acdd2c1491ebf85d"}, "mark": "line", "encoding": {"facet": {"type": "nominal", "columns": 4, "field": "variable", "title": null}, "x": {"type": "quantitative", "field": "season", "scale": {"domain": [1, 31]}}, "y": {"type": "quantitative", "field": "value", "title": "Proportion of Restaurants"}}, "height": 240, "width": 240, "$schema": "https://vega.github.io/schema/vega-lite/v4.8.1.json", "datasets": {"data-8e93dedf0bddf0a4acdd2c1491ebf85d": [{"season": 0, "variable": "Diner", "value": 0.16666666666666666}, {"season": 1, "variable": "Diner", "value": 0.7352941176470589}, {"season": 2, "variable": "Diner", "value": 0.9743589743589743}, {"season": 3, "variable": "Diner", "value": 0.7619047619047619}, {"season": 4, "variable": "Diner", "value": 0.8717948717948718}, {"season": 5, "variable": "Diner", "value": 0.7222222222222222}, {"season": 6, "variable": "Diner", "value": 0.48717948717948717}, {"season": 7, "variable": "Diner", "value": 0.6666666666666666}, {"season": 8, "variable": "Diner", "value": 0.6129032258064516}, {"season": 9, "variable": "Diner", "value": 0.42105263157894735}, {"season": 10, "variable": "Diner", "value": 0.5135135135135135}, {"season": 11, "variable": "Diner", "value": 0.41025641025641024}, {"season": 12, "variable": "Diner", "value": 0.23076923076923078}, {"season": 13, "variable": "Diner", "value": 0.24324324324324326}, {"season": 14, "variable": "Diner", "value": 0.23529411764705882}, {"season": 15, "variable": "Diner", "value": 0.29411764705882354}, {"season": 16, "variable": "Diner", "value": 0.19047619047619047}, {"season": 17, "variable": "Diner", "value": 0.25806451612903225}, {"season": 18, "variable": "Diner", "value": 0.12121212121212122}, {"season": 19, "variable": "Diner", "value": 0.07692307692307693}, {"season": 20, "variable": "Diner", "value": 0.047619047619047616}, {"season": 21, "variable": "Diner", "value": 0.05714285714285714}, {"season": 22, "variable": "Diner", "value": 0.1111111111111111}, {"season": 23, "variable": "Diner", "value": 0.09090909090909091}, {"season": 24, "variable": "Diner", "value": 0.15789473684210525}, {"season": 25, "variable": "Diner", "value": 0.05}, {"season": 26, "variable": "Diner", "value": 0.06451612903225806}, {"season": 27, "variable": "Diner", "value": 0.0}, {"season": 28, "variable": "Diner", "value": 0.057692307692307696}, {"season": 29, "variable": "Diner", "value": 0.03508771929824561}, {"season": 30, "variable": "Diner", "value": 0.03333333333333333}, {"season": 31, "variable": "Diner", "value": 0.16279069767441862}, {"season": 0, "variable": "Driveins", "value": 0.0}, {"season": 1, "variable": "Driveins", "value": 0.058823529411764705}, {"season": 2, "variable": "Driveins", "value": 0.02564102564102564}, {"season": 3, "variable": "Driveins", "value": 0.023809523809523808}, {"season": 4, "variable": "Driveins", "value": 0.0}, {"season": 5, "variable": "Driveins", "value": 0.0}, {"season": 6, "variable": "Driveins", "value": 0.05128205128205128}, {"season": 7, "variable": "Driveins", "value": 0.02564102564102564}, {"season": 8, "variable": "Driveins", "value": 0.03225806451612903}, {"season": 9, "variable": "Driveins", "value": 0.02631578947368421}, {"season": 10, "variable": "Driveins", "value": 0.02702702702702703}, {"season": 11, "variable": "Driveins", "value": 0.0}, {"season": 12, "variable": "Driveins", "value": 0.0}, {"season": 13, "variable": "Driveins", "value": 0.0}, {"season": 14, "variable": "Driveins", "value": 0.0}, {"season": 15, "variable": "Driveins", "value": 0.0}, {"season": 16, "variable": "Driveins", "value": 0.0}, {"season": 17, "variable": "Driveins", "value": 0.0}, {"season": 18, "variable": "Driveins", "value": 0.0}, {"season": 19, "variable": "Driveins", "value": 0.02564102564102564}, {"season": 20, "variable": "Driveins", "value": 0.0}, {"season": 21, "variable": "Driveins", "value": 0.02857142857142857}, {"season": 22, "variable": "Driveins", "value": 0.0}, {"season": 23, "variable": "Driveins", "value": 0.0}, {"season": 24, "variable": "Driveins", "value": 0.0}, {"season": 25, "variable": "Driveins", "value": 0.0}, {"season": 26, "variable": "Driveins", "value": 0.0}, {"season": 27, "variable": "Driveins", "value": 0.0}, {"season": 28, "variable": "Driveins", "value": 0.0}, {"season": 29, "variable": "Driveins", "value": 0.0}, {"season": 30, "variable": "Driveins", "value": 0.0}, {"season": 31, "variable": "Driveins", "value": 0.0}, {"season": 0, "variable": "Dives", "value": 0.0}, {"season": 1, "variable": "Dives", "value": 0.08823529411764706}, {"season": 2, "variable": "Dives", "value": 0.07692307692307693}, {"season": 3, "variable": "Dives", "value": 0.07142857142857142}, {"season": 4, "variable": "Dives", "value": 0.05128205128205128}, {"season": 5, "variable": "Dives", "value": 0.08333333333333333}, {"season": 6, "variable": "Dives", "value": 0.02564102564102564}, {"season": 7, "variable": "Dives", "value": 0.02564102564102564}, {"season": 8, "variable": "Dives", "value": 0.03225806451612903}, {"season": 9, "variable": "Dives", "value": 0.02631578947368421}, {"season": 10, "variable": "Dives", "value": 0.13513513513513514}, {"season": 11, "variable": "Dives", "value": 0.02564102564102564}, {"season": 12, "variable": "Dives", "value": 0.0}, {"season": 13, "variable": "Dives", "value": 0.0}, {"season": 14, "variable": "Dives", "value": 0.0}, {"season": 15, "variable": "Dives", "value": 0.0}, {"season": 16, "variable": "Dives", "value": 0.0}, {"season": 17, "variable": "Dives", "value": 0.03225806451612903}, {"season": 18, "variable": "Dives", "value": 0.0}, {"season": 19, "variable": "Dives", "value": 0.0}, {"season": 20, "variable": "Dives", "value": 0.0}, {"season": 21, "variable": "Dives", "value": 0.05714285714285714}, {"season": 22, "variable": "Dives", "value": 0.0}, {"season": 23, "variable": "Dives", "value": 0.0}, {"season": 24, "variable": "Dives", "value": 0.02631578947368421}, {"season": 25, "variable": "Dives", "value": 0.0}, {"season": 26, "variable": "Dives", "value": 0.0}, {"season": 27, "variable": "Dives", "value": 0.0}, {"season": 28, "variable": "Dives", "value": 0.0}, {"season": 29, "variable": "Dives", "value": 0.0}, {"season": 30, "variable": "Dives", "value": 0.016666666666666666}, {"season": 31, "variable": "Dives", "value": 0.023255813953488372}, {"season": 0, "variable": "Cafes", "value": 0.16666666666666666}, {"season": 1, "variable": "Cafes", "value": 0.0}, {"season": 2, "variable": "Cafes", "value": 0.02564102564102564}, {"season": 3, "variable": "Cafes", "value": 0.07142857142857142}, {"season": 4, "variable": "Cafes", "value": 0.07692307692307693}, {"season": 5, "variable": "Cafes", "value": 0.08333333333333333}, {"season": 6, "variable": "Cafes", "value": 0.23076923076923078}, {"season": 7, "variable": "Cafes", "value": 0.10256410256410256}, {"season": 8, "variable": "Cafes", "value": 0.03225806451612903}, {"season": 9, "variable": "Cafes", "value": 0.02631578947368421}, {"season": 10, "variable": "Cafes", "value": 0.13513513513513514}, {"season": 11, "variable": "Cafes", "value": 0.3076923076923077}, {"season": 12, "variable": "Cafes", "value": 0.05128205128205128}, {"season": 13, "variable": "Cafes", "value": 0.0}, {"season": 14, "variable": "Cafes", "value": 0.029411764705882353}, {"season": 15, "variable": "Cafes", "value": 0.058823529411764705}, {"season": 16, "variable": "Cafes", "value": 0.0}, {"season": 17, "variable": "Cafes", "value": 0.0}, {"season": 18, "variable": "Cafes", "value": 0.030303030303030304}, {"season": 19, "variable": "Cafes", "value": 0.02564102564102564}, {"season": 20, "variable": "Cafes", "value": 0.0}, {"season": 21, "variable": "Cafes", "value": 0.05714285714285714}, {"season": 22, "variable": "Cafes", "value": 0.037037037037037035}, {"season": 23, "variable": "Cafes", "value": 0.06060606060606061}, {"season": 24, "variable": "Cafes", "value": 0.02631578947368421}, {"season": 25, "variable": "Cafes", "value": 0.05}, {"season": 26, "variable": "Cafes", "value": 0.03225806451612903}, {"season": 27, "variable": "Cafes", "value": 0.0}, {"season": 28, "variable": "Cafes", "value": 0.07692307692307693}, {"season": 29, "variable": "Cafes", "value": 0.14035087719298245}, {"season": 30, "variable": "Cafes", "value": 0.05}, {"season": 31, "variable": "Cafes", "value": 0.09302325581395349}, {"season": 0, "variable": "Bbq", "value": 0.16666666666666666}, {"season": 1, "variable": "Bbq", "value": 0.11764705882352941}, {"season": 2, "variable": "Bbq", "value": 0.10256410256410256}, {"season": 3, "variable": "Bbq", "value": 0.23809523809523808}, {"season": 4, "variable": "Bbq", "value": 0.0}, {"season": 5, "variable": "Bbq", "value": 0.1388888888888889}, {"season": 6, "variable": "Bbq", "value": 0.05128205128205128}, {"season": 7, "variable": "Bbq", "value": 0.07692307692307693}, {"season": 8, "variable": "Bbq", "value": 0.0967741935483871}, {"season": 9, "variable": "Bbq", "value": 0.18421052631578946}, {"season": 10, "variable": "Bbq", "value": 0.10810810810810811}, {"season": 11, "variable": "Bbq", "value": 0.05128205128205128}, {"season": 12, "variable": "Bbq", "value": 0.05128205128205128}, {"season": 13, "variable": "Bbq", "value": 0.0}, {"season": 14, "variable": "Bbq", "value": 0.11764705882352941}, {"season": 15, "variable": "Bbq", "value": 0.08823529411764706}, {"season": 16, "variable": "Bbq", "value": 0.09523809523809523}, {"season": 17, "variable": "Bbq", "value": 0.12903225806451613}, {"season": 18, "variable": "Bbq", "value": 0.18181818181818182}, {"season": 19, "variable": "Bbq", "value": 0.10256410256410256}, {"season": 20, "variable": "Bbq", "value": 0.09523809523809523}, {"season": 21, "variable": "Bbq", "value": 0.14285714285714285}, {"season": 22, "variable": "Bbq", "value": 0.14814814814814814}, {"season": 23, "variable": "Bbq", "value": 0.21212121212121213}, {"season": 24, "variable": "Bbq", "value": 0.13157894736842105}, {"season": 25, "variable": "Bbq", "value": 0.075}, {"season": 26, "variable": "Bbq", "value": 0.12903225806451613}, {"season": 27, "variable": "Bbq", "value": 0.17142857142857143}, {"season": 28, "variable": "Bbq", "value": 0.057692307692307696}, {"season": 29, "variable": "Bbq", "value": 0.12280701754385964}, {"season": 30, "variable": "Bbq", "value": 0.11666666666666667}, {"season": 31, "variable": "Bbq", "value": 0.13953488372093023}, {"season": 0, "variable": "Bars", "value": 0.5}, {"season": 1, "variable": "Bars", "value": 0.14705882352941177}, {"season": 2, "variable": "Bars", "value": 0.15384615384615385}, {"season": 3, "variable": "Bars", "value": 0.3333333333333333}, {"season": 4, "variable": "Bars", "value": 0.2564102564102564}, {"season": 5, "variable": "Bars", "value": 0.16666666666666666}, {"season": 6, "variable": "Bars", "value": 0.28205128205128205}, {"season": 7, "variable": "Bars", "value": 0.20512820512820512}, {"season": 8, "variable": "Bars", "value": 0.1935483870967742}, {"season": 9, "variable": "Bars", "value": 0.18421052631578946}, {"season": 10, "variable": "Bars", "value": 0.24324324324324326}, {"season": 11, "variable": "Bars", "value": 0.20512820512820512}, {"season": 12, "variable": "Bars", "value": 0.07692307692307693}, {"season": 13, "variable": "Bars", "value": 0.02702702702702703}, {"season": 14, "variable": "Bars", "value": 0.029411764705882353}, {"season": 15, "variable": "Bars", "value": 0.029411764705882353}, {"season": 16, "variable": "Bars", "value": 0.047619047619047616}, {"season": 17, "variable": "Bars", "value": 0.12903225806451613}, {"season": 18, "variable": "Bars", "value": 0.06060606060606061}, {"season": 19, "variable": "Bars", "value": 0.15384615384615385}, {"season": 20, "variable": "Bars", "value": 0.38095238095238093}, {"season": 21, "variable": "Bars", "value": 0.2857142857142857}, {"season": 22, "variable": "Bars", "value": 0.07407407407407407}, {"season": 23, "variable": "Bars", "value": 0.2727272727272727}, {"season": 24, "variable": "Bars", "value": 0.15789473684210525}, {"season": 25, "variable": "Bars", "value": 0.15}, {"season": 26, "variable": "Bars", "value": 0.0967741935483871}, {"season": 27, "variable": "Bars", "value": 0.14285714285714285}, {"season": 28, "variable": "Bars", "value": 0.11538461538461539}, {"season": 29, "variable": "Bars", "value": 0.19298245614035087}, {"season": 30, "variable": "Bars", "value": 0.2833333333333333}, {"season": 31, "variable": "Bars", "value": 0.16279069767441862}, {"season": 0, "variable": "Pubs", "value": 0.0}, {"season": 1, "variable": "Pubs", "value": 0.029411764705882353}, {"season": 2, "variable": "Pubs", "value": 0.02564102564102564}, {"season": 3, "variable": "Pubs", "value": 0.047619047619047616}, {"season": 4, "variable": "Pubs", "value": 0.07692307692307693}, {"season": 5, "variable": "Pubs", "value": 0.0}, {"season": 6, "variable": "Pubs", "value": 0.05128205128205128}, {"season": 7, "variable": "Pubs", "value": 0.0}, {"season": 8, "variable": "Pubs", "value": 0.16129032258064516}, {"season": 9, "variable": "Pubs", "value": 0.10526315789473684}, {"season": 10, "variable": "Pubs", "value": 0.13513513513513514}, {"season": 11, "variable": "Pubs", "value": 0.10256410256410256}, {"season": 12, "variable": "Pubs", "value": 0.07692307692307693}, {"season": 13, "variable": "Pubs", "value": 0.02702702702702703}, {"season": 14, "variable": "Pubs", "value": 0.029411764705882353}, {"season": 15, "variable": "Pubs", "value": 0.08823529411764706}, {"season": 16, "variable": "Pubs", "value": 0.09523809523809523}, {"season": 17, "variable": "Pubs", "value": 0.06451612903225806}, {"season": 18, "variable": "Pubs", "value": 0.0}, {"season": 19, "variable": "Pubs", "value": 0.0}, {"season": 20, "variable": "Pubs", "value": 0.14285714285714285}, {"season": 21, "variable": "Pubs", "value": 0.02857142857142857}, {"season": 22, "variable": "Pubs", "value": 0.037037037037037035}, {"season": 23, "variable": "Pubs", "value": 0.12121212121212122}, {"season": 24, "variable": "Pubs", "value": 0.07894736842105263}, {"season": 25, "variable": "Pubs", "value": 0.05}, {"season": 26, "variable": "Pubs", "value": 0.06451612903225806}, {"season": 27, "variable": "Pubs", "value": 0.05714285714285714}, {"season": 28, "variable": "Pubs", "value": 0.038461538461538464}, {"season": 29, "variable": "Pubs", "value": 0.08771929824561403}, {"season": 30, "variable": "Pubs", "value": 0.11666666666666667}, {"season": 31, "variable": "Pubs", "value": 0.023255813953488372}, {"season": 0, "variable": "Brunch", "value": 0.0}, {"season": 1, "variable": "Brunch", "value": 0.029411764705882353}, {"season": 2, "variable": "Brunch", "value": 0.02564102564102564}, {"season": 3, "variable": "Brunch", "value": 0.0}, {"season": 4, "variable": "Brunch", "value": 0.0}, {"season": 5, "variable": "Brunch", "value": 0.0}, {"season": 6, "variable": "Brunch", "value": 0.1282051282051282}, {"season": 7, "variable": "Brunch", "value": 0.05128205128205128}, {"season": 8, "variable": "Brunch", "value": 0.06451612903225806}, {"season": 9, "variable": "Brunch", "value": 0.0}, {"season": 10, "variable": "Brunch", "value": 0.05405405405405406}, {"season": 11, "variable": "Brunch", "value": 0.1794871794871795}, {"season": 12, "variable": "Brunch", "value": 0.0}, {"season": 13, "variable": "Brunch", "value": 0.0}, {"season": 14, "variable": "Brunch", "value": 0.0}, {"season": 15, "variable": "Brunch", "value": 0.0}, {"season": 16, "variable": "Brunch", "value": 0.0}, {"season": 17, "variable": "Brunch", "value": 0.0}, {"season": 18, "variable": "Brunch", "value": 0.0}, {"season": 19, "variable": "Brunch", "value": 0.02564102564102564}, {"season": 20, "variable": "Brunch", "value": 0.047619047619047616}, {"season": 21, "variable": "Brunch", "value": 0.08571428571428572}, {"season": 22, "variable": "Brunch", "value": 0.0}, {"season": 23, "variable": "Brunch", "value": 0.12121212121212122}, {"season": 24, "variable": "Brunch", "value": 0.07894736842105263}, {"season": 25, "variable": "Brunch", "value": 0.025}, {"season": 26, "variable": "Brunch", "value": 0.03225806451612903}, {"season": 27, "variable": "Brunch", "value": 0.05714285714285714}, {"season": 28, "variable": "Brunch", "value": 0.038461538461538464}, {"season": 29, "variable": "Brunch", "value": 0.07017543859649122}, {"season": 30, "variable": "Brunch", "value": 0.03333333333333333}, {"season": 31, "variable": "Brunch", "value": 0.023255813953488372}, {"season": 0, "variable": "Gas Station", "value": 0.0}, {"season": 1, "variable": "Gas Station", "value": 0.0}, {"season": 2, "variable": "Gas Station", "value": 0.0}, {"season": 3, "variable": "Gas Station", "value": 0.0}, {"season": 4, "variable": "Gas Station", "value": 0.0}, {"season": 5, "variable": "Gas Station", "value": 0.0}, {"season": 6, "variable": "Gas Station", "value": 0.02564102564102564}, {"season": 7, "variable": "Gas Station", "value": 0.02564102564102564}, {"season": 8, "variable": "Gas Station", "value": 0.0}, {"season": 9, "variable": "Gas Station", "value": 0.02631578947368421}, {"season": 10, "variable": "Gas Station", "value": 0.0}, {"season": 11, "variable": "Gas Station", "value": 0.0}, {"season": 12, "variable": "Gas Station", "value": 0.0}, {"season": 13, "variable": "Gas Station", "value": 0.02702702702702703}, {"season": 14, "variable": "Gas Station", "value": 0.0}, {"season": 15, "variable": "Gas Station", "value": 0.0}, {"season": 16, "variable": "Gas Station", "value": 0.0}, {"season": 17, "variable": "Gas Station", "value": 0.0}, {"season": 18, "variable": "Gas Station", "value": 0.0}, {"season": 19, "variable": "Gas Station", "value": 0.0}, {"season": 20, "variable": "Gas Station", "value": 0.0}, {"season": 21, "variable": "Gas Station", "value": 0.0}, {"season": 22, "variable": "Gas Station", "value": 0.037037037037037035}, {"season": 23, "variable": "Gas Station", "value": 0.0}, {"season": 24, "variable": "Gas Station", "value": 0.0}, {"season": 25, "variable": "Gas Station", "value": 0.0}, {"season": 26, "variable": "Gas Station", "value": 0.0}, {"season": 27, "variable": "Gas Station", "value": 0.0}, {"season": 28, "variable": "Gas Station", "value": 0.0}, {"season": 29, "variable": "Gas Station", "value": 0.017543859649122806}, {"season": 30, "variable": "Gas Station", "value": 0.016666666666666666}, {"season": 31, "variable": "Gas Station", "value": 0.023255813953488372}, {"season": 0, "variable": "Joint", "value": 0.16666666666666666}, {"season": 1, "variable": "Joint", "value": 0.11764705882352941}, {"season": 2, "variable": "Joint", "value": 0.2564102564102564}, {"season": 3, "variable": "Joint", "value": 0.2857142857142857}, {"season": 4, "variable": "Joint", "value": 0.20512820512820512}, {"season": 5, "variable": "Joint", "value": 0.1388888888888889}, {"season": 6, "variable": "Joint", "value": 0.1794871794871795}, {"season": 7, "variable": "Joint", "value": 0.05128205128205128}, {"season": 8, "variable": "Joint", "value": 0.0967741935483871}, {"season": 9, "variable": "Joint", "value": 0.18421052631578946}, {"season": 10, "variable": "Joint", "value": 0.16216216216216217}, {"season": 11, "variable": "Joint", "value": 0.1794871794871795}, {"season": 12, "variable": "Joint", "value": 0.07692307692307693}, {"season": 13, "variable": "Joint", "value": 0.13513513513513514}, {"season": 14, "variable": "Joint", "value": 0.11764705882352941}, {"season": 15, "variable": "Joint", "value": 0.029411764705882353}, {"season": 16, "variable": "Joint", "value": 0.0}, {"season": 17, "variable": "Joint", "value": 0.06451612903225806}, {"season": 18, "variable": "Joint", "value": 0.09090909090909091}, {"season": 19, "variable": "Joint", "value": 0.05128205128205128}, {"season": 20, "variable": "Joint", "value": 0.047619047619047616}, {"season": 21, "variable": "Joint", "value": 0.02857142857142857}, {"season": 22, "variable": "Joint", "value": 0.14814814814814814}, {"season": 23, "variable": "Joint", "value": 0.12121212121212122}, {"season": 24, "variable": "Joint", "value": 0.2631578947368421}, {"season": 25, "variable": "Joint", "value": 0.2}, {"season": 26, "variable": "Joint", "value": 0.12903225806451613}, {"season": 27, "variable": "Joint", "value": 0.2571428571428571}, {"season": 28, "variable": "Joint", "value": 0.19230769230769232}, {"season": 29, "variable": "Joint", "value": 0.19298245614035087}, {"season": 30, "variable": "Joint", "value": 0.16666666666666666}, {"season": 31, "variable": "Joint", "value": 0.11627906976744186}, {"season": 0, "variable": "Mexican", "value": 0.0}, {"season": 1, "variable": "Mexican", "value": 0.0}, {"season": 2, "variable": "Mexican", "value": 0.05128205128205128}, {"season": 3, "variable": "Mexican", "value": 0.11904761904761904}, {"season": 4, "variable": "Mexican", "value": 0.07692307692307693}, {"season": 5, "variable": "Mexican", "value": 0.08333333333333333}, {"season": 6, "variable": "Mexican", "value": 0.07692307692307693}, {"season": 7, "variable": "Mexican", "value": 0.07692307692307693}, {"season": 8, "variable": "Mexican", "value": 0.06451612903225806}, {"season": 9, "variable": "Mexican", "value": 0.02631578947368421}, {"season": 10, "variable": "Mexican", "value": 0.05405405405405406}, {"season": 11, "variable": "Mexican", "value": 0.0}, {"season": 12, "variable": "Mexican", "value": 0.05128205128205128}, {"season": 13, "variable": "Mexican", "value": 0.08108108108108109}, {"season": 14, "variable": "Mexican", "value": 0.029411764705882353}, {"season": 15, "variable": "Mexican", "value": 0.058823529411764705}, {"season": 16, "variable": "Mexican", "value": 0.0}, {"season": 17, "variable": "Mexican", "value": 0.03225806451612903}, {"season": 18, "variable": "Mexican", "value": 0.06060606060606061}, {"season": 19, "variable": "Mexican", "value": 0.05128205128205128}, {"season": 20, "variable": "Mexican", "value": 0.09523809523809523}, {"season": 21, "variable": "Mexican", "value": 0.05714285714285714}, {"season": 22, "variable": "Mexican", "value": 0.1111111111111111}, {"season": 23, "variable": "Mexican", "value": 0.030303030303030304}, {"season": 24, "variable": "Mexican", "value": 0.05263157894736842}, {"season": 25, "variable": "Mexican", "value": 0.025}, {"season": 26, "variable": "Mexican", "value": 0.06451612903225806}, {"season": 27, "variable": "Mexican", "value": 0.11428571428571428}, {"season": 28, "variable": "Mexican", "value": 0.019230769230769232}, {"season": 29, "variable": "Mexican", "value": 0.08771929824561403}, {"season": 30, "variable": "Mexican", "value": 0.11666666666666667}, {"season": 31, "variable": "Mexican", "value": 0.11627906976744186}, {"season": 0, "variable": "Bakery", "value": 0.16666666666666666}, {"season": 1, "variable": "Bakery", "value": 0.0}, {"season": 2, "variable": "Bakery", "value": 0.0}, {"season": 3, "variable": "Bakery", "value": 0.0}, {"season": 4, "variable": "Bakery", "value": 0.0}, {"season": 5, "variable": "Bakery", "value": 0.0}, {"season": 6, "variable": "Bakery", "value": 0.07692307692307693}, {"season": 7, "variable": "Bakery", "value": 0.02564102564102564}, {"season": 8, "variable": "Bakery", "value": 0.03225806451612903}, {"season": 9, "variable": "Bakery", "value": 0.0}, {"season": 10, "variable": "Bakery", "value": 0.08108108108108109}, {"season": 11, "variable": "Bakery", "value": 0.05128205128205128}, {"season": 12, "variable": "Bakery", "value": 0.0}, {"season": 13, "variable": "Bakery", "value": 0.02702702702702703}, {"season": 14, "variable": "Bakery", "value": 0.0}, {"season": 15, "variable": "Bakery", "value": 0.0}, {"season": 16, "variable": "Bakery", "value": 0.0}, {"season": 17, "variable": "Bakery", "value": 0.0}, {"season": 18, "variable": "Bakery", "value": 0.0}, {"season": 19, "variable": "Bakery", "value": 0.0}, {"season": 20, "variable": "Bakery", "value": 0.0}, {"season": 21, "variable": "Bakery", "value": 0.02857142857142857}, {"season": 22, "variable": "Bakery", "value": 0.07407407407407407}, {"season": 23, "variable": "Bakery", "value": 0.030303030303030304}, {"season": 24, "variable": "Bakery", "value": 0.02631578947368421}, {"season": 25, "variable": "Bakery", "value": 0.05}, {"season": 26, "variable": "Bakery", "value": 0.03225806451612903}, {"season": 27, "variable": "Bakery", "value": 0.0}, {"season": 28, "variable": "Bakery", "value": 0.019230769230769232}, {"season": 29, "variable": "Bakery", "value": 0.03508771929824561}, {"season": 30, "variable": "Bakery", "value": 0.03333333333333333}, {"season": 31, "variable": "Bakery", "value": 0.023255813953488372}]}}, {"mode": "vega-lite"});
</script>




```python

```


```python

```


```python

```


```python
# charts = []
# cols=['diner','driveins','dives','bars','pubs','bbq','brunch','gas station']
# for i in range(len(cols)):
#     charts.append(alt.Chart(W).mark_line().encode(
#     alt.X('Season'),
#     alt.Y(cols[i],title='Percent of Season',scale=alt.Scale(domain=(5, 20)))))
# alt.vconcat(*charts)
```


```python
# base=alt.Chart(WL).mark_line().encode(
#     x='season:Q',
#     y=alt.Y('value:Q',title='Proportion of Season'),
# ).properties(
#     width=240,
#     height=240
# ).facet(
#     facet='variable:N',
#     columns=4
# )
```

The major takeaway is that diners were by far the most common in early seasons, but have dropped in proportion since then. We also see that at this point, the types of restaurants visited are more varied.

However, before I declare "Guy Fieri is a liar!", I do want to point out that just because he doesn't visit many drive-ins, there are two things to keep in mind:

1. There aren't many drive-ins period
2. Guy Fieri might have been to more drive-ins than most other Food Network shows, such that if you see a drive-in on TV, it's most likely DDD.

To that end, I tried out this analysis on another show, Michael Symon's "Burgers, Brew, and 'Que", which not only should have a high overlap on types of restaurants visited, but clearly took inspiration from Guy Fieri on its title.


```python
# base = alt.Chart(WL).encode(
#     x='season:Q',
#     y='value:Q',
# )

# c = base.mark_line()

# alt.layer(c).facet('variable:N',columns=4,header=alt.Header(labelFontSize=20))
```


```python
with open('symon.json') as json_file:
    data = json.load(json_file)
    
names=[]
diners=[]
driveins=[]
dives=[]
cafes=[]
bars=[]
bbq=[]
pubs=[]
brunch=[]
gas=[]
joint=[]
mex=[]
bake=[]
for r in data:
    if r['name']:
        if r['desc']:
            names.append(r['name'])
            if re.search(" dive | divey | dive-y ",r['desc'].lower()):
                dives.append(1)
            else:
                dives.append(0)
            if re.search("diner|diners",r['desc'].lower()):
                diners.append(1)
            else:
                diners.append(0)
            if re.search("driveins|drive-ins|drive ins|drivein|drive-in|drive in",r['desc'].lower()):
                driveins.append(1)
            else:
                driveins.append(0)

            if re.search(" cafe| cafes",r['desc'].lower()):
                cafes.append(1)
            else:
                cafes.append(0)

            if re.search(" barbecue| barbeque| bbq| bbqs| barbeques |barbecue",r['desc'].lower()):
                bbq.append(1)
            else:
                bbq.append(0)

            if re.search(" bar| bars",r['desc'].lower()):
                bars.append(1)
            else:
                bars.append(0)

            if re.search(" pub| pubs| gastropub| gastropubs",r['desc'].lower()):
                pubs.append(1)
            else:
                pubs.append(0)
            if re.search(" brunch| brunch spot",r['desc'].lower()):
                brunch.append(1)
            else:
                brunch.append(0)

            if re.search(" joint| joints",r['desc'].lower()):
                joint.append(1)
            else:
                joint.append(0)        
            if re.search(" gas station| gas stations| pump station| pump stations| fuel station| fuel stations",r['desc'].lower()):
                gas.append(1)
            else:
                gas.append(0)

            if re.search(" mexican| taqueria",r['desc'].lower()):
                mex.append(1)
            else:
                mex.append(0)

            if re.search(" bakery",r['desc'].lower()):
                bake.append(1)
            else:
                bake.append(0)
wordlist2=pd.DataFrame(list(zip(names,diners,driveins,dives,cafes,bbq,bars,pubs,brunch,gas,joint,mex,bake)),columns=['name','diner','driveins','dives','cafes','bbq','bars','pubs','brunch','gas station','joint','mexican','bakery'])
```


```python

```


```python
Q2=wordlist2.mean(axis=0)[1:15].copy().reset_index()
Q2.columns=['Category','Proportion']
Q2.style.hide_index()
```




<style  type="text/css" >
</style><table id="T_f98c9dca_6399_11eb_9154_e03f491329ca" ><thead>    <tr>        <th class="col_heading level0 col0" >Category</th>        <th class="col_heading level0 col1" >Proportion</th>    </tr></thead><tbody>
                <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow0_col0" class="data row0 col0" >driveins</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow0_col1" class="data row0 col1" >0.000000</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow1_col0" class="data row1 col0" >dives</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow1_col1" class="data row1 col1" >0.000000</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow2_col0" class="data row2 col0" >cafes</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow2_col1" class="data row2 col1" >0.000000</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow3_col0" class="data row3 col0" >bbq</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow3_col1" class="data row3 col1" >0.140845</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow4_col0" class="data row4 col0" >bars</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow4_col1" class="data row4 col1" >0.338028</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow5_col0" class="data row5 col0" >pubs</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow5_col1" class="data row5 col1" >0.084507</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow6_col0" class="data row6 col0" >brunch</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow6_col1" class="data row6 col1" >0.014085</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow7_col0" class="data row7 col0" >gas station</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow7_col1" class="data row7 col1" >0.000000</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow8_col0" class="data row8 col0" >joint</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow8_col1" class="data row8 col1" >0.197183</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow9_col0" class="data row9 col0" >mexican</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow9_col1" class="data row9 col1" >0.028169</td>
            </tr>
            <tr>
                                <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow10_col0" class="data row10 col0" >bakery</td>
                        <td id="T_f98c9dca_6399_11eb_9154_e03f491329carow10_col1" class="data row10 col1" >0.000000</td>
            </tr>
    </tbody></table>



Thankfully, we do see that bars, bbq are pretty common words used in describing places Michal Symon goes, but drive-ins are not.

#### Considerations and Future Work

This analysis can be improved in a few ways. For one, the keywords I used to search are probably underestimating the quantity of restaurants visited (For example, the word "dive" many not show up in a restaurant's name or description"). Still, after hand-checking, I do catch a majority of what should be categorized. One way to improve categorization is linking the restaurants to databases like yelp, which would provide a wealth of information about a restaurant. For example, user reviews would give a hint as to the quality of restaurant, such as it's "diveyness." It also would provide information such as pricing, noise level, and other attributes. 

Furthermore, this could be extended out to how restaurant descriptions might vary between show. We see that Michael Symon goes to different restaurants, but it might be possible that word choice might vary between shows. Preliminary analysis shows that Burgers, Brew, and 'Que may use more poetic words like "succulent" in their descriptions. 

Ultimately, Diners, Drive-Ins, and Dives is not different from any other long-running show: While keeping their premise early on, but evolves and changes over time. And if it has a strong brand, it wouldn't make sense to change the name, would it? As long as the quality of restaurants hasn't decline (and it doesn't seem like that's the case), we can accept that Guy Fieri has broadened the scope of the show. 

