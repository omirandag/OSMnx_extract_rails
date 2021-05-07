# Metodología

## Objetivo:

Explicar el procesos metodologico efectuado para la extraccción de redes de carreteras medinte el uso de la librería OSMNx. 

## Requerimientos:

- Python 3.8 
- Miniconda3
- Jupyter lab
- OSMnx 1.0.1 
- NetworkX 
- Geopandas 
- Matplotlib

## 1. Instalación

Inicialmnete, se debe descargar [miniconda](https://docs.conda.io/en/latest/miniconda.html). Una vez instalado, se deberá acceder al promt (*Anaconda promt*) y teclear el siguiente codigo:

~~~
conda config --prepend channels conda-forge
conda create -n ox --strict-channel-priority osmnx jupyterlab
conda activate ox
python -m ipykernel install --user --name ox --display-name "OSMNX"
jupyter lab
~~~

Esto instala `OSMnx` y `JupyterLab` en un entorno conda llamado `ox`, activa el entorno, instala un kernel ipython en el entorno y luego inicia `JupyterLab`
[(Boeing, 2020)](https://stackoverflow.com/questions/59603695/osmnx-wont-open-in-juypter-notebook/62180703#62180703).

## 2. Extraer redes de calles mediante la librería OSMNx

Para poder extraer las redes de carretear mediante el uso de la librería `osmnx` es necesario importar las siguientes librerías: 
``` python 
import geopandas as gpd
import osmnx as ox
```
Aunque existen diversas formas para extraer las redes de carreteras [(Boeing, 2016)](https://geoffboeing.com/2016/11/osmnx-python-street-networks/), el presente trabajo optó por descargar dichas redes mediante el uso de un poligono predeterminado `ox.graph_from_polygon()`. Entonces, se emplearon poligonos correspondientes a las 74 zonas metropolitanas de México [(Conapo, 2010)](https://www.gob.mx/conapo/documentos/delimitacion-de-las-zonas-metropolitanas-de-mexico-2015). Los cuales fueron delimitados mediante el uso del *software* QSIG 3.10 y exportados en formato *.geojson*. 

Posteriormente, todos los archivos fueron guardados en una carpeta en la raíz de *C://* para ser leidos en *Jupyter Lab* mediante el código `gpd.read_file()`.  Cabe señalar, que para asignar la ruta los archivos *.geojson*, es importante cambiar los signos "\\" por "/". 

Ahora, una vez recopilada esa información, se procedió en *Jupyter Lab* a extraer las redes de vialidades de las Zonas Metropolitanas de México. Por lo que se usaron las siguientes lineas de código [(Boeing, 2021)](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/01-overview-osmnx.ipynb). 

``` python 
Aguascalientes = gpd.read_file("C:/Analisis_redes_ZZ_MM/JSON/ZM_Aguascalientes.geojson")
ZM_Aguascalientes = Aguascalientes["geometry"].iloc[0] 
```
El código anterior, sirvió para asignar los poligonos que se utilizarán para extraer el grafo que contiene las redes de vialidades. Cabe resaltar que OSMNx permite extraer diferentes tipos de redes [(Boeing, 2021)](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/03-graph-place-queries.ipynb).

- `drive`: obtiene calles públicas manejables (pero no carreteras de servicio).
- `drive_service`: obtiene calles transitables, incluidas las carreteras de servicio.
- `walk`: obtiene todas las calles y caminos que los peatones pueden usar (este tipo de red ignora la direccionalidad unidireccional).
- `bike`: obtiene todas las calles y caminos que los ciclistas pueden usar.
- `all`: descarga todas las calles y caminos de OSM no privados.
- `all_private`: descarga todas las calles y caminos de OSM, incluidos los de acceso privado.

Para los fines de la investigación, se consideraron las calles transitables incuidas las carreteras de servicio. En tal sentido, se usó el siguiente codigo para la obtención de grafos.

``` python 
G_AguascalientesDrive = ox.graph_from_polygon(ZM_Aguascalientes, network_type= "drive_service")
```
Finalmente, una vez descargado el *grafo* se procedió a guardarlo  mediante el siguiente código, esto permite trabajarlo en otro momento y con otro *software* como *Gephi* [(Boeing, 2021)](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/05-save-load-networks.ipynb).

``` python 
ox.save_graphml(G_Aguascalientes, "G_Aguascalientes.graphml", gephi= True)
```



