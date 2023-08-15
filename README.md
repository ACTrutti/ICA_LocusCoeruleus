# ICA_LocusCoeruleus
quick check on functional 7T  




Locus coeruleus on functional 7T NTNU data

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

Output: 
By visual inspection, IC no. 17 was identified as ‘LC component’ as it showed most overlap with the probabilistic LC atlas and additional exhibited the expected co-activation of the midbrain: 
2.08 % of explained variance; 0.64 % of total variance

LC = black outlines; 
VTA = green outlines;
 SN = blue outlines; 
Brainstem mask = white outlines 
 
MNI z = 37  
 
MNI z = 44 
 
MNI z = 41  
 
MNI z = 40  
 LC component found in both ICA’s with 30 and 50 dimensions (output components), respectively [plotted: d50 ICA]
 VTA and SN masks also overlaid to check for (dopaminergic) midbrain co-activation; seems to be present. However, as can be seen in the brainstem mask outline a substantial part of the (anterior) dopaminergic midbrain is cut-off. 

2. tSNR 
I added the probabilistic LC atlas (Ye et al., 2021 NeuroImage) to the list of ROIs we use to extract tSNR estimates to get an idea about the tSNR in the LC in comparison to the other ROIs (across all runs and subjects that we already collected). tSNR seems to be comparable to other ROI is that area.
 
![image](https://github.com/ACTrutti/ICA_LocusCoeruleus/assets/26138934/6847aff1-8702-424d-a764-5f7cbccd554a)

