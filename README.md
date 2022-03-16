# FIFI-LS FAQ

1) **How to get the PWV from the header**

PWV stands for precipitated water vapor. This is a measurement of the quantity of water vapor present in the column of atmosphere
over SOFIA during the observation. Since the atmospheric telluric lines affects the spectra taken with the SOFIA telescope, it is 
important to quantify the PWV to correct the spectrum for the atmospheric absorption.

The header of each raw FIFI-LS file in the archive contains several keywords reporting the precipitated water vapor measurement.
Two keywords report the precipitated water vapor as measured by the "water vapor monitor" of SOFIA:
 -  WVZ_STA
 -  WVZ_END
 
These are the measurements taken at the beginning and end of acquisition of a file.
Unfortunately, these measurements are not reliable. The "water vapor monitor" is not very accurate and sometimes does not work at all.

In the latest two years we implemented a direct way to measure the water vapor by monitoring during the flight several atmospheric lines.
The result of the fits of these lines compared to atmospheric model is conserved in the keyword:
- WVZ_OBS

This value of water vapor is used for the atmospheric correction during the data reduction.
To access these values one has to read the keyword of the header.
I Python is installed on your computer, from a terminal one can execute:

```
fitsheader filename | grep WVZ_OBS
```
In a notebook:

```
from astropy.io import fits

with fits.open(file) as hdul:
   header = hdul[0].header
   print(header['WVZ_OBS'])
```
   
filename corresponds to the name of the file (and its absolute path).

2) **How to get a upper limit on detection**

The pipeline reports an error on each spectrum which is derived from the error in fitting the ramps. However, this estimate 
of error is tautological, since the ramp fitting is done by minimizing the Chi-2 of the fit. 

One can think to evaluate the error directly from the spectra. However, the spectra are obtained by smoothing the data and the error
can be underestimated. In the case that telluric features appear in the spectrum and are not perfectly corrected, this can lead to overestimate the error.

A better way to estimate the error is starting from the raw data. In the observations, the observation of a ramp at a single position of the grating is repeated several times. By collecting all the repetiotions together, a better estimate of the error can be found.

Another method which can be used in the case of point sources it is to evaluate the flux in several positions where no sources are expected.
Also in this case, anyway, only a rough estimate can be derived since the responce of the different pixels vary across the detector.
