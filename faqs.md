# FAQs

## EXES

### How do I get an absolute flux calibration of EXES data?

**A: Contact the Helpdesk**

If you are interested in the absolute flux calibration of EXES data please contact the Helpdesk. Most of the data prior to 2022 has not had an accurate absolute flux scaling of the reduced data products. Depending on when the data were taken, a more accurate scaling can be applied.

## FORCAST

### Why are the USPOT exposure times higher than before?

**A: Due to a change in the calculation (related to low thermal backgrounds)**

In Cycle 10 a correction was made to the USPOT exposure time calculation. This primarily affected estimates at 5 and 6 microns where the thermal background of the sky is lower than those of the other filters and grisms.

The instrument scientists aim to keep a consistent thermal background to avoid issues of nonlinearity, keeping same level of charge in the detector wells relative to the saturation level. Due to the relatively lower thermal background at 5 and 6 microns the chop frequency is on the lower side and the frame-rate must be at least twice as large as the chop rate plus throwaways. When using F056, F064, and/or G063 FORCAST ends up throwing away more frames due to the lower thermal background and this creates the inflated overhead values at these wavelengths. Taking this into account now provide a more realistic estimate of the observing overheads.

<!-- ### Has a COMA aberration correction been done on the data?

**A: No, but it's unlikely to be needed** -->

<!-- Due to dithering?

### Is there a ghost in my data?

**A: look at radomski talk**

s

### How many dithers do I need for photometry?

**A: 3 is fine but 5 is better**

s -->

### How can we check for memory/persistence issues?

**A: Check the residual spectrum in the raw chopped frames**

Persistence is an effect on the detector, where observing a bright source leaves a temporary impression in subsequent observations. If you suspect your data are contaminated by this effect, check the residual spectrum in the raw chopped frames of your data. If there is a lasting impression, you should be able to see it in these frames. Also, check if other observations were done between the bright object in question and the data that you are concerned about.

### Should I worry about saturating the FORCAST detector?

**A: Probably not**

Very bright objects can cause linear stripes across the array that have a very slight (few percent) affect on the flux of the target but can intercept extended regions of lower flux surrounding the object making analysis difficult. Also when dealing with very bright sources the overall sensitivity of the array can be reduced which also can be detrimental if you are trying to detect very faint sources in the same field of very bright sources. This is not a worry typically til you get in the realm of hundreds of Janskys and also trying to detect faint sources of emission.

### How do I find the on-sky angle of the FORCAST grism slit?

**A: The "SKY_ANGL" keyword**

Within the [header information](howtoaccessheader) of the data should be a "SKY_ANGL" keyword which is the long-axis position angle of the slit on the sky in degrees E of N.

### What is the FORCAST photometric uncertainty?

**A: Typically 5-15%**

The uncertainty is typically between 5-15%. To calculate this value you must add (in quadrature) the measurement of the flux calibration error ([header keyword](howtoaccessheader): ERRCALF), and the possible uncertainty on the flux model. The uncertainty is typically less variable in the short wavelength camera (SWC) and slightly more variable in the long wavelength camera (LWC) and in spectroscopy mode. For more details as well as a demonstration of the uncertainty calculation in python see the SOFIA data analysis [cookbooks](https://sofia-data-analysis-cookbooks.readthedocs.io/en/latest/).

<!-- ### How accurate is the absolute flux calibration of the FORCAST imaging data?

**A:**

look in observers' handbook and then ask Bill Vacca -->

<!-- Look for some in James Radomski's talk -->


## FIFI-LS

### How to get an upper limit on a FIFI-LS detection?

**A: By averaging multiple fits to single-position ramps**

The pipeline reports an error on each spectrum which is derived from the error in fitting the ramps. However, this estimate of error is tautological, since the ramp fitting is done by minimizing the Chi-2 of the fit.

One can think to evaluate the error directly from the spectra. However, the spectra are obtained by smoothing the data and the error can be underestimated. In the case that telluric features appear in the spectrum and are not perfectly corrected, this can lead to overestimating the error.

A better way to estimate the error is starting from the raw data. In the observations, the observation of a ramp at a single position of the grating is repeated several times. By collecting all the repetitions together, a better estimate of the error can be found.

Another method which can be used in the case of point sources it is to evaluate the flux in several positions where no sources are expected.
Also in this case, anyway, only a rough estimate can be derived since the response of the different pixels vary across the detector.

### Should I use FIFI-LS or GREAT for [CII]?

**A: FIFI-LS (in most cases)**

Unless the line you are targeting is expected to be bright (e.g. extragalactic), it will not likely be detected by GREAT. We suggest using FIFI-LS for targeting [CII] due to its faster mapping speed. The trade-off, however, is that GREAT has a much higher spectral resolution. If the spectral resolution is critical, GREAT may be the better choice. See observing documentation and SITE (https://dcs.arc.nasa.gov/proposalDevelopment/SITE/index.jsp) for more details.

### What is the beam/psf size of FIFI-LS?
**A: Typically 2.3 - 3”**

The intrinsic PSF of FIFI-LS is smaller than the PSF from the SOFIA telescope for most of the spectral range. Therefore, the telescope PSF should be the dominating factor for the effective spatial resolution of FIFI-LS on SOFIA. SOFIA's PSF size is highly wavelength dependent. For longer wavelengths, the PSF size is diffraction limited. For shorter wavelengths (<30 um) the PSF depends on diffraction, shear layer seeing, jitter, pointing accuracy, stability and drift (see https://www.worldscientific.com/doi/full/10.1142/S2251171718400111). It should also be noted that the pixel size of FIFI-LS is smaller than the PSF, meaning that the data are intentionally oversampled.

### What is the pixel size of FIFI-LS data?

**A: More than a factor of 3**

The PSF is oversampled in the FIFI-LS data. The spacing in the spatial dimensions (dx) is fixed for each channel at 1.5 arcseconds in the BLUE and 3.0 arcseconds in the RED. The projected pixel sizes are 12” in the red channel and 6” in the blue channel. These values are chosen to ensure an oversampling of the spatial FWHM by at least a factor of three.

<!-- ### What is the best strategy for observing point sources?

**A: ask Juan Luis Verbena for help on this (GREAT; SOFIA School)?** -->

## The Data and Data Analysis

### What is the boresight?

**A: The window that lets in light.**

The boresight is the rectangle region that light passes through to get to the instruments. The boresight is occasionally calibrated to ensure no loss of signal outside of this window.


### What is the best way to access the FITS header information?

**A: Using python, DS9, or other packages**

If you have astropy installed, use the following command in a terminal:

```
fitsheader filename | grep WVZ_OBS
```
or within python:

```
from astropy.io import fits

with fits.open(filename) as hdul:
   header = hdul[0].header
   print(header['WVZ_OBS'])
```

*filename* corresponds to the name of the file (and its absolute path).

### How to get the PWV from the header

**A: FITS header keyword: WVZ_OBS**

PWV stands for precipitated water vapor. This is a measurement of the quantity of water vapor present in the column of atmosphere over SOFIA during the observation. Since the atmospheric telluric lines affect the spectra taken with the SOFIA telescope, it is important to quantify the PWV to correct the spectrum for the atmospheric absorption.

The header of each raw FIFI-LS file in the archive contains several keywords reporting the precipitated water vapor measurement.

Two keywords report the precipitated water vapor as measured by the "water vapor monitor" of SOFIA:
 -  WVZ_STA
 -  WVZ_END

These are the measurements taken at the beginning and end of acquisition of a file. Unfortunately, these measurements are not reliable. The "water vapor monitor" is not very accurate and sometimes does not work at all.

In the latest two years we implemented a direct way to measure the water vapor by monitoring during the flight several atmospheric lines.
The result of the fits of these lines compared to the atmospheric model is conserved in the keyword:
- WVZ_OBS

This value of water vapor is used for the atmospheric correction during the data reduction.
To access these values one has to read the keyword of the header.

### What version of redux was used to reduce my data?

**A: FITS header keyword: PIPEVERS**

Within the reduced data products fits file there is a [header keyword](howtoaccessheader), PIPEVERS, which lists the redux version.

### What do the quality assurance comments mean, and should I be worried?

**A: Look for Known Problem Codes and obvious words like contamination, problem, or failure.**

SOFIA is aware of several common issues with the data that we denote with 4-letter codes starting with the letter O. We have a list of the Known Problems and their associated codes hosted on the SOFIA website (https://www.sofia.usra.edu/sites/default/files/USpot_DCS_DPS/Documents/DCS_Known_Issues.pdf). If you are still worried about the quality of the data after identifying any known issues, please contact the help-desk (sofia_help@sofia.usra.edu).

Additionally, be aware of Quality Assurance comments with words like: contamination, problem, failure, artifact, degradation, or issue. If the comments include words like acquisition or test, you may not have the correct file.

<!-- ### What does this file extension mean?

**A: It differs for each instrument.**

Typically, a filename follows the template Flight_IS_MOD_AOR-ID_SPECTEL_Type_FN1-FN2.fits, where:

*Flight*: SOFIA flight number </br>
*IS*: instrument identifier </br>
*SPE*: specifies the instrument mode </br>
*AOR-ID*: is the 8 digit AOR identifier for the observation </br>
*SPECTEL*: is the keywords specifying the spectral configuration </br>
*Type*: three letters identifying the product type </br>
*FN1*: is the file number corresponding to the first input file </br>
*FN2*: is the file number corresponding to the last input file </br>

Additional information on common *Types* of Level 3 & 4 data:

**FORCAST Photometry** </br>
COA: Level 2 coadded image </br>
CAL: Level 3 calibrated image </br>
MOS: Level 4 mosaicked image </br>

**FORCAST Spectroscopy** </br>
CRM: Level 3 calibrated spectrum </br>
COA: Level 3 coadded spectrum </br>
CMB: Level 3 combined spectrum </br>
SCB: Level 4 spectral cube </br>

**EXES** </br>
COA: Level 3 averaged spectrum using all frames </br>
CMD: Level 3 combined spectrum </br>
MRD: Level 3 merged spectrum </br>

**FIFI-LS** </br>
CAL: Level 3 flux calibrated data </br>
WGR: Level 3 wavelength_resampled (???) </br>
WXY: Level 4 FIFI-LS data </br>

**HAWC+** </br>
MRG: Level 2 map of merged dithers </br>
CAL: Level 3 calibrated data </br>
PMP: Level 4 map of polarization vectors

**GREAT** </br>
TMB: Level X ? -->

### How do I find out if the data products are the raw or calibrated files?

**A: You can usually use the Level of the data product; Levels 3 and 4 are fully-calibrated.**

Data products come in different levels depending on the processing that has been done. Generally the higher the level, the more calibrated for analysis.


### I can’t tell if my detection is real or noise, what can I do?

**A: Contact the help-desk**

If you have a tentative detection, consult the instrument and data handbooks to ensure the signal is outside of the expected uncertainty. Identify any known problems using the quality assurance comments within the data. If you are still unsure of your detection, contact the SOFIA helpdesk.


## The Observatory

### Where can I access the SOFIA filter curves?

**A: SVO filter service**

The Spanish Virtual Observatory (SVO) provides filter/transmission curves for many observatories including SOFIA (<http://svo2.cab.inta-csic.es/svo/theory/fps3/index.php?id=SOFIA>)

<!-- ### Who is in charge of SOFIA? (USRA, DSI, NASA, DLR)

**A: NASA and the German Aerospace Center (DLR)**

SOFIA is an 80/20 partnership between NASA and the German Aerospace Center (DLR). It is operated by the Universities Space Research Association (USRA) in partnership with the Deutsches SOFIA Institut (DSI). The SOFIA Science Center is located at NASA Ames (Mountain View, California) and the aircraft is operated out of NASA's Armstrong Flight Research Center at Edwards Air Force Base (Palmdale, California). -->

<!--
### What is the different between a series and a deployment?

**A: Series flights return to Palmdale**

A series is several days of flights with the same instrument. A deployment is typically an extended period in which the aircraft does not return to its home-base in Palmdale, CA.
 -->

## Proposing

### Am I allowed to apply for SOFIA time?

**A: Yes**

Yes, everyone is welcome to submit a proposal. Any associated funding, however, can only be hosted at US institutions.


### Am I allowed to apply for an archival proposal?

**A: Depends on the nationality of your institution**

You must be at a US institution to received funding, and thus these proposals are only eligible at US institutions.

### When am I allowed to apply for Directors Discretionary time?

**A: Any time**

Always, just ensure that the proposal is time-sensitive, and could not be submitted during the regular call or proposals.

### When I submit a proposal, what kind of budget do I have to include?

**A: A form that we can easily understand**

If your proposal requires a budget (e.g. legacy proposals), we ask that budgets be submitted in a form that we can later parse into our standard format. We acknowledge that different institutions use different formats, and allow for them so long as we can easily parse the data when creating the budget.

### My target is low in the sky, can I observe it?

**A: Maybe**

The specified limits of SOFIA observations is 21-58 degrees. Below the lower vignetting limit, the background increases significantly as the instrument starts to see some of the door. These limits are not prohibitive, but mark the point of significant loss of sensitivity.
