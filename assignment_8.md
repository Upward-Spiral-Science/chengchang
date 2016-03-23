Table of Contents:

------------------------------------------------------------------------

-   [Overview](#Overview)

-   [Scientific Questioning](#Scientific%20Questioning)
    -   [Descriptive Analysis](#Descriptive%20Analysis)
    -   [Exploratory Analysis](#Exploratory%20Analysis)
    -   [Inferential Analysis](#Inferential%20Analysis)
    -   [Predictive Analysis](#Predictive%20Analysis)
    -   [Testing Assumptions](#Testing%20Assumptions)
    -   [Next Steps](#Next%20Steps)
-   [Methods](#)
    -   [Descriptive Analysis](#Descriptive%20Analysis)
    -   [Exploratory Analysis](#Exploratory%20Analysis)
    -   [Inferential Analysis](#Inferential%20Analysis)
    -   [Predictive Analysis](#Predictive%20Analysis)
    -   [Testing Assumptions](#Testing%20Assumptions)

------------------------------------------------------------------------

Overview
--------

Systems neuroscience aims to fully map human connectome. Because of
studying neurons individually, traditional techniques cannot provide
information such as spatial distribution. Nowdays, electron microscopy
can map entire section of cortex in details. Therefore, we are able to
analyze the distribution in cortex. In this project, we have been
focusing on the density of synapses across the one specific
3D-dimensional layer of cortex.

Scientific Questioning
----------------------

Starting with descriptive analysis, here we will describe the analysis
we have been doing so far, such as preliminary analysis and assumptions
testing. Each analysis consists of two parts: results and methods
(codes), and they will be discussed seperately in the following report.

### Descriptive Analysis

In the beginning, we started from the basic descriptive analysis to get
the gist of the whole data set. This data set has only 5 attributes:
`X, Y, Z, unmasked, synapses`. We examined whether the data has invalid
values such as missing data or infinity (i.e., `NaN/NA/Inf`). Below is
the table of the result.

<table>
<thead>
<tr class="header">
<th align="center">Item</th>
<th align="center">NA</th>
<th align="center">Inf</th>
<th align="center">NaN</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="center">Numbers</td>
<td align="center">0</td>
<td align="center">0</td>
<td align="center">0</td>
</tr>
</tbody>
</table>

Then, we checked the basic descriptive statistics of the synapses,
including the number of our focus: "synapses",

<table>
<thead>
<tr class="header">
<th align="center">Item</th>
<th align="center">Max</th>
<th align="center">Median</th>
<th align="center">Mean</th>
<th align="center">SD</th>
<th align="center">Sum</th>
<th align="center">data points</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="center">Numbers</td>
<td align="center">507</td>
<td align="center">144</td>
<td align="center">124.71</td>
<td align="center">91.998</td>
<td align="center">7704178</td>
<td align="center">61776</td>
</tr>
</tbody>
</table>

Now we realized that this data set does not have any invalid value, and
has 61, 776 3D bins (i.e., number of rows). Finally, we examined the
resolution and dimension, or size, (unit: *μ**m*<sup>3</sup>) of this
scanned region in the data set.

<table>
<thead>
<tr class="header">
<th align="center">Item</th>
<th align="center">X</th>
<th align="center">Y</th>
<th align="center">Z</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="center">resolution</td>
<td align="center">3.9</td>
<td align="center">3.9</td>
<td align="center">5.55</td>
</tr>
<tr class="even">
<td align="center">dimension</td>
<td align="center">421.2</td>
<td align="center">202.8</td>
<td align="center">61.05</td>
</tr>
</tbody>
</table>

### Exploratory Analysis

It seems that the number of synapses vary greatly, which means the
synapses is not evenly distributed in this region. Therefore, we started
to look at the distribution of synapses. We tried to examine in
different views: by one dimension, two dimensions, and use histogram to
see the distribution of synapse density. The results are as below.

From these figures, we can see that there is no apperent trend of
distribution. Also, since there are many 0*s* appeapr in the data since
they are masked, we decided to filter out these 0*s* in order to make
our further analysis more accurate.

### Inferential Analysis

In order to deal with the 0*s*, we created anotehr attribute, the ratio
between synapses and unmasked, to filter out the bins with less than 0.5
and called it `weighted synapses`. By doing so, we looked at the
distribution of bins again. Here, we assumed this distribution follows
Poisson distribution. Hence, we chose the chi-square test for goodness
of fit to perform this hypothesis testing.

The result showed that the p-value is nearly 0, which strongly rejected
our assumption that the distribution of bins follows Poission
distribution. Since this test significantly rejected, we may needed to
reconsider this hypothesis and maybe try to fit with Gaussian or
log-normal distribution.

### Predictive Analysis

Since our data does not have categorical data, we created another
attribute again: the density of synapses in one Z-layer is high or low.
First, we used k-means and set the *k* = 2 to generate 2 clusters of
densities. And then we got the labels of each Z-layer. Next, we split
each Z-layer into 5 × 5 grids, and tried to use these grids to predict
whether this grid belongs to high density layer or low density layer.
After labeling the Z-layers, we then use different classifiers to train
and test our data set. The results are below,

<table>
<thead>
<tr class="header">
<th align="center">Classifier</th>
<th align="center">Accuracy</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="center">Nearest Neighbors</td>
<td align="center">0.75 (+/- 0.01)</td>
</tr>
<tr class="even">
<td align="center">Linear SVM</td>
<td align="center">0.80 (+/- 0.01)</td>
</tr>
<tr class="odd">
<td align="center">Random Forest</td>
<td align="center">0.80 (+/- 0.01)</td>
</tr>
<tr class="even">
<td align="center">Linear Discriminant Analysis</td>
<td align="center">0.80 (+/- 0.01)</td>
</tr>
<tr class="odd">
<td align="center">Quadratic Discriminant Analysis</td>
<td align="center">0.80 (+/- 0.01)</td>
</tr>
</tbody>
</table>

These classifiers have accracies between 75 to 80 percent, which is
better than change (i.e., random guessing). However, these numbers are
slightly better than just choosing the class with the maximum prior 100%
of the time under the assumption that the priors is ture. Given the
large overlap area in observed densities from two classes, if we wanted
to distinguish between similar populations of Z-layers with different
priors (i.e., another data set), then it is likely that the accuracy
will decrease.

### Testing Assumptions

In our analysis, we made a few assumptions about our data set:

-   `Unmasked` is independent of `weighted number of synapses`.  
-   `Weighted number of synapses` is independent of `X` and `Y`.
-   Bins are i.i.d.  
-   Means of grids are i.i.d.  
-   Class conditional difference between layers.

Therefore, it was time to examine our assumptions to see if they are
correct so that we may improve our model accordingly.

First, we tested the indepednce between `unmasked` and `weighted`. We
used different threshold to test the independece. As the table below
shows, it is obvious that the correlation drops as the threshold
increases. By thinking intuitively, this is not surprise because the
more masked region, the less observed synapses. Therefore if we cut out
those low threshold data, the correlation also decreases.

<table>
<thead>
<tr class="header">
<th align="center">Voxel threshold</th>
<th align="center">Correlation</th>
<th align="center">p-value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="center">0 %</td>
<td align="center">0.7495</td>
<td align="center">0.0</td>
</tr>
<tr class="even">
<td align="center">5.0 %</td>
<td align="center">0.5835</td>
<td align="center">0.0</td>
</tr>
<tr class="odd">
<td align="center">10.0 %</td>
<td align="center">0.5319</td>
<td align="center">0.0</td>
</tr>
<tr class="even">
<td align="center">20.0 %</td>
<td align="center">0.4726</td>
<td align="center">0.0</td>
</tr>
<tr class="odd">
<td align="center">30.0 %</td>
<td align="center">0.4413</td>
<td align="center">0.0</td>
</tr>
<tr class="even">
<td align="center">50.0 %</td>
<td align="center">0.3513</td>
<td align="center">0.0</td>
</tr>
<tr class="odd">
<td align="center">70.0 %</td>
<td align="center">0.0188</td>
<td align="center">0.013</td>
</tr>
</tbody>
</table>

Secondly, we examined the whether the `X` and `Y` positions are
independent with `weighted`. From the table below we realized that the
`X` positions have little correlation with `weighted`. However, the `Y`
positions slightly correlate with `weighted`, which contradicts with our
previous assumption.

<table>
<thead>
<tr class="header">
<th align="center">Position</th>
<th align="center">Correlation</th>
<th align="center">p-value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="center">X</td>
<td align="center">0.1227</td>
<td align="center">4.49e-05</td>
</tr>
<tr class="even">
<td align="center">Y</td>
<td align="center">-0.3552</td>
<td align="center">4.60e-34</td>
</tr>
</tbody>
</table>

Next, we tested whether these bins are independently sampled and
identically distributed. The figures below show the covariance matrix
and BIC score with GMM clustering. Since the content in the off-diagonal
is not 0 in the most part, it suggests that bins are not independent.
But the optimal number of cluster is 1, which suggests that they are
actually identically distributed.

\[FIGURE for bins\]

Then, we examined the iid assumption of means of grid as a whole and in
Z-layer in particular. From the figures below, we see that the ratio of
on- and off- diagonal in the covariance matrix is very small, suggesting
that they are not independent. Since the optimal number of clusters is
1, we concluded the grid means are identical.

About the Z-layers, as the ratio of on- and off-diagonal determinants is
extremely large, we can conclude that they are independent in fact. But
since the optimal number of clusters is not 1, suggesting that the
Z-layers are not identically distributed.

\[FIGURE for grid and Z\]

Finally, we tested the conditional difference between high and low
density Z-layers. From the figure below, it clearly that the classifier
cannot distinguish grids just based on their means. Therefore this
assumption is false.

\[FIGURE for conditional\]

### Next Steps

So far, we understood some properties of synapses densities in a cortex.
Next, we may need more information to get a better prediction of
classification. For example, using larger grid size may be a good way
since this can capture more information within a Z-layer. Alternatively,
using another statistics such as median or other scaled statistics
instead of using mean could be another way to investigate the data.
Finally, if we can use the functional layer, we could perform a better
prediction since a functional layer may have larger impact of number of
synapse than simply a Z-layer.

Methods
-------

The code and mathematical theory along with each questions are explained
in details, which can be found in the links below. In this section we
will discuss about the methods used in each part, including why we
perform these analysis.

<table>
<thead>
<tr class="header">
<th align="left">Question type</th>
<th align="left">Code</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Descriptive</td>
<td align="left">[Assignment 3] (<a href="https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%203.ipynb">https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%203.ipynb</a>)</td>
</tr>
<tr class="even">
<td align="left">Exploratory</td>
<td align="left">[Assignment 3] (<a href="https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%203.ipynb">https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%203.ipynb</a>)</td>
</tr>
<tr class="odd">
<td align="left">Inferential</td>
<td align="left">[Assignment 4] (<a href="https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%204.ipynb">https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%204.ipynb</a>)</td>
</tr>
<tr class="even">
<td align="left">Predictive</td>
<td align="left">[Assignment 5] (<a href="https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment.5.ipynb" class="uri">https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment.5.ipynb</a>)</td>
</tr>
<tr class="odd">
<td align="left">Testing</td>
<td align="left">[Assignment 6] (<a href="https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%206.ipynb">https://github.com/Upward-Spiral-Science/chengchang/blob/master/Assignment%206.ipynb</a>)</td>
</tr>
</tbody>
</table>

### Descriptive Analysis

Our data set has relatively simple because it consists of only 5
attributes. 3 of them are the coordinate of 3D-dimension, and `synapses`
is the main target. First step, we got the big picture of this data set
so that we could understand the existence of invalid values which we
have to find out in order not to be affected. Then the sparsity of data
and the basic descriptive statistics were computed.

### Exploratory Analysis

We answered several exploratory questions: the first one is about the
synapses distribution in different 1-dimension. We used histograms to
plot the number of synapses along `X`, `Y` and `Z` coordinate. We also
combined 3 coordinates togrether to see the synapses distribution.

Since 1-dimension histograms did not give up too much information, we
used heat map to investigate 2-dimension synpases densities. In X-Y
coordinate, it was hard to tell whether there is anything particular. In
X-Z and Y-Z coordinate, we saw the edge of `X` and `Y` layers barely
have synapses. So perhaps we have to deal with it for further analysis.
Also, since `Z` layer have only 11 different values, which was few when
compared with `X` and `Y`, we tried to focus on the difference between
each X-Y coordinate (i.e., Z-layer). Consequentrly, we ploted again in
X-Y coordinate by heat map to see the synapses densities.

### Inferential Analysis

For this part, we first re-scaled our data for `weighted synapses` since
there are too many 0*s* in the data set. These 0*s* mean that the data
point is partly censored so that it affect the detection of synapses. So
for the the better analysis, we filtered them out. Then, we defined a
model to test if this `weighted synapses` follows Poisson distribution.
We chose Poisson distribution because it is discrete distribution with
only one parameter, which to some extent reflects the fact that our data
set is also simple. For this purpose, we used chi-square test for
goodness of fit to test this hypothesis because it is perfect for single
population and categorical data (as we used bins to perform the
analysis). In order to prove that the this chi-square test can be used,
we simulated data from null model (i.e., Poisson distribution) and
alternative model (i.e., any distribution other than Poisson). Below
shows the result of this simulation, as we can see the p-value converges
to 1 when the number of sample increases in alternative model; in the
null model, it shows that the p-values stay around 0.05 which
correspondes our expectation.

### Predictive Analysis

Because our data set is not totally categorical, we made up another
variable for this analysis. According to the mean density for each
Z-layer, We labeled them with high and low density. This procedure was
done with k-mean algorithm with *k* = 2. It may be better using Jenk's
Natural Breaks to classify since this density has 1-dimension only. Then
we made the preditive question: can we predict this layer belongs to
high density or low density by a small part of this layer? In order to
do so, we split each Z-layer into 5 × 5 group and used the mean of grid
to predict. For the loss function we used the 0-1 loss function - get
the loss 1 if the labels were different; otherwise there was no loss.
Next we tested our data with 5 classifier: Linear discriminant analysis
(LDA), Quadratic discriminant analysis (QDA), k-nearest neighbor (kNN),
Support vectors machines (SVM) and Random forest (RF). Parameters are
set: 3 for kNN, 0.001 for SVM and default values except maximum depth is
5 for RF. LDA and QDA do not have to specify any parameter. Although
these parameters may make a huge impact on the results of analysis, but
for now we just go for these setting to see the performance. The picture
below shows the performances of each classifier tested on simulated data
and our real data.

### Testing Assumptions

So far we have done several analysis and these analysis have some
assumptions behind. Therefore, it is better to examine whether these
assumptions are true or not. The following are the assumptions we made:

-   `Unmasked` is independent of `weighted number of synapses`.  
-   `Weighted number of synapses` is independent of `X` and `Y`.
-   Bins are i.i.d.  
-   Means of grids are i.i.d.  
-   Class conditional difference between layers. classes have different
    covariances

For testing two objects are independet, we examined the correlation
coefficient of them. Generally speaking, if the absolute value of
coefficient is around 0.3 or below, then we may conclude that these two
variables have low correlation; a value around 0.6 or higher indicates a
high correlation. Although it is needed to notice that this procedure
check the linear correlation only. For testing independence of bins and
means, we calculated the covariance matrix and check the ratio of on-
and off- diagonal. If the ratio is large, then we may conclude that they
are not linearly independent.

For testing whether bins and means are identically distributed or not,
we computed number of cluster(s) from the data with a Gaussian Mixture
Model and used the Bayesian Information Criterion (BIC) to score with
different cluster numbers. The highest BIC score suggests the optimal
number of cluster of the data. Hence, if the highest BIC score appears
at 1 cluster, which means there does not exist two or more different
clusters, then it is likely that the data is identically distributed.

For testing class conditional covariance, we fitted the each class with
linear regression and computed the residuals. Then we plot the residual
with grid means to see if they can be seperated into two different
groups by grid means.
