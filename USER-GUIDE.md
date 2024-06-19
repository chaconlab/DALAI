
# DALAI-GA User Guide

<p>Once the program is downloaded. Now you are ready to reconstruct the shape and dimensions of your problem macromolecule. This section gives a brief overview of the necessary steps to run DALAI-GA. Nevertheless, we recommend following the tutorial with the given examples.</p>
<p>Input files</p>
<hr width="100%" /><!-- Raya fina -->
<p>The current version of the program is designed to work with three input files: </p>
<ul>
<li>Parameter File (dalai_ga.ini)</li>
<li>SAXS normalized profile (FILEINPUT)</li>
<li>Initial model search space (MODELINI, optional)</li>
</ul>
<p>All you need to do is to fill in three proper input files put them in the directory where you want to have the output and launch the program.</p>
<p>SAXS profile</p>
<p>This is the filename of your data set. i.e. the target intensity profile data set from a monodisperse solution.   The format is a two-column ASCII file.  Each line gives the S-vector modulus (defined as the reciprocal of the Bragg spacing i.e. 2 (sin(theta)/lambda, in reciprocal Anstromg units ) and Intensity values, blank separated. It must be normalized to 1.0  at S=0  (WARNING: THE POINT AT S=0 WITH I=1 SHOULD NOT BE INCLUDED). <a href="images/sbg/dalai_ga/2bb2.int">Click here </a>to get an example of a target profile used in the tutorial. If the scattering vector values are in Q units (2pi S), they must be transformed into S units. </p>
<p>Initial search space</p>
<p>An initial model composed of closely packed identical spheres, in which the GA will search for a mass distribution compatible with the target SAXS profile. This file is in PDB format with a 2-line header. <a href="images/sbg/dalai_ga/L162.pdb">Click here</a> to see an example of an ellipsoid of 150x100x100 composed of  162 beads of 7A radius. The program generates this initial search space  with the following input parameters file.</p>
<p>Parameter file</p>
<p>The input parameter file is called <a href="assets/dalai_ga.ini">dalai_ga.ini</a>  and it includes all the necessary  parameters and the input file names. The following table gives a detailed description of such parameters:  </p>
<table class="text" style="width: 100%;" border="1" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td width="10%">Parameter </td>
<td> Description</td>
<td colspan="9" align="center" valign="middle" width="10%">Example Value</td>
</tr>
<tr>
<td>FILEINPUT</td>
<td>The SAXS profiles filename. An <a href="assets/2bb2.int">example </a>is shown for the target profile used in the tutorial.</td>
<td>2bb2.int</td>
</tr>
<tr>
<td>FILEMODEL</td>
<td>The search space bead model in PDB format. Here you can find an <a href="assets/L162.pdb">example</a>. </td>
<td>L162.pdb</td>
</tr>
<tr>
<td>ELLSIZE</td>
<td>DALAI_GA can generate an initial search space, a triaxial ellipsoid with the  dimensions given in these parameters. In this case, FILEMODEL mus be NONE. </td>
<td>150 100 100</td>
</tr>
<tr>
<td>RADIUS</td>
<td>The radius of the beads that will compose your ellipsoid.  As initial guest select a size radius that produce initial models formed by less than 200-300 beads. DALAI_GA will refine the model later with smaller balls. </td>
<td>7.0</td>
</tr>
<tr>
<td>DELTA_R</td>
<td>
<p>Once a best model of a given bead size is found a new GA search with smaller beads starts over. This iterative process is repeated till 3A beads. This parameter gives a reduction of the radius for the next step. The default value is 1. </p>
</td>
<td>1.0</td>
</tr>
<tr>
<td>END_R</td>
<td>Minimal size radius used</td>
<td>3.0</td>
</tr>
<tr>
<td>MASS+/-</td>
<td>When the program starts a random initial population is generated, the maximum and minimum number of beads of the starting models is specified here.</td>
<td>40 10</td>
</tr>
<tr>
<td>RG+/- </td>
<td>The target radius of gyration and the corresponding allowed deviation only in the initial generation.</td>
<td>30 5</td>
</tr>
<tr>
<td>NL/NC</td>
<td>The speed of convergence of the algorithm, the smaller the values, the faster it converges. The larger the values the safer your results will be.</td>
<td>10 100 200 500</td>
</tr>
<tr>
<td>DISPLAY</td>
<td>Defines the level of diagnostic messages, 1 means standard verbosity mode and 0 non-messages.</td>
<td>1</td>
</tr>
</tbody>
</table>
<p> Output files</p>
<hr width="100%" />
<p>DALAI_GA will produce two output files (plus some rubbish you shouldn't care about): the structure of the best models plus the experimental and calculated SAXS profiles. They will have the name bestxxxx plus the following extensions where xxxx is the  generation number.</p>
<p>*.dat -&gt; Files with extension "data" correspond to the calculated and experimental  SAXS profiles. <br />            It has five columns, that correspond: <br />            1) S values. <br />            2) Normalized Intensities of the model. <br />            3) Experimental normalized intensities. <br />            4) Normalized intensities of the model in logarithmic scale. <br />            5) Experimental normalized intensities in logarithmic scale.  </p>
<p>*.pdb -&gt; bead model in PDB format. You can easily visualise it with rasmol (using spacefill command) or grasp (making a molecular surface).</p>
<p>Every 1000 steps the best model configurations are saved in the best_(nº generations). dat or .pdb, the current best configuration is stored in best.dat and best.pdb files. The best model obtained at a given resolution is saved as well.</p>
<p>Practical advices</p>
<hr width="100%" />
<p>1) The better the data the better the model.</p>
<p>All ab initio shape determination strongly depends on SAXS data quality, so:</p>
<ul>
<li>Repeat measurements to reduce the noise.</li>
<li>Measurements with different camera lengths are recommended to extend the maximum S vector, and therefore the resolution.</li>
<li>Accurate buffer subtraction. Make sure that the product I(S) x S^2 decays at higher angles and that I(S)x S^4 follows Porod's law</li>
<li>The data must be normalized to 1 at S=0. The use of <a href="http://www.embl-hamburg.de/ExternalInfo/Research/Saxs/index.html">GNOM</a> is recommended to obtain the value of I(0) from the SAXS profile.  </li>
<li>Errors in the normalization &amp; buffer subtraction could produce failures and strong deviations in the mass estimation.</li>
<li>SAXS at low angle is very sensitive to inhomogenities in the sample. A small part of this region may be edited if it is suspected that it has been affected by this problem, without much loss of shape information. Different experimental and model Rg values normally indicate errors in this part of the profile.</li>
THIS IS EXTREMELY IMPORTANT!!</ul>
<p>2) Reduce the computation time.</p>
<p>The first step of the search defines the initial configurational search space. As initial trial, an ellipsoid with  an hexagonal packing of beads with dimensions at least 30 A bigger than the maximal dimension of the particle, Dmax as obtained from the SAXS profile, is advisable (for Dmax, use <a href="http://www.embl-hamburg.de/ExternalInfo/Research/Sax/index.html">GNOM</a>). The size of the spheres must be compatible with the maximum scattering vector, but even a nice data set should start with relatively large spheres (the number of spheres of the initial models has to be between 20-40 for optimal results, and the size of the beads of the initial configurational space can be estimated from the Mw in dalton from figure 2 of Chacon et al. J.Mol. Biol. 299, 1289-1302, (2000) ). DALAI-GA will take the data up to this resolution and use the rest when the model is refined. Run several initial trials, with low values of NL/NC 10 100 200 500, and no more than 500 iterations of DALAI_GA. Look at the dimensions and the number of spheres of the models to check the initial search models. Good performance can be obtained if the number of beads in the search space is less than ten times the number of beads in the resulting models.</p>
<p>3) Be sure of reproducibility.</p>
<p>Once having a good initial search space, run DALAI-GA at least 10 times. The stochastic character of the method and the intrinsic degeneracy of the inverse scattering problem result to final different models. In practice, all of them should correspond to a given shape and dimensions. Be patient enough to manually superimpose the models. Enantiomorphic/symmetric solutions may appear with small bead numbers.</p>
<p>Run at least one DALAI_GA with safe NL/NC parameters 500, 1000, 1000, 2000.</p>
<p>4) Do not try too far (too small bead size)</p>
<p>Remember that the bead size must be in agreement with the S range. So, do not try too small spheres if the data does not have sufficient resolution; in this case, the final models will diverge due to overfitting.</p>
 

