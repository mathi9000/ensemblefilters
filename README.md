# Collection of ensemble square root kalman filters implemented in Python

This repository offers Python code for a variety of Ensemble Kalman Filters as presented in the overview paper by Vetra-Carvalho et al. (2018) [1]. I highly appreciate the authors effort to present a variaty data-assimilation methods using a unified mathematical notation and recommend reading the paper if you want to understand what it is all about.

For the implemntation I followed the Fortran-like pseudocode given by authors in the appendix and indicated in the comments where I deviated from it due to errors or lack of clarity.

## Content of repository:
* Folder "kalmanfilters": Separate file for each Kalman Filter
* Folder "testdata": data from a general circulation model which can be assimilated with the functions
* kalman_filters_tests-notebook: Simple scripts to check that the output of the different functions is equal (posterior mean and covariance matrix)

## Dependencies
* numpy as np
* scipy (only the direct EnSRF method, which I do not recommend to use, needs it for matrix square root calculation)
* time for speed calculation

## Input variables and dimension conventions
* Note that the observation operator  H  is only implemented implicitely in these functions, the observations from the model  Hx  need to be precalculated. The observation uncertainties are assumed to be uncorrelated, hence the matrix R is diagonal (algorithms are written for diagonal R).

**Variables**
* Xf: Prior ensemble ( N_x  *  N_e )
* HX: Observations from model ( N_y  *  N_e )
* Y: Observations ( N_y  * 1) 
* R: Observation error (uncorrelated, R is assumed diagonal) ( N_y  * 1)

**Dimensions**
*  N_e  Ensemble Size 
*  N_x  State Vector length
*  N_y  Number of measurements

## Ensemble Kalman Filters implemented so far

* EnSRF: Ensemble Square Root Filter
    * simultaneous solver
    * serialized solver
    * direct solving of K and K-tilde
* ETKF: Ensemble Transform Kalman Filter:
    * Square Root Formulation by Hunt
    * Adaptation by Livings 
* ESTKF: Error-subspace transform Kalman Filter 


## Test Data
As I work on paleoclimate DA project the test-data is from a past-millenium climate simulation. Of course you can also easily generate some random test data.

* Y: Measurements (293 * 1) (Actualized synthesized observations generated with additional noise from the prior)
* R: Measurement errors (293 * 1)
* Xf Forecast from model (55296 * 100) (The number of rows is given by the number of gridpoints of the climate model. Prior contains temperature values (K))
* HXf; Observations from model (293 * 100)

For this type of test data, the speed is dominated by the last operation (multiplication of perturbation matrix with weight matrix).

# Contact
If you find errors,ways to optimize the code etc.  feel free to open an issue or contact me via mchoblet -AT- iup.uni-heidelberg.de

# Literature
[1] Sanita Vetra-Carvalho et al. State-of-the-art stochastic data assimilation methods for high-dimensional non-Gaussian problems. Tellus A: Dynamic Meteorology and Oceanography, 70(1):1445364, 2018. https://doi.org/10.1080/16000870.2018.1445364
The authors have implemented some of the functions for the sangema project in Fortran and in julia language. I have not checked this code in detail.

https://sourceforge.net/projects/sangoma/
https://github.com/Alexander-Barth/DataAssim.jlts

