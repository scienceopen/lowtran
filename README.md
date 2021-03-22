# Lowtran in Python

[![Zenodo DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.213475.svg)](https://doi.org/10.5281/zenodo.213475)
[![CDash](./tests/cdash.png)](https://my.cdash.org/index.php?project=lowtran)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/space-physics/lowtran.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/space-physics/lowtran/context:python)
![Actions Status](https://github.com/space-physics/lowtran/workflows/ci_unix/badge.svg)
![Actions Status](https://github.com/space-physics/lowtran/workflows/ci_windows/badge.svg)
[![PyPi version](https://img.shields.io/pypi/pyversions/lowtran.svg)](https://pypi.python.org/pypi/lowtran)
[![PyPi Download stats](http://pepy.tech/badge/lowtran)](http://pepy.tech/project/lowtran)

LOWTRAN7 atmospheric absorption extinction model.
Updated by Michael Hirsch to be platform independent and easily accessible from Python &ge; 3.6 and
[Matlab](#matlab).

The main LOWTRAN program has been made accessible from Python by using direct memory transfers instead of the cumbersome and error-prone process of writing/reading text files.
`xarray.Dataset` high-performance, simple N-D array data is passed out, with appropriate metadata.

## Gallery

See below for how to make these examples.

![Lowtran7 absorption](./gfx/lowtran.png)

## Install

You will need a Fortran compiler.
`gfortran` is one suitable compiler.
We use `f2py` (part of `numpy`) to seamlessly use Fortran libraries from Python by special compilation of the Fortran library with auto-generated shim code.

1. If a Fortran compiler is not already installed, install Gfortran:

   * Linux: `apt install gfortran`
   * Mac: `brew install gcc`
   * [Windows](https://www.scivision.dev/windows-gcc-gfortran-cmake-make-install/) use MSYS2 or MinGW to get Gfortran.
     Windows only: from Powershell:

   ```posh
   echo "[build]`ncompiler=mingw32" | Out-File -Encoding ASCII ~/pydistutils.cfg
   ```

   Note: Cygwin is essentially obsolete due to Windows Subsystem for Linux. Cygwin is broken for Numpy and Gfortran and general.

2. Install Python Lowtran code

   ```sh
   pip install -e .
   ```

## Examples

In these examples, you can write to HDF5 with the `-o` option.

We present examples of:

* ground-to-space transmittance:
  [TransmittanceGround2Space.py](./TransmittanceGround2Space.py)

  ![Lowtran Transmission](./doc/txgnd2space.png)
* sun-to-observer scattered radiance (why the sky is blue):
  [ScatterRadiance.py](./ScatterRadiance.py)

  ![Lowtran Scatter Radiance](./gfx/whyskyisblue.png)
* sun-to-observer irradiance:
  [SolarIrradiance.py](./SolarIrradiance.py)

  ![Lowtran Solar Irradiance](./gfx/irradiance.png)
* observer-to-observer solar single-scattering solar radiance (up-going) with custom Pressure, Temperature and partial pressure for 12 species:
  [UserDataHorizontalRadiance.py](./UserDataHorizontalRadiance.py)
  ![Lowtran Solar Irradiance](./gfx/thermalradiance.png)
* observer-to-observer transmittance with custom Pressure, Temperature and partial pressure for 12 species:
  [UserDataHorizontalTransmittance.py](./UserDataHorizontalTransmittance.py)
* observer-to-observer transmittance:
  [HorizontalTransmittance.py](./HorizontalTransmittance.py)

  ![Lowtran Horizontal Path transmittance](./gfx/horizcompare.png)

### Matlab

Matlab users can seamlessly access Python modules, as demonstrated in
[RunLowtran.m](./matlab/RunLowtran.m) and
[lowtran_transmission.m](./matlab/lowtran_transmission.m).

Here's what's you'll need:

1. [Setup Python &harr; Matlab interface](https://www.scivision.dev/matlab-python-user-module-import/).
2. Install Lowtran in Python as at the top of this Readme.
3. From Matlab, verify everything is working by from the `lowtran/` directory:

   ```matlab
   runtests('tests')
   ```

## Notes

LOWTRAN7
[User manual](http://www.dtic.mil/dtic/tr/fulltext/u2/a206773.pdf)
Refer to this to understand what parameters are set to default.
Currently I don't have any aerosols enabled for example, though it's possible to add them into the code.

Right now a lot of configuration features aren't implemented, please request those you want.

### Reference

* Original 1994 Lowtran7 [Code](http://www1.ncdc.noaa.gov/pub/data/software/lowtran/)
* `LOWFIL` program in reference/lowtran7.10.f was not connected as we had previously implemented a filter function directly in  Python.
* `LOWSCAN` spectral sampling (scanning) program in `reference/lowtran7.13.f` was not connected as we had no need for coarser spectral resolution.
