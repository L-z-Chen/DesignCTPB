# DesignCTPB

This is the beta version of R package for designing clinical trial with potential biomarker effect. Currently we are working on the following two tasks,\
  (1) preparing documentation for this package.\
  (2) testing this package in various environments and evalueting its consistency. Our original code was developed on Compute Canada Servers with versions of dependency listed above. 
  
For a given setting of input parameters, this package can solve up to 5-dimension alpha-split problems. This can also be expended to handle higher dimension problems. But in practice, we do not suggest consider too high dimensions, since considering too many subpopulation leads to too much loss in power, and not being the optimal choice.\
This package can also guide the choice of size of nested populations, i.e. find optimal r-values. The function visualizes and optimizes r-values, but only supports 3-dimension. The optimization of r-values in more than 3-dimension is trivial, but visualization can be too hard.

We implemented it with GPU computing and smoothing method(thin plate spline). 

## How to install in R:

devtools::install_github("ubcxzhang/DesignCTPB")

## How to run in R:

### Setting Python environment and load package
Sys.setenv(RETICULATE_PYTHON='python_path')# *<font face = "Times New Roman">Note that please specify one python version here instead of using the default r-reticulate python environment, which may encounter errors.</font>*\
library(DesignCTPB)

### Calculating optimal alpha-split for a given setting of input parameters
alpha_split(r=c(1,0.5,0.3),N1=20480,N2=10240,N3=2000,E=NULL,sig=NULL,sd_full=1/base::sqrt(20),delta=NULL,delta_linear_bd = c(0.2,0.8),Power=NULL,seed=NULL)

### Calculating optimal alpha-split for many settings of r values (i.e. size of nested subpopulations), and visualize their results and calculate optimal choice of r values
res <- design_ctpb(m=24, n_dim=3, N1=20480, N2=10240, N3=2000, E=NULL, SIGMA=NULL, sd_full=1/base::sqrt(20), DELTA=NULL, delta_linear_bd=c(0.2,0.8), seed=NULL)\
res$plot_alpha # *<font face = "Times New Roman">to see the 3-d rotatable plot of optimal alpha versus r2 and r3.</font>*\
res$plot_power # *<font face = "Times New Roman">to see the 3-d rotatable plot of optimal power versus r2 and r3.</font>*\
res$opt_r_split\
res$opt_alpha_split\
res$opt_power

**The default inputs give the results of the strong biomarker effect in our paper. Users can change the values of input parameters to generate plot and obtain the optimal alpha and r values. <!--In practice, we never design nested-population clinical trails where there is no biomarker effect. So we do not suggest users to use our settings as "the no biomarker effect" example in our paper, which is only for demostrate when no biomarker effect, our method make the correct choice of put all alpha- values on full population. -->

In our package, the user can specify the standard deviation of each population by giving SIGMA as input, and the harzard reduction rate DELTA for each population. Just give input values to SIGMA and DELTA, but note that the inputed matrix should coincides with the matrix of r-split setting. \
  *<font face = "Times New Roman">(e.g. if m=24 and n_dim=3, which means we are going to have 276 r-split setting(like our default setting), so each row of the SIGMA(DELTA) matrix should coincides with the corresponding row of r-split setting).</font>*\
For obtaining the r-split setting, user can specify it personalized or follow our r_setting(m,n_dim) function. 

#### Note for selection of N3
We are developing a better selection of N3, than presented in our paper, which should consider the proportions of each subset. This feature will be in the production version of this package.

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



