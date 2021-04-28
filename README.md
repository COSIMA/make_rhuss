# Generate JRA-55 relative humidity forcing file

This repository contains code to create a JRA-55 relative humidity
input file from the provided specific humidity fields. This is
intended to be used when one wants to provide relative humidity as a
forcing field instead of specific humidity (e.g. to keep the relative
humidity constant when applying an air temperature perturbation).

The `make_rhuss.py` file creates the relative humidity file from the
JRA-55 specific humidity file. Currently this is setup to convert only
the RYF 1990-1991 file, but is easy to generalise.

Usage:

On an interactive job on Gadi:

```
qsub -I -X -P e14 -q express -l mem=32GB -l storage=gdata/hh5+gdata/ik11+gdata/e14
```

run the following:

```
module use /g/data3/hh5/public/modules
module load conda/analysis3-unstable

python3 make_rhuss.py
```

To use this properly the required configuration changes are:

- Use a CIC5 executable which includes the updated code
  (https://github.com/COSIMA/cice5/pull/59).

- change `qair_i` to `relh_i` in `ice/input_ice.nml`

- change `qair_ai qair_i` to `relh_ai relh_i` in `namcouple`



