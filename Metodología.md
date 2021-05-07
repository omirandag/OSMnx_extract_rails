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

## Instalación

En primer lugar debes descargar [miniconda](https://docs.conda.io/en/latest/miniconda.html). Una vez instalado, deberás acceder al promt (*Anaconda promt*) y teclear el siguiente codigo

~~~
conda config --prepend channels conda-forge
conda create -n ox --strict-channel-priority osmnx 
~~~

Si se quiere instalar ´jupyterlab´ solo agrega el nombre después de ´osmnx´[Boeing, 2021](https://osmnx.readthedocs.io/en/stable/)

~~~
python -m ipykernel install --user --name ox --display-name "OSMNX"
~~~




