# Backing up ECE raw output data to ECFS tape storage
- read this [ECWMF ECFS user documentation](https://confluence.ecmwf.int/display/UDOC/ECFS+user+documentation)
- TODO (potentially): in contrast to what `ece3-raw-backup` is doing, the number of files should be minimized, i.e., tarring all files of one year together; now retrieval is very slow

## general strategy
- raw model out and restart files to ECFS tape storage
- the published, cmorized (CMIP6) data is usually on ESGF
- in case of SOFIAMIP runs, I will upload the cmorized data to the SOFIMAIP ftp server and keep a local copy

## Philippe's code 
- [ece3-raw-backup github repo](https://github.com/plesager/ece3-raw-backup)
- cloned to `$HPCPERM`, i.e. `/ec/res4/hpcperm/nkaj`
- includes README with most things described
	- set user-specific folders in `config.cfg`
- on the hpc2020, can only submit 60 sbatch jobs at the same time, 30 can run at the same time

### copy data to ECFS: `backup_ece.sh`
1. set experiment name: e.g. `exp=sf02`
- to run for single year: `sbatch backup_ece3.sh $exp $i` where `i` is the leg number; takes about >10 minutes
- to run for multiple years, use single line
  ```for i in {1..50}; do mess=${exp}-$(printf %03d $i); sbatch -o log/${mess}.out -J $mess backup_ece3.sh ${exp} $i; done```

### get data from ECFS: `retrieve_ece.sh`


### `check_bckp.sh`
- use `-r` option to check file numbers on ECFS


#### SOFIAMIP runs status
NEMO output amount now 2023-05-31 15:00 all done!?
