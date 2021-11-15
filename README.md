# Generate JRA55-do relative humidity forcing file

This repository contains code to create a JRA55-do relative humidity
input file from the provided specific humidity fields. This is
intended to be used when one wants to provide relative humidity as a
forcing field instead of specific humidity (e.g. to keep the relative
humidity constant when applying an air temperature perturbation).
See discussion here: https://github.com/COSIMA/cice5/issues/57

The `make_rhuss.py` file creates the relative humidity file from the
JRA55-do specific humidity file. Currently this is setup to convert only
the RYF 1990-1991 file, but is easy to generalise.

The "main" branch will work with JRA55 v1.4 data. The "jra55_v1-3"
branch will work with JRA55 v1.3 data.

Usage:

On an interactive job on Gadi:

```
qsub -I -X -P e14 -q express -l mem=32GB -l storage=gdata/hh5+gdata/ik11+gdata/e14+gdata/ua8 -l wd
```

run the following:

```
module use /g/data3/hh5/public/modules
module load conda/analysis3-21.04

python3 make_rhuss.py
```

To use this properly the required configuration changes are:

- Use a CICE5 executable which includes the updated code
  (https://github.com/COSIMA/cice5/pull/59).

- change `qair_i` to `relh_i` in `ice/input_ice.nml`

- change `qair_ai qair_i` to `relh_ai relh_i` in `namcouple`

- in atmosphere/forcing.json change:
```
      "filename": "INPUT/RYF.huss.1990_1991.nc",
      "fieldname": "huss",
      "cname": "qair_ai"
```
  to
```
      "filename": "INPUT/RYF.rhuss.1990_1991.nc",
      "fieldname": "rhuss",
      "cname": "relh_ai"
```


