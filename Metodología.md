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
[(Boeing, 2020)](https://stackoverflow.com/questions/59603695/osmnx-wont-open-in-juypter-notebook/62180703#62180703).

## 2. Extraer redes de calles mediante la librería OSMNx

~~~ python 
import pandas as pd
import geopandas as gpd
import osmnx as ox
import  networkx as nx
import matplotlib.pyplot as plt

~~~




