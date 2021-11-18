<h1> DEMIX library </h1>

The DEMIX Library that allow you to get scores for specific DEMIX tiles and DEM
You also can download DEMIX Tile associated DEM Layers like SourceMask, Height, ...

<h2> Table of contents: </h2>

0. <a href="#Installation">Installation</a><br/>
1. <a href="#demix_lib_functions"> DEMIX lib functions</a><br/>
    1.1 <a href="#Getting-Score">Getting Scores</a><br/>
    1.2 <a href="#Getting-Geotiffs">Getting DEM</a><br/>
2. <a href="#dem_and_criterions">Available DEMs and criterions</a><br/>
    2.1 <a href="#Dem">Dem list</a><br/>
    2.2 <a href="#Criterion">Criterion list</a><br/>
3. <a href="#utility">Utility functions</a><br/>
4. <a href="#Usage_example">Usage example</a><br/>

<h2 id='Installation'> Installation</h2>
To install the DEMIX library on your python environment :

```
pip install demix_lib
```



<div id='demix_lib_functions'></div>
<h1>DEMIX lib functions</h1>
<h2 id='#Getting-Score'>Getting Scores</h2>
First thing first, you can use the demix api to get directly stats from the desired DEMIX Tile and Criterion
<br/>
In order to get scores to specific dem and tile, you need to choose a criterion.
The criterion list is available <a href="#Criterion">here</a>. List of supported dems is also visible <a href="#Dem">here</a>.


```Python
import demix_lib as dl

#getting the list of implemented criterions
criterions = dl.get_criterion_list()
#getting the list of supported dems
dems = dl.get_supported_dem_list()

#defining the wanted DEMIX Tile name 
demix_tile_name = "N35YE014F"

#getting the score of each dem, for the criterion 
for dem in  dems:
    for criterion in criterions:
        print(dl.get_score(demix_tile_name=demix_tile_name, dem=dem, criterion=criterion))
```

<div></div>
<H2 id='Getting-Geotiffs'>Getting DEM</H2>
To go further :
You can always use your own criterions by downloading the wanted layer on your DEMIX tile and apply custom code to it.
<br/>To download a DEM layer for a specific DEMIX Tile :

```Python
import demix_lib as dl
import matplotlib as plt #we use matplotlib to visualise the downloaded layer
from matplotlib import cm #we use cm to make a legend/colormap
from matplotlib.lines import Line2D #to add colored line in the legend

#defining wanted tile
demix_tile_name = "N35YE014F"
#asking for the SourceMask layer for the CopDEM_GLO-30 dem and the tile N64ZW019C
response = dl.download_layer(demix_tile_name=demix_tile_name,dem="CopDEM_GLO-30",layer="SourceMask")

#creating legend for the plot
legend_handle = list(map(int, response['values'].keys()))
legend_label = list(response['values'].values())
#defining the colormap for the layer (the layer has 6 values)
color_map = cm.get_cmap('rainbow',6)
#we use plt to look at the data
plt.imshow(response["data"], interpolation='none', cmap=color_map, vmin=0, vmax=6)
#creating legend values using the color map and the values stored
custom_handles = []
for value in legend_handle:
    custom_handles.append(Line2D([0], [0], color=color_map(value), lw=4))
plt.legend( custom_handles,legend_label)
#show the layer with custom legend and color map
plt.show()
```

<H2 id='Getting-Geotiffs'>Utility functions</H2>
The DEMIX lib give you some utility functions that allow you to get or print informations about currently implemented criterions, available DEMs, layers...

```python
import demix_lib as dl

#get or show the layers that you can ask in a download_layer function
layer_list = dl.get_layer_list()
dl.print_layer_list()
#get or show the full dem list
dem_list = dl.get_dem_list()
dl.print_dem_list()
#get or show the supported dem list
supported_dem_list = dl.get_supported_dem_list()
dl.print_supported_dem_list()
#get or show the implemented criterion list
criterion_list = dl.get_criterion_list()
dl.print_criterion_list()


```


<h2 id='dem_and_criterions'>Available DEMs and criterions</h2>
<h3 id='Dem'>DEMs list</h3>

| DEM name | supported |
| :-------------: | :-------------: |
| ALOS World 3D | <span style="color:red">no</span> |
| ASTER GDEM | <span style="color:red">no</span> |
| CopDEM GLO-30 | <span style="color:green">yes</span> |
| NASADEM | <span style="color:red">no</span> |
| SRTMGL1 | <span style="color:green">yes</span> |

<h3 id='Criterion'>Criterion list</h3>
    
| Criterion name | Criterion id | version | Date | Category | Target | Description | Requirement |
| :---: | :---: | :---: | :---: | :---: | :---: | :--- | :--- |
| Product fractional cover | A01 |  0.1 | 20211103 | A-completeness | <span style="color:green">All</span> |  indique le pourcentage dela tuile demix qui est ouverte par le produit, area couverte divisée par area de la tuile |  Area computation on an ellipsoïd and simple |
| Valid data fraction | A02 |  0.1 | 20211103 | A-completeness | <span style="color:green">All</span> |  Area d'une demix tile qui est couverte par une data valide , on fait la somme de chaque cellule avec des données valide et on calcule l'aire puis on fait ce total divisé par l'aire totale de la tuile demix |  metadata no/void.un masque par pixel doit indiqué si ils sont extrapole, infilled, ou masqué sinon c'est recalé |
| Primary data | A03 |  0.1 | 20211103 | A-completeness | <span style="color:green">All</span> |  indique l'aire dans une demix tile couverte par une valid data provenant de la source principale |  metadata no/void.un masque par pixel doit indiqué si ils sont extrapole, infilled, ou masqué sinon c'est recalé |
| Valid land fraction | A04 |  0.1 | 20211103 | A-completeness | <span style="color:green">All</span> |   indique l'aire dans une demix tile couverte par une valid data provenant de la source principale |  metadata no/void.un masque par pixel doit indiqué si ils sont extrapole, infilled, ou masqué sinon c'est recalé |
| Primary land fraction | A05 |  0.1 | 20211103 | A-completeness | <span style="color:green">All</span> |  indique  aire couverte par une donnée primaire en land |  metadata no/void.un masque par pixel doit indiqué si ils sont extrapole, infilled, ou masqué sinon c'est recalé |

<h3 id='Layers'>Layer list</h3>

| Layer name |
| :-------------: |
| Height |
| validMask | 
| SourceMask |
| landWaterMask |