GCPDS EEGNet
============

Keras Implementation of EEGNet
------------------------------

::

   http://iopscience.iop.org/article/10.1088/1741-2552/aace8c/meta
   Note that this implements the newest version of EEGNet and NOT the earlier
   version (version v1 and v2 on arxiv). We strongly recommend using this
   architecture as it performs much better and has nicer properties than
   our earlier version. For example:
       
       1. Depthwise Convolutions to learn spatial filters within a 
       temporal convolution. The use of the depth_multiplier option maps 
       exactly to the number of spatial filters learned within a temporal
       filter. This matches the setup of algorithms like FBCSP which learn 
       spatial filters within each filter in a filter-bank. This also limits 
       the number of free parameters to fit when compared to a fully-connected
       convolution. 
       
       2. Separable Convolutions to learn how to optimally combine spatial
       filters across temporal bands. Separable Convolutions are Depthwise
       Convolutions followed by (1x1) Pointwise Convolutions. 
       

   While the original paper used Dropout, we found that SpatialDropout2D 
   sometimes produced slightly better results for classification of ERP 
   signals. However, SpatialDropout2D significantly reduced performance 
   on the Oscillatory dataset (SMR, BCI-IV Dataset 2A). We recommend using
   the default Dropout in most cases.
       
   Assumes the input signal is sampled at 128Hz. If you want to use this model
   for any other sampling rate you will need to modify the lengths of temporal
   kernels and average pooling size in blocks 1 and 2 as needed (double the 
   kernel lengths for double the sampling rate, etc). Note that we haven't 
   tested the model performance with this rule so this may not work well. 

   The model with default parameters gives the EEGNet-8,2 model as discussed
   in the paper. This model should do pretty well in general, although it is
   advised to do some model searching to get optimal performance on your
   particular dataset.
   We set F2 = F1 * D (number of input filters = number of output filters) for
   the SeparableConv2D layer. We haven't extensively tested other values of this
   parameter (say, F2 < F1 * D for compressed learning, and F2 > F1 * D for
   overcomplete). We believe the main parameters to focus on are F1 and D. 
