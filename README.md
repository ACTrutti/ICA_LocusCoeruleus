# ICA_LocusCoeruleus
quick check on Locus coeruleus on functional 7T NTNU data



1. Group ICA on resting state data 
 Data:
 Functional data from two resting-state runs of the anatomical scan session [653 volumes, 15:01.14 min; TR = 1.38s, TE = 14ms, phase-encoding direction: Anterior-Posterior; Single-echo EPI sequence, voxel size: 1.5mm isotropic]

Processing: 
 1.	Frmiprep
     output group space: MNI152NLin2009cAsym 1mm (referred to as mni09c in the following)
      brings all individual rs runs in the same space 

     Structural images: 1 T1-weighted
     Functional series: 2
     Task: rs (2 runs)
     Standard output spaces: MNI152NLin2009cAsym
     Non-standard output spaces: T1w

 2.	Denoising of rs data 
     [high-pass filter, removal of motion + physiological noise]

     Using RETROICOR regressors

 3.	Registration of templates and masks to group space 
     MNI templates, brainstem, LC, VTA and SN masks 
     [warp templates to mni09c; apply warp to the particular masks]

     Using ANTS (nighres.registration embedded_antsreg)

 4.	Down sample all masks to resolution of functional runs [1.5mm]

     Using ANTS (nighres.registration embedded_antsreg)

 5.	Mask brainstem for each subject and run




Independent component analysis:
 1.	melodic ICA (fsl) with each individual rs run masked for brainstem (as in Jacobs et al., 2020)

     melodic -i rs_runs.txt -o ../ICA_output --tr=1.38 --report -d 50
     -	rs_runs.txt = contains 2 runs per subject and only includes brainstem voxels 
     -	temporal concatenation 

 2.	FWE-correction p<0.007 (p<0.01 in Jacobs et al., 2020)

     

