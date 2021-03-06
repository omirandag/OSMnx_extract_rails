# Metodología

## Objetivo: Explicar el proceso de extracción de redes de carreteras mediante la librería *OSMnx*. 
## Requerimientos:

- QGIS 3.10
- Miniconda3
  - Python 3.8 
  - Jupyter lab
  - OSMnx 1.0.1 
  - Geopandas 

## 1. Instalación

El proceso de obtención de carreteras, requirió de la descarga e instalacion de dos *software*. El primero fue [QGIS  3.10](https://qgis.org/es/site/forusers/download.html) que es una aplicación de Sistemas de Información Geografía (SIG) de código abierto que permite la manipulación y análisis de información espacial. Su instalación fue muy sencilla pues solo se necesitó descargar el *instalador autónomo para QGIS* y ejecutar la aplicación.  

El segundo fue [miniconda](https://docs.conda.io/en/latest/miniconda.html) que consiste en un gestor de paquetes y un sistema de gestión de entornos de código abierto [(Anaconda, Inc., 2017)](https://docs.conda.io/projects/conda/en/latest/). Una vez instalado, se accedió al promt (*Anaconda promt*) y tecleó el siguiente codigo:

~~~
conda config --prepend channels conda-forge
conda create -n ox --strict-channel-priority osmnx jupyterlab
conda activate ox
python -m ipykernel install --user --name ox --display-name "OSMNX"
jupyter lab
~~~

Esto instala *OSMnx* y *Jupyter Lab* en un entorno conda llamado `ox`, activa el entorno, instala un kernel ipython y luego inicia `Jupyter lab`
[(Boeing, 2020)](https://stackoverflow.com/questions/59603695/osmnx-wont-open-in-juypter-notebook/62180703#62180703).

## 2. Extraer redes de calles mediante la librería OSMnx

Aunque existen diversas formas para extraer las redes de carreteras [(Boeing, 2016)](https://geoffboeing.com/2016/11/osmnx-python-street-networks/), el presente trabajo optó por descargar dichas redes mediante el uso de un poligono predeterminado `ox.graph_from_polygon()`. Entonces, se usó información correspondiente a las 74 zonas metropolitanas de México [(Conapo, 2015)](https://www.gob.mx/conapo/documentos/delimitacion-de-las-zonas-metropolitanas-de-mexico-2015). Las cuales fueron delimitadas mediante el uso del *software* QGIS 3.10 y exportadas en formato *.geojson*. 

La información de los municipios que conforman a las zonas metropolitanas, fue obtenida del geoportal de [Conabio](http://www.conabio.gob.mx/informacion/gis/). Los metadatos se descargaron en formato *.shp*  a escala 1:250000 y correspondieron al año 2019. Es importante considerar que la información deberá ser **descargada en coordenadas geográficas (WGS84)**, de lo contrario no será procesada en la librería *OSMnx* (Imagen 1).  

**Imagen 1**  

<img src="https://github.com/omirandag/OSMnx_extract_rails/blob/main/Imagenes/CONABIO.png"> Fuente: [(Conabio, 2021)](http://www.conabio.gob.mx/informacion/gis/).

Debido a que el archivo *.shp* fue descargado con todos los municipios de la república mexicana, se realizó un filtrado de la informacion para obtener los municipios que conforman a las zonas metropolitanas de México (Imagen 2). 

 **Imagen 2**

<img src = "https://github.com/omirandag/OSMnx_extract_rails/blob/main/Imagenes/ZZMM.png"> Fuente: Elaboración propia con base en QGIS .

Una vez delimitados los municipios, se generó un geoproceso para disolver los limites municipales y obtener los perimetros de cada una de las 74 zonas metropolitanas. Esto se obtuvo en QGIS mediante la siguiente ruta: Barra de herramientas -> Vectorial -> Herramientas de geoproceso -> Disolver (Imagen 3).

**Imagen 3**

<img src = "https://github.com/omirandag/OSMnx_extract_rails/blob/main/Imagenes/DISOLVER.png"> Fuente: QGIS.

Para completar el proceso, fue necesario asiganar una capa de entrada correspondiente a la zona metropolitana y se guardó como archivo *.geojson* (Imagen 4 y 5). 

**Imagen 4**

<img src = "https://github.com/omirandag/OSMnx_extract_rails/blob/main/Imagenes/DISOLVER2.png"> Fuente: QGIS.

**Imagen 5**

<img src = "https://github.com/omirandag/OSMnx_extract_rails/blob/main/Imagenes/GEOJSON.png"> Fuente: QGIS.

Finalmente, los archivos fueron guardados en una carpeta en *C:/* con el proposito de ser leidos en *Jupyter Lab* mediante el código `gpd.read_file()`. Una vez recopilada esa información, se usó *Jupyter Lab* para extraer las redes de las zonas metropolitanas de México. El primer paso consistió en importar las siguientes librerías: 

``` python 
import geopandas as gpd
import osmnx as ox
```

Posteriormente, se procedió a leer los archivos mediante las siguientes líneas de código [(Boeing, 2021a)](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/01-overview-osmnx.ipynb). Cabe señalar, que para asignar la ruta de los archivos *.geojson*, fue importante cambiar los signos "\\" por "/".

``` python 
Aguascalientes = gpd.read_file("C:/Analisis_redes_ZZ_MM/JSON/ZM_Aguascalientes.geojson")
ZM_Aguascalientes = Aguascalientes["geometry"].iloc[0] 
```

El código anterior, sirvió para asignar los poligonos que se utilizarán para extraer el *grafo* que contiene las redes de vialidades. Cabe resaltar, que OSMNx permite extraer diferentes tipos de redes [(Boeing, 2021b)](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/03-graph-place-queries.ipynb).

- `drive`: obtiene calles públicas manejables (pero no carreteras de servicio).
- `drive_service`: obtiene calles transitables, incluidas las carreteras de servicio.
- `walk`: obtiene todas las calles y caminos que los peatones pueden usar (este tipo de red ignora la direccionalidad unidireccional).
- `bike`: obtiene todas las calles y caminos que los ciclistas pueden usar.
- `all`: descarga todas las calles y caminos de OSM no privados.
- `all_private`: descarga todas las calles y caminos de OSM, incluidos los de acceso privado.

Para los fines de la investigación, se consideraron las calles transitables incuidas las carreteras de servicio. En tal sentido, se usó el siguiente código para la obtención del *grafo*.

``` python 
G_AguascalientesDrive = ox.graph_from_polygon(ZM_Aguascalientes, network_type= "drive_service")
```
Finalmente, una vez descargado el *grafo* se procedió a guardarlo mediante el siguiente código, esto permite trabajarlo en otro momento y con otro *software* como *Gephi* [(Boeing, 2021c)](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/05-save-load-networks.ipynb).

``` python 
ox.save_graphml(G_Aguascalientes, "G_Aguascalientes.graphml", gephi= True)
```
## Referencias
- Anaconda, Inc. (2017). *Conda*. Recuperado de [https://docs.conda.io/projects/conda/en/latest/](https://docs.conda.io/projects/conda/en/latest/).
- Boeing, Geoff (1 de mayo de 2021a). *OSMnx overview: querying, simplifying, visualizing, saving*. Recuperado de [https://github.com/gboeing/osmnx-examples/blob/main/notebooks/01-overview-osmnx.ipynb](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/01-overview-osmnx.ipynb).
- Boeing, Geoff (19 de marzo de 2021b). *Use OSMnx to get street networks by place name*. Recuperado de [https://github.com/gboeing/osmnx-examples/blob/main/notebooks/03-graph-place-queries.ipynb](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/03-graph-place-queries.ipynb).
- Boeing, Geoff (1 de mayo de 2021c). *Save/load street network models to/from disk*. Recuperado de [https://github.com/gboeing/osmnx-examples/blob/main/notebooks/05-save-load-networks.ipynb](https://github.com/gboeing/osmnx-examples/blob/main/notebooks/05-save-load-networks.ipynb).
- Boeing, Geoff (3 de junio de 2020). *OSMnx wont open in Juypter Notebook*. Recuperado de [https://stackoverflow.com/questions/59603695/osmnx-wont-open-in-juypter-notebook/62180703#62180703](https://stackoverflow.com/questions/59603695/osmnx-wont-open-in-juypter-notebook/62180703#62180703).
- Boeing, Geoff (1 de noviembre de 2016). *OSMnx: Python for Street Networks*. Recuperado de [https://geoffboeing.com/2016/11/osmnx-python-street-networks/](https://geoffboeing.com/2016/11/osmnx-python-street-networks/).
- Conabio (2021). *Portal de Geoinformación 2021.Sistema Nacional de Información Sobre Biodiversidad (SNIB)*. Recuperado de [http://www.conabio.gob.mx/informacion/gis/](http://www.conabio.gob.mx/informacion/gis/).
- Conapo (2015). *Delimitación de las zonas metropolitanas de México 2015*. Recuperado de [https://www.gob.mx/conapo/documentos/delimitacion-de-las-zonas-metropolitanas-de-mexico-2015](https://www.gob.mx/conapo/documentos/delimitacion-de-las-zonas-metropolitanas-de-mexico-2015).
