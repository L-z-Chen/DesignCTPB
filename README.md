# DesignCTPB

This is the immature version of R package for designing clinical trial with potential biomarker effect, which is only temporarily workable for calculating 3-dimension r-split cases and up to 4-dimension alpha-split cases. We implemented it with GPU computing and smoothing method(thin plate spline). 

## How to install in R:

devtools::install_github("ubcxzhang/DesignCTPB")

## How to run in R:

### for r-split in 3-dimension
Sys.setenv(RETICULATE_PYTHON='python_path')# Note that please specify one python version here instead of using the default r-reticulate python environment, which may encounter errors\
library(DesignCTPB)\
res <- plot_clinical()\
res$plot_alpha # to see the 3-d rotatable plot of optimal alpha versus r2 and r3\
res$plot_power # to see the 3-d rotatable plot of optimal power versus r2 and r3\
res$opt_r_split\
res$opt_alpha_split\
res$opt_power

### for one-point alpha-split
reticulate::source_python(system.file("python","power4R.py",package="DesignCTPB")) # source python4R.py into the environment\
power <- Power_sampling\
alpha_slpit(r=c(1,0.4,0.2,0.1), sd=1/sqrt(20), N1=20480, N2=10240, N3=4000, lower_bio_eff=0.1, upper_bio_effect=0.5, power=power, seed=NULL)


## R Dependencies:

R/4.0.2\
reticulate(Package to interface python in R)\
mnormt/fields/plotly/dply

## Python Dependencies:

Python 3.6.3 or later\
numba 0.46.0 or later\
scipy/numpy/pandas

## GPU and other Dependency 
### Note that we develop our package under this cuda version, while we are still testing for other versions

gcc/7.3.0\
CUDA 9.2.148


## Notes:
This package is still under development. Currently we are working on the following two tasks,\
  (1) preparing documentation for this package.\
  (2) testing this package in various environments and evalueting its consistency. Our original code was developed on Compute Canada Servers with versions of dependency listed above. 

If any suggestions and problems encountered, please contact us by email Xuekui@UVic.ca. thanks. 
