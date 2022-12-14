# KoopDV: Geospatial and temporal predictions of seismic velocities using Koopman methods

Interpolate and extrapolate in time and space the dv/v measurements to create maps of changes in seismic velocities. Forecast and hindcase dv/v when the sensors are not (yet) running.

Compare two main approaches:
* Koopman Forecast (optimization on the global time series)
* Recurrent Neural Networks (optimization on local short-term time series)


The perturbation in seismic velocities are proportional to shallow strains of Earth materials. Strains can also be obtained using surface displacements with remote sensing (GPS). Several factors contribute to altering seismic velocities/strains: changes in air temperature, precipitations (subsurface and as surface loads), earthquake damage, long term tectonic loading.


# Data
* **dv/v**: Data comes from Clements and Denolle (202?) and can be found as a zip file [here](https://www.dropbox.com/s/tz8e6675ikpinqg/DVV-90-DAY-2.0-4.0.zip?dl=1). They are obtained using 20 years of data from the Southern California Seismic Network, the Northern California Seismic Network, and temporary stations/recordings collected on the IRIS-DMC. They are obtained from 2-4Hz single-station ambient noise cross correlations, which gives an approximate perturbation in shear wavespeed of the upper 200m of the Earth crust. The data comes with a daily resolution. Data is characterized by an annual variability (seasonal weather) and a subdecadal (6-7 year) variability controled by El Nino. Data can be gappy. Data does not start and end at the same time depending on station availability.

* **Seismic stations locations**: the file ``./data/CAstations.csv`` includes lat, long, elevation. 

* **Additional data that could be used to inform the forcast:**

    - **Weather data**: can be obtained using [PRISM Climate data](https://prism.oregonstate.edu/). Tim Clements wrote a Julia [script](./get_prism.jl) to collet the data into a netcdf file, copied in this repos.
    - **GPS data**: download daily positions using the [notebook](./get_gps.ipynb). This will save CSV files with north, east, vertical positions for each stations calculated in the [Nevada Geodetic Lab](http://geodesy.unr.edu/). GPS data is position/velocity. UW colleague Brendan Crowell has scripts to convert this to strain, which should be more similar, in theory, to the dv/v measurements.
    - **GPS station locations**: is the GPS site lat-long in a csv file here.
    - Other attributes that may play a role: rock type, average shear wave velocity.


# Installation
Create a conda environment:

``conda env create -f environment.yml``

``conda activate koopdv``

The dependencies are mostly scipy, torch, numpy, and the DPK module from https://github.com/AlexTMallen/dpk. To get a sense on how to use the core functions of DPK, check the source codes here:
* ``model_object``: https://github.com/AlexTMallen/dpk/blob/main/dpk/model_objs.py
* ``koopman_probabilistic``: https://github.com/AlexTMallen/dpk/blob/main/dpk/koopman_probabilistic.py

# Initial tests

koop_dv.ipynb is the initial notebook to make a most basic forecast without train/val/test split to get started.
get_gps.ipnyb is the script to download the GPS time series and put them into a CSV file. Same forecast could be done using these time series! Marine seems to be having bandwidth issues, but will download all of the data offline and upload it on dropbox.


# Initial goals

1 - Perform forecast using DPK and using RNNS on individual time series. Compare both.

2- Perform geospatial forecast using DPK by adding geospatial attribute to the DPK neural networks. 

2 - Improve temporal forecast, hindcast, gap filling using DPK or LSTMs/GRUs at individual sites

3 - Include other factors (temperature, precipitation), to inform temporal forecast/hindcast/gap filling (additional data set that should help the fit)

4 - Include GPS time series (or their derivative strains) to further inform the geospatial prediction


