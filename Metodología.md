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

En primer lugar debes descargar [miniconda](https://docs.conda.io/en/latest/miniconda.html). Una vez instalado, deberás acceder al promt (*Anaconda promt*) y teclear el siguiente codigo:

~~~
conda config --prepend channels conda-forge
conda create -n ox --strict-channel-priority osmnx jupyterlab
conda activate ox
python -m ipykernel install --user --name ox --display-name "OSMNX"
jupyter lab
~~~

Esto instala `OSMnx` y `JupyterLab` en un entorno conda llamado `ox`, activa el entorno, instala un kernel ipython en el entorno y luego inicia `JupyterLab`.
[(Boeing, 2020)](https://stackoverflow.com/questions/59603695/osmnx-wont-open-in-juypter-notebook/62180703#62180703)

## 2. Extraer redes de calles mediante la librería OSMNx

Para poder extraer las redes de carretear mediante el uso de la librería `osmnx` es necesario importar las siguientes librerías: 
``` python 
import geopandas as gpd
import osmnx as ox
```
Aunque existen diversas formas para extraer las redes de carreteras [(Boeing, 2016)](https://geoffboeing.com/2016/11/osmnx-python-street-networks/), el presente trabajo optó por descargar dichas redes mediante el uso de un poligono predeterminado `ox.graph_from_polygon()`. Entonces, se emplearon poligonos correspondientes a las 74 zonas metropolitanas de México [(Conapo, 2010)](https://www.gob.mx/conapo/documentos/delimitacion-de-las-zonas-metropolitanas-de-mexico-2015) 
``` python 
Aguascalientes = gpd.read_file("C:/Analisis_redes_ZZ_MM/JSON/ZM_Aguascalientes.geojson")
ZM_Aguascalientes = Aguascalientes["geometry"].iloc[0] 
```


