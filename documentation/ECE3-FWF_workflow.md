This documents the steps in the workflow 

[[workflow_overview.excalidraw]]

## 1. preparations

### 1.1 code changes and  recompilation
- NEMO changes in `CONFIG/ORCA1_LIM3/MY_SOURCE`
	- `sbccpl.F90`
	- `sbcfwf.F90` new freshwater forcing code
	- `sbcmod.F90` overarching surface boundary condition module, needs to call new routine
	- `sbcrnf.F90`
- namelist amendment for additional forcing files
	- in case of using the `sbcfwf.F90` module, create fwfnamlist with information about netcdf filename, included variable names (see 0.3.1), and forcing frequency (monthly vs annual)
	- in case of using the `sbcisf.F90` module
- compilation via `compile.sh`
- for ECE3, these changes are on the EC-EArth [svn](svn.md) branch https://svn.ec-earth.org/ecearth3/branches/development/2023/r9409-cmip6-fwf-knmi

### 1.2 possibly modify runoff mapper files
- possibly remove Greenland and or Antarctica runoff files if the default freshwater fluxes (excess snowfall rerouted to ocean) are completely replaced with freshwater forcing

### 1.3 forcing files
#### 1.3.1 `fwf.F90`: surface solid and liquid forcing
- create yearly .nc files with monthly resolution
- contans both a liquid and sold component
#### 1.3.2 `sbcisf.F90`: ice shelf module (Mathiot et al. 2017) forcing
- this is relevant if the ISF module is used 
	- change the namelist options accordingly
- use Pierre Mathiot's depth files and relativefreshwater amounts per gridcell
- theses are on eORCA grids, so I regridded them to ORCA1 which is waht ECE3 uses
- or create own depth max and min files

### 1.4 restart files
- located on Philippe's partition
- see Döscher et al. Fig. 3 for which run number corresponds to which piControl year
- linked into run directory by the wrapper


## 2. running the model
### 2.1 custom wrapper script
- takes as input:
	- initial conditions (see Döscher et al piControl picture for the assigned integer numbers)
	- type of experiment/forcing

### 2.2 monitoring and restarting run
- I made a jupyter notebook for monitoring the runs


## 3. handling ECE3 output
- in the monitoring notebook, the back/cmorization can be started as well

### 3.1 cmorize
- needs to be done per year, takes forever
- because the date of creation is part of the name/folder structure, there is a renaming script available; I don't have it yet, Philippe has it

### 3.2 backup to raw output to ECFS tape storage
- [ECE backup to ECFS](<ECFS.md>)
- using `hpcperm/ece3-raw-backup`

### 3.3 upload to SOFIAMIP archive

